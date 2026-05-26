# 项目四：Agent 系统

构建多 Agent 协作平台。

## 项目目标

**预计时长**: 2-3 小时

创建一个多 Agent 协作系统，多个专业 Agent 协同完成复杂任务。

## 功能需求

### 1. Agent 角色
- **architect** - 系统设计
- **developer** - 代码实现
- **code-reviewer** - 代码审查
- **tester** - 测试工程师
> 参考: [代码审查类 Agent](../DocumentosDeReferência/agents/1.%20代码审查类.md) | [架构类 Agent](../DocumentosDeReferência/agents/6.%20架构类.md)

### 2. 协作机制
- 任务分发
- 结果汇总
- 冲突解决
- 状态同步

### 3. 管理功能
- Agent 生命周期管理
- 任务队列
- 性能监控
- 资源调度

## 技术方案

### 消息协议

```json
{
  "type": "task",
  "from": "architect",
  "to": "developer",
  "payload": { "task": "implement-auth", "context": {} }
}
```

### 任务状态机
- `pending` - 等待分配
- `running` - 执行中
- `completed` - 已完成
- `failed` - 失败
- `blocked` - 阻塞

## 验收标准

- [ ] 支持 4+ 种 Agent 角色
- [ ] 任务自动分发
- [ ] 结果正确汇总
- [ ] 失败恢复机制
- [ ] 实时状态监控

---

[返回实战项目目录](./README.md)
