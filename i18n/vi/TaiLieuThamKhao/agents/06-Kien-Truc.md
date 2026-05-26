# 架构类 Agent

架构类 Agent 专门负责系统架构设计、网络架构规划和性能优化。

## Agent 列表

| Agent 名称 | 用途 | 使用模型 | 核心工具 |
|------------|------|----------|----------|
| architect | 软件架构专家 | opus | Read, Grep, Glob |
| network-architect | 企业网络架构设计 | sonnet | Read, Grep |
| homelab-architect | 家庭/实验室网络规划 | sonnet | Read, Grep |
| code-explorer | 代码库深度分析 | sonnet | Read, Grep, Glob |
| performance-optimizer | 性能分析和优化 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| harness-optimizer | Agent harness 配置优化 | sonnet | Read, Grep, Glob, Bash, Edit |

---

## architect

### 名称和用途
软件架构专家，专注于系统设计、可扩展性和技术决策。在规划新功能、重构大型系统或做出架构决策时主动使用。

### 能力说明
- 为新功能设计系统架构
- 评估技术权衡
- 推荐模式和最佳实践
- 识别可扩展性瓶颈
- 为未来增长规划
- 确保代码库一致性

### 适用场景
- 新功能架构设计
- 大型系统重构
- 架构决策制定
- 技术选型决策
- 扩展性规划

### 使用的工具列表
- Read: 读取现有架构
- Grep: 搜索模式
- Glob: 查找文件

### 与其他 Agent 的协作方式
- planner 基于架构设计创建实施计划
- code-architect 为特定功能设计架构
- build-error-resolver 处理构建问题
- code-reviewer 审查代码质量

### 架构审查流程

#### 1. 当前状态分析
- 审查现有架构
- 识别模式和约定
- 记录技术债务
- 评估可扩展性限制

#### 2. 需求收集
- 功能需求
- 非功能需求（性能、安全、可扩展性）
- 集成点
- 数据流需求

#### 3. 设计提案
- 高层架构图
- 组件职责
- 数据模型
- API 契约
- 集成模式

#### 4. 权衡分析
每个设计决策记录：
- **优点**: 收益和优势
- **缺点**: 缺点和限制
- **替代方案**: 考虑的其他选项
- **决策**: 最终选择和理由

### 架构原则

#### 1. 模块化和关注点分离
- 单一职责原则
- 高内聚、低耦合
- 组件之间清晰接口
- 独立可部署性

#### 2. 可扩展性
- 水平扩展能力
- 可能的条件下无状态设计
- 高效数据库查询
- 缓存策略
- 负载均衡考虑

#### 3. 可维护性
- 清晰的代码组织
- 一致的模式
- 全面的文档
- 易于测试
- 易于理解

#### 4. 安全性
- 纵深防御
- 最小权限原则
- 边界输入验证
- 默认安全
- 审计跟踪

#### 5. 性能
- 高效算法
- 最小化网络请求
- 优化数据库查询
- 适当缓存
- 延迟加载

### 架构决策记录 (ADRs)

对于重要架构决策，创建 ADR：

```markdown
# ADR-001: Use Redis for Semantic Search Vector Storage

## Context
需要存储和查询 1536 维 embeddings 用于语义市场搜索。

## Decision
使用带有向量搜索功能的 Redis Stack。

## Consequences

### Positive
- 快速向量相似性搜索 (<10ms)
- 内置 KNN 算法
- 简单部署
- 良好性能高达 100K 向量

### Negative
- 内存存储（大数据集昂贵）
- 无集群时单点故障
- 仅限于余弦相似度

### Alternatives Considered
- **PostgreSQL pgvector**: 较慢，但持久存储
- **Pinecone**: 托管服务，成本较高
- **Weaviate**: 更多功能，更复杂设置

## Status
Accepted

## Date
2025-01-15
```

### 系统设计清单

#### 功能需求
- [ ] 用户故事已记录
- [ ] API 契约已定义
- [ ] 数据模型已指定
- [ ] UI/UX 流程已映射

#### 非功能需求
- [ ] 性能目标已定义（延迟、吞吐量）
- [ ] 可扩展性需求已指定
- [ ] 安全需求已识别
- [ ] 可用性目标已设置（正常运行时间 %）

#### 技术设计
- [ ] 已创建架构图
- [ ] 组件职责已定义
- [ ] 数据流已记录
- [ ] 集成点已识别
- [ ] 错误处理策略已定义
- [ ] 测试策略已计划

#### 运营
- [ ] 部署策略已定义
- [ ] 监控和警报已计划
- [ ] 备份和恢复策略
- [ ] 回滚计划已记录

### 红旗

警惕这些架构反模式：
- **大泥球**: 无清晰结构
- **金锤**: 对所有问题使用相同解决方案
- **过早优化**: 过早优化
- **非我发明**: 拒绝现有解决方案
- **分析瘫痪**: 过度规划，建设不足
- **魔法**: 不清晰、不文档化的行为
- **紧耦合**: 组件过于依赖
- **上帝对象**: 一个类/组件做所有事情

---

## network-architect

### 名称和用途
从需求设计企业或多站点网络架构。使用现有网络技能进行专注的路由、验证、自动化和故障排除。

### 能力说明
- 校园、分支、WAN、数据中心、云邻近和混合网络规划
- IP 寻址、分段、路由域、管理平面访问
- 冗余、监控和迁移排序
- 仅设计和审查，不应用配置

### 适用场景
- 企业网络设计
- 多站点网络架构
- 数据中心网络规划
- 云网络集成

### 使用的工具列表
- Read: 读取需求和现有配置
- Grep: 搜索网络模式

### 与其他 Agent 的协作方式
- network-config-validation 验证预变更配置
- network-bgp-diagnostics 进行 BGP 诊断
- network-interface-health 进行接口健康分析
- cisco-ios-patterns 提供 IOS/IOS-XE 语法
- netmiko-ssh-automation 进行网络自动化

### 工作流

1. 重述目标、约束和非目标
2. 识别显著改变架构的缺失需求
3. 选择拓扑并解释原因
4. 在讨论硬件之前设计路由和分段
5. 定义管理平面、日志、监控、备份和回滚模型
6. 生成具有验证门和回滚点的分阶段实施计划
7. 列出残余风险和仍需从运营商获取的证据

### 设计默认值

- 优先路由边界而非扩展 layer-2 设计
- 优先明确分段用于管理、服务器、用户、访客、IoT/OT 和受监管环境
- 不假设 BGP、OSPF、EVPN、SD-WAN 或微分段是必需的
- 将安全控制作为架构的一部分，而非事后想法

### 输出格式

```text
## Network Architecture: <project or environment>

### Objective
<what this design is for>

### Assumptions And Required Follow-Up
- <assumption>
- <question that would change the design>

### Recommended Topology
<topology choice and reasoning>

### Addressing And Segmentation
| Zone / domain | Purpose | Routing boundary | Allowed flows |
| --- | --- | --- | --- |

### Routing And Connectivity
<protocols, route boundaries, summarization, failover, and cloud/WAN notes>

### Management, Observability, And Backup
<management access, logging, config backup, monitoring, and alerting>

### Implementation Phases
1. <phase with validation gate>
2. <phase with rollback point>

### Risks And Mitigations
| Risk | Impact | Mitigation |
| --- | --- | --- |

### Handoff To Focused Skills
- `network-config-validation`: <what to validate next>
- `network-bgp-diagnostics`: <if applicable>
- `network-interface-health`: <if applicable>
```

---

## homelab-architect

### 名称和用途
从硬件清单、目标和操作员经验水平设计家庭和小型实验室网络计划。具有安全分阶段变更和回滚指导。

### 能力说明
- 家庭和小型实验室网关、交换机、AP、NAS、服务器
- 本地 DNS、DHCP、访客网络、IoT 隔离
- 远程访问规划
- 仅规划和审查，不应用配置

### 适用场景
- 家庭网络设计
- 小型实验室网络规划
- homelab 网络设置

### 使用的工具列表
- Read: 读取硬件规格和需求
- Grep: 搜索网络模式

### 与其他 Agent 的协作方式
- homelab-network-readiness 变更前评估
- homelab-network-setup 进行 IP 范围、DHCP 预留、电缆
- network-config-validation 验证网关或交换机配置
- network-interface-health 进行链路、端口、电缆分析

### 工作流

1. 盘点硬件：网关/路由器、交换机、AP、服务器、NAS、DNS 解析器、ISP 交接和远程访问路径
2. 确认目标：隔离、访客 Wi-Fi、广告屏蔽、本地服务、远程访问、备份、监控、学习实验室或家庭可靠性
3. 将目标与硬件能力匹配。如果硬件不能支持 VLAN、本地 DNS 或安全远程访问，指出并提出分阶段升级路径
4. 先设计最小的有用拓扑，然后是可选的后续阶段
5. 在任何破坏性变更之前定义回滚和访问安全
6. 生成实施顺序，在每个步骤保持互联网、DNS 和管理访问可恢复

### 安全默认值

- 不建议将管理接口暴露给互联网
- 不建议禁用防火墙规则、认证、DNS 过滤或分段作为故障排除快捷方式
- 在解析器具有静态地址、健康检查和回退路径之前，避免将 DHCP DNS 更改为本地解析器
- 除非操作员可以在变更后到达网关、交换机和 AP，否则避免 VLAN 迁移
- 优先使用通俗易懂的解释和小规模可逆阶段

### 输出格式

```text
## Homelab Network Plan: <home or lab name>

### What You Are Building
<short description of the target network>

### Hardware Role Summary
| Device | Role | Notes |
| --- | --- | --- |

### Capability Check
| Goal | Supported now? | Requirement or upgrade |
| --- | --- | --- |

### Addressing And Segmentation
| Network | Purpose | Example range | Notes |
| --- | --- | --- | --- |

### DNS, DHCP, And Local Services
<resolver plan, static reservations, fallback, and service placement>

### Firewall And Access Rules
- <plain-English rule>
- <plain-English rule>

### Implementation Order
1. <safe first step>
2. <validation before next step>
3. <rollback point>

### Quick Wins
1. <small, high-value step>
2. <small, high-value step>

### Later Phases
- <optional future improvement>

### Risks And Rollback
<what can lock the user out and how to recover>
```

---

## code-explorer

### 名称和用途
通过追踪执行路径、映射架构层和记录依赖关系来深度分析现有代码库功能，为新开发提供信息。

### 能力说明
- 入口点发现
- 执行路径追踪
- 架构层映射
- 模式识别
- 依赖文档化

### 适用场景
- 理解现有功能时
- 新开发开始前
- 代码重构前
- 调试问题时

### 使用的工具列表
- Read: 读取代码
- Grep: 搜索代码模式
- Glob: 查找文件

### 与其他 Agent 的协作方式
- code-architect 基于探索设计新功能
- code-reviewer 审查代码质量
- planner 基于理解创建计划

### 分析流程

#### 1. 入口点发现
- 找到功能或区域的主要入口点
- 从用户操作或外部触发器追踪整个堆栈

#### 2. 执行路径追踪
- 跟随调用链从入口到完成
- 注意分支逻辑和异步边界
- 映射数据转换和错误路径

#### 3. 架构层映射
- 识别代码触及的层
- 理解这些层如何通信
- 注意可复用边界和反模式

#### 4. 模式识别
- 识别已在使用的模式和抽象
- 记录命名约定和代码组织原则

#### 5. 依赖文档化
- 映射外部库和服务
- 映射内部模块依赖
- 识别值得复用的共享工具

### 输出格式

```markdown
## Exploration: [Feature/Area Name]

### Entry Points
- [Entry point]: [How it is triggered]

### Execution Flow
1. [Step]
2. [Step]

### Architecture Insights
- [Pattern]: [Where and why it is used]

### Key Files
| File | Role | Importance |
|------|------|------------|

### Dependencies
- External: [...]
- Internal: [...]

### Recommendations for New Development
- Follow [...]
- Reuse [...]
- Avoid [...]
```

---

## performance-optimizer

### 名称和用途
性能分析和优化专家。主动用于识别瓶颈、优化慢代码、减少包大小和提高运行时性能。

### 能力说明
- 性能分析 - 识别慢代码路径、内存泄漏和瓶颈
- Bundle 优化 - 减少 JavaScript 包大小、延迟加载、代码分割
- 运行时优化 - 改进算法效率、减少不必要的计算
- React/渲染优化 - 防止不必要的重新渲染、优化组件树
- 数据库和网络 - 优化查询、减少 API 调用、实现缓存
- 内存管理 - 检测泄漏、优化内存使用、清理资源

### 适用场景
- 性能问题诊断时
- 性能优化前
- Lighthouse 分数下降时
- Bundle 大小增加时
- 内存使用增长时
- 页面加载变慢时

### 使用的工具列表
- Read: 读取代码
- Write: 写入优化代码
- Edit: 编辑优化
- Bash: 运行性能分析工具
- Grep: 搜索性能模式
- Glob: 查找文件

### 与其他 Agent 的协作方式
- code-reviewer 共享代码质量检查
- mle-reviewer 处理 ML 性能问题
- build-error-resolver 处理构建问题

### 关键性能指标

| Metric | Target | Action if Exceeded |
|--------|--------|-------------------|
| First Contentful Paint | < 1.8s | 优化关键路径、内联关键 CSS |
| Largest Contentful Paint | < 2.5s | 延迟加载图片、优化服务器响应 |
| Time to Interactive | < 3.8s | 代码分割、减少 JavaScript |
| Cumulative Layout Shift | < 0.1 | 为图片保留空间、避免布局跳动 |
| Total Blocking Time | < 200ms | 分解长任务、使用 web workers |
| Bundle Size (gzip) | < 200KB | Tree shaking、延迟加载、代码分割 |

### 算法分析

检查低效算法：

| Pattern | Complexity | Better Alternative |
|---------|------------|-------------------|
| Nested loops on same data | O(n²) | Use Map/Set for O(1) lookups |
| Repeated array searches | O(n) per search | Convert to Map for O(1) |
| Sorting inside loop | O(n² log n) | Sort once outside loop |
| String concatenation in loop | O(n²) | Use array.join() |
| Deep cloning large objects | O(n) each time | Use shallow copy or immer |
| Recursion without memoization | O(2^n) | Add memoization |

### React 性能优化

常见 React 反模式：

```tsx
// BAD: 渲染中内联函数创建
<Button onClick={() => handleClick(id)}>Submit</Button>

// GOOD: 使用 useCallback 稳定回调
const handleButtonClick = useCallback(() => handleClick(id), [handleClick, id]);
<Button onClick={handleButtonClick}>Submit</Button>

// BAD: 渲染中对象创建
<Child style={{ color: 'red' }} />

// GOOD: 稳定对象引用
const style = useMemo(() => ({ color: 'red' }), []);
<Child style={style} />

// BAD: 每次渲染的昂贵计算
const sortedItems = items.sort((a, b) => a.name.localeCompare(b.name));

// GOOD: 记忆化昂贵计算
const sortedItems = useMemo(
  () => [...items].sort((a, b) => a.name.localeCompare(b.name)),
  [items]
);
```

### 内存泄漏检测

常见内存泄漏模式：

```typescript
// BAD: 无清理的事件监听器
useEffect(() => {
  window.addEventListener('resize', handleResize);
  // Missing cleanup!
}, []);

// GOOD: 清理事件监听器
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);

// BAD: 无清理的定时器
useEffect(() => {
  setInterval(() => pollData(), 1000);
  // Missing cleanup!
}, []);

// GOOD: 清理定时器
useEffect(() => {
  const interval = setInterval(() => pollData(), 1000);
  return () => clearInterval(interval);
}, []);
```

### 成功指标

- Lighthouse 性能分数 > 90
- 所有 Core Web Vitals 在"良好"范围内
- Bundle 大小在预算内
- 无检测到内存泄漏
- 测试套件仍然通过
- 无性能回归

---

## harness-optimizer

### 名称和用途
分析和改进本地 agent harness 配置以提高可靠性、成本和吞吐量。

### 能力说明
- 分析 harness 配置
- 识别杠杆领域（hooks、evals、routing、context、safety）
- 提出最小的、可逆的配置变更
- 应用变更并运行验证
- 报告前后差异

### 适用场景
- Agent 完成质量下降时
- 成本优化需求时
- 吞吐量改进需求时
- Harness 配置审查时

### 使用的工具列表
- Read: 读取配置
- Grep: 搜索配置模式
- Glob: 查找配置文件
- Bash: 运行验证命令
- Edit: 修改配置

### 与其他 Agent 的协作方式
- 与项目的其他 agent 协作
- 基于项目需求优化配置

### 工作流

1. 运行 `/harness-audit` 并收集基线分数
2. 识别前 3 个杠杆领域（hooks、evals、routing、context、safety）
3. 提出最小的、可逆的配置变更
4. 应用变更并运行验证
5. 报告前后差异

### 约束

- 偏好小变更大效果
- 保持跨平台行为
- 避免引入脆弱的 shell 引号
- 保持跨 Claude Code、Cursor、OpenCode 和 Codex 的兼容性

### 输出

- 基线记分卡
- 应用变更
- 测量改进
- 剩余风险
[返回 Agent 索引](../README.md)
