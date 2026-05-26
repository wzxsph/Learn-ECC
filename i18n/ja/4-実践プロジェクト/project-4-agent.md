# Project 4: Agent System

マルチエージェントコラボレーションプラットフォームを構築します。

## Project Goal

**Estimated time**: 2-3 hours

Create a multi-Agent collaboration system where multiple specialized Agents work together to complete complex tasks.

## Feature Requirements

### 1. Agent Roles
- **architect** - System design
- **developer** - Code implementation
- **code-reviewer** - Code review
- **tester** - Test engineer
> Reference: [Code Review Agents](../参考ドキュメント/agents/1.%20代码审查类.md) | [Architecture Agents](../参考ドキュメント/agents/6.%20架构类.md)

### 2. Collaboration Mechanism
- Task distribution
- Result aggregation
- Conflict resolution
- State synchronization

### 3. Management Features
- Agent lifecycle management
- Task queue
- Performance monitoring
- Resource scheduling

## Technical Solution

### Message Protocol

```json
{
  "type": "task",
  "from": "architect",
  "to": "developer",
  "payload": { "task": "implement-auth", "context": {} }
}
```

### Task State Machine
- `pending` - Waiting for assignment
- `running` - Executing
- `completed` - Completed
- `failed` - Failed
- `blocked` - Blocked

## Acceptance Criteria

- [ ] Support 4+ Agent roles
- [ ] Automatic task distribution
- [ ] Correct result aggregation
- [ ] Failure recovery mechanism
- [ ] Real-time state monitoring

---

[Return to practical projects directory](./README.md)
