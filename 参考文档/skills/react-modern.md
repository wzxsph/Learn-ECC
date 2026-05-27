---
name: react-modern
description: React 现代模式 — Hooks、Server Components、Suspense、Streaming SSR、Directives 和最佳实践。
origin: ECC
---

# React 现代模式

React 18+ 核心模式快速参考：Hooks、Server Components、Suspense、流式 SSR、指令和新最佳实践。

## 何时激活

- 使用 React 18+ 新特性（Concurrent Features、Server Components、Actions）
- 构建需要流式渲染或 Suspense 的应用
- 使用新的表单处理模式（useActionState、useFormStatus）
- 理解 React 19+ 指令（use client、use server）
- 排查 React 性能问题或不必要的重新渲染

## 快速参考

### 核心 Hooks（必须掌握）

| Hook | 用途 | 关键点 |
|------|-----|--------|
| `useState` | 局部状态 | 返回 `[state, setState]` 元组 |
| `useEffect` | 副作用 | 依赖数组决定何时运行 |
| `useCallback` / `useMemo` | 记忆化 | 避免不必要计算 |
| `useRef` | 引用 DOM 或可变值 | `.current` 可变，不触发重渲染 |
| `useContext` | 跨组件传值 | 可能导致不必要的重渲染 |

### 高级 Hooks 组合

```jsx
// 1. 数据获取 — use + fetch
function usePost(id) {
  const [post, setPost] = useState(null);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch(`/api/posts/${id}`)
      .then(res => res.json())
      .then(setPost)
      .catch(setError);
  }, [id]);

  return { post, error, loading: !post && !error };
}

// 2. 订阅模式 — 清理函数
useEffect(() => {
  const sub = api.subscribe(handleData);
  return () => sub.unsubscribe(); // 清理！
}, [handleData]);

// 3. 状态从哪里来
// 来自父组件？ → props
// 全局共享？ → Context 或状态管理
// 服务端数据？ → Server Component + streaming
// 表单本地状态？ → useState 或 useFormState
```

### Server Components vs Client Components

```
Server Components (默认):
- 在服务端渲染，可以直接访问数据库
- 不能使用 useState、useEffect、event handlers
- 适合：数据获取、静态内容、文件 IO

Client Components (用 'use client' 标记):
- 在客户端渲染，可以交互
- 不能访问服务端资源
- 适合：事件处理、状态管理、浏览器 API
```

```jsx
// app/page.tsx (Server Component — 默认)
async function Page() {
  const data = await db.query('SELECT * FROM posts'); // 直接访问 DB
  return <PostList posts={data} />;
}

// components/Button.tsx
'use client'; // 标记为客户端组件
export function Button() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

### Suspense 与 Streaming

```jsx
import { Suspense } from 'react';

// 流式数据获取 — 边加载边显示
function Page() {
  return (
    <div>
      <h1>My Page</h1>
      <Suspense fallback={<Spinner />}>
        <HeavyComponent />
      </Suspense>
    </div>
  );
}

// HeavyComponent 可以是 async Server Component
async function HeavyComponent() {
  const data = await fetchLargeData(); // 慢查询
  return <DataDisplay data={data} />;
}
```

### React 19+ 表单处理

```jsx
import { useActionState } from 'react'; // 或 react-dom 的 useFormState

async function submitAction(prevState, formData) {
  const result = await saveToDatabase(formData);
  return result;
}

function MyForm() {
  const [state, submitAction, pending] = useActionState(submitAction, null);

  return (
    <form action={submitAction}>
      <input name="title" />
      <button type="submit" disabled={pending}>
        {pending ? 'Saving...' : 'Save'}
      </button>
      {state?.error && <p>{state.error}</p>}
    </form>
  );
}
```

### React 19 指令

```jsx
// 指令：'use client' 和 'use server'
// 'use server' 在服务端运行，支持 async
'use server';
async function serverAction(formData) {
  'use server';
  // 直接访问 DB、文件等
  return await db.create(formData);
}
```

## 常见模式

### 1. 组件设计原则

```jsx
// ❌ 错误：职责不清，太多状态
function BadComponent() {
  const [data, setData] = useState();
  const [loading, setLoading] = useState();
  const [error, setError] = useState();
  const [filter, setFilter] = useState();
  const [sort, setSort] = useState();
  // ... 200 行后
}

// ✅ 正确：拆分职责，小组件
function DataList({ data }) {
  return data.map(item => <ListItem key={item.id} item={item} />);
}

function ListItem({ item }) {
  return <div>{item.name}</div>;
}

function DataListContainer() {
  const { data, loading, error } = useData();
  if (error) return <ErrorBoundary error={error} />;
  if (loading) return <Skeleton />;
  return <DataList data={data} />;
}
```

### 2. 状态从哪里来

| 问题 | 解决方案 |
|------|----------|
| 状态只在当前组件使用 | `useState` |
| 状态需要传递给多个子组件 | 提升到父组件或用 Context |
| 多个不相关的组件需要同一状态 | Context 或状态管理库 |
| 服务端数据，渲染时获取 | Server Component + Suspense |
| 表单输入状态 | 受控组件 + `useFormState` |

### 3. 性能优化 Checklist

- [ ] 列表渲染有 `key` prop
- [ ] 避免内联对象/函数作为 props：`<Component onClick={() => handle()} />`
- [ ] 大列表考虑 `react-window` 或虚拟滚动
- [ ] 昂贵计算用 `useMemo`
- [ ] 回调函数用 `useCallback`
- [ ] 不必要的 Context 重渲染：用 `useMemo` 包裹 value

## 反模式

| 反模式 | 问题 | 解决 |
|--------|------|------|
| 内联函数/对象作 prop | 子组件每次都重渲染 | 用 `useCallback`/`useMemo` |
| 在 useEffect 里直接使用 async | effect 不会等待 | 提取到独立函数 |
| 用 `index` 作为列表 key | 顺序变化时 diff 出错 | 用唯一 ID |
| 过度抽象 | 难以追踪数据流 | 保持简单，按需抽象 |
| 缺少错误边界 | 错误会崩溃整个应用 | 添加 ErrorBoundary |

## 相关

- 技能: `frontend-patterns` — 前端模式参考
- 技能: `typescript` — TypeScript 最佳实践
- 技能: `vite-patterns` — 构建工具模式
- 智能体: `code-reviewer` — 代码审查工作流