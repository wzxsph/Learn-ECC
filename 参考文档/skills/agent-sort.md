---
name: agent-sort
description: 通过并行仓库感知审查，将技能、命令、规则、钩子和额外组件分类为 DAILY 和 LIBRARY 两个桶，构建基于证据的 ECC 安装计划。当需要将 ECC 精简为项目实际需要的内容而非加载完整捆绑包时使用。
origin: ECC
---

# Agent Sort（智能分类）

当一个仓库需要项目专属的 ECC 配置而非默认的完整安装时，使用此技能。

目标不是猜测什么"感觉有用"，而是用实际代码库的证据对 ECC 组件进行分类。

## 适用场景

- 项目只需要 ECC 的一个子集，完整安装太臃肿
- 仓库技术栈清晰，但没人想一个一个手动精选技能
- 团队希望获得可重复的安装决策，基于 grep 证据而非主观意见
- 需要将始终加载的日常工作区与可搜索的库/参考区分离
- 仓库混入了错误的语言、规则或钩子集，需要清理

## 不可违背的原则

- 使用当前仓库作为真相来源，而非通用偏好
- 每个 DAILY 决策必须引用具体的仓库证据
- LIBRARY 不意味着"删除"，而意味着"保持可访问但默认不加载"
- 不安装当前仓库无法使用的钩子、规则或脚本
- 优先使用 ECC 原生界面，不引入第二套安装系统

## 输出产物

按顺序生成以下产物：

1. DAILY 清单
2. LIBRARY 清单
3. 安装计划
4. 验证报告
5. 可选的 `skill-library` 路由（如果项目需要）

## 分类模型

仅使用两个桶：

- `DAILY`（日常）
  - 此仓库的每个会话都应加载
  - 与仓库的语言、框架、工作流或操作界面高度匹配
- `LIBRARY`（库）
  - 有用的保留项，但不值得默认加载
  - 应可通过搜索、路由技能或选择性手动使用来访问

## 证据来源

在做出任何分类之前，使用仓库本地证据：

- 文件扩展名
- 包管理器和锁文件
- 框架配置
- CI 和钩子配置
- 构建/测试脚本
- 导入语句和依赖清单
- 仓库文档中明确描述技术栈的部分

常用命令包括：

```bash
rg --files
rg -n "typescript|react|next|supabase|django|spring|flutter|swift"
cat package.json
cat pyproject.toml
cat Cargo.toml
cat pubspec.yaml
cat go.mod
```

## 并行审查通道

如果有并行子代理可用，将审查拆分为以下通道：

1. **Agents（代理）**
   - 对 `agents/*` 进行分类
2. **Skills（技能）**
   - 对 `skills/*` 进行分类
3. **Commands（命令）**
   - 对 `commands/*` 进行分类
4. **Rules（规则）**
   - 对 `rules/*` 进行分类
5. **Hooks 和 Scripts（钩子与脚本）**
   - 对钩子表面、MCP 健康检查、辅助脚本和操作系统兼容性进行分类
6. **Extras（额外组件）**
   - 对上下文、示例、MCP 配置、模板和指导文档进行分类

如果没有并行子代理，按相同顺序逐个运行这些通道。

## 核心工作流

### 1. 读取仓库

在对任何内容进行分类之前，确定真实的技术栈：

- 正在使用的语言
- 正在使用的框架
- 主要包管理器
- 测试栈
- lint/格式化栈
- 部署/运行时界面
- 已有的操作集成

### 2. 构建证据表

对每个候选表面记录：

- 组件路径
- 组件类型
- 建议的桶
- 仓库证据
- 简短理由

使用此格式：

```text
skills/frontend-patterns | skill | DAILY | 84 个 .tsx 文件，存在 next.config.ts | 核心前端栈
skills/django-patterns   | skill | LIBRARY | 无 .py 文件，无 pyproject.toml       | 此仓库未激活
rules/typescript/*       | rules | DAILY | package.json + tsconfig.json            | 活跃的 TS 仓库
rules/python/*           | rules | LIBRARY | 零个 Python 源文件                   | 仅保持可访问
```

### 3. 决定 DAILY vs LIBRARY

晋升到 `DAILY` 当：

- 仓库明确使用匹配的技术栈
- 组件足够通用，能帮助每个会话
- 仓库已依赖相应的运行时或工作流

降级到 `LIBRARY` 当：

- 组件不在技术栈上
- 仓库可能以后需要，但不是每天都需要
- 它增加了上下文开销而没有直接相关性

### 4. 构建安装计划

将分类转化为行动：

- DAILY skills -> 安装或保留在 `.claude/skills/`
- DAILY commands -> 仅在仍然有用时保留为显式填充程序
- DAILY rules -> 仅安装匹配的语言集
- DAILY hooks/scripts -> 仅保留兼容的
- LIBRARY 表面 -> 通过搜索或 `skill-library` 保持可访问

如果仓库已使用选择性安装，更新该计划而非创建另一套系统。

### 5. 创建可选的库路由

如果项目需要可搜索的库表面，创建：

- `.claude/skills/skill-library/SKILL.md`

该路由应包含：

- DAILY vs LIBRARY 的简短解释
- 分组的触发关键词
- 库引用所在位置

不要在路由中复制每个技能的主体内容。

### 6. 验证结果

计划应用后，验证：

- 每个 DAILY 文件存在于预期位置
- 过时的语言规则未被保留为活跃
- 不兼容的钩子未被安装
- 生成的安装配置实际匹配仓库技术栈

返回紧凑报告，包含：

- DAILY 数量
- LIBRARY 数量
- 已移除的过时表面
- 待解决的问题

## 交接

如果下一步是交互式安装或修复，转交给：

- `configure-ecc`

如果下一步是重叠清理或目录审查，转交给：

- `skill-stocktake`

如果下一步是更广泛的上下文精简，转交给：

- `strategic-compact`

## 输出格式

按此顺序返回结果：

```text
STACK（技术栈）
- 语言/框架/运行时总结

DAILY（日常）
- 始终加载的项及证据

LIBRARY（库）
- 可搜索/参考的项及证据

INSTALL PLAN（安装计划）
- 应安装、移除或路由的内容

VERIFICATION（验证）
- 已运行的检查和剩余差距
```