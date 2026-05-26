# Hookify 系统命令

## 概述

Hookify 系统用于创建和管理 hooks，防止不良行为和自动化工作流。

## 命令列表

### /hookify

**用途**: 创建 hooks 防止不良行为

**描述**: 创建自定义 hooks 来防止不希望发生的行为。

**使用场景**:
- 防止意外删除操作
- 阻止危险命令
- 自动格式化
- 质量检查

**Hook 类型**:
- **PreToolUse**: 工具执行前
- **PostToolUse**: 工具执行后
- **Stop**: 会话结束时

---

### /hookify-list

**用途**: 列出所有配置的 hookify 规则

**描述**: 显示当前所有已配置的 hookify 规则。

---

### /hookify-configure

**用途**: 交互式启用/禁用 hookify 规则

**描述**: 交互式配置 hookify 规则的启用和禁用状态。

---

### /hookify-help

**用途**: Hookify 系统帮助

**描述**: 获取 Hookify 系统的详细帮助信息。

---

## Hook 配置示例

```json
{
  "matcher": { "pattern": "Bash|Edit|Write" },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Running quality check'"
    }
  ]
}
```

---

## 相关命令

- `/hookify` - 创建 hook
- `/hookify-list` - 列出规则
- `/hookify-configure` - 配置规则
- `/hookify-help` - 获取帮助
