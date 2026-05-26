# Hookify 系统命令

## Genel Bakış

Hookify 系统Kullanılan创建和Yönet hooks，防止不良行为和Otomatik İş Akışı。

## 命令列表

### /hookify

**Kullanım**: 创建 hooks 防止不良行为

**Tanım**: 创建自定义 hooks 来防止不希望发生的行为。

**Kullanım Senaryosu**:
- 防止意外删除操作
- 阻止危险命令
- 自动格式化
- 质量检查

**Hook Türleri**:
- **PreToolUse**: 工具执行前
- **PostToolUse**: 工具执行后
- **Stop**: 会话结束时

---

### /hookify-list

**Kullanım**: 列出所有配置的 hookify 规则

**Tanım**: 显示当前所有已配置的 hookify 规则。

---

### /hookify-configure

**Kullanım**: 交互式启用/禁用 hookify 规则

**Tanım**: 交互式配置 hookify 规则的启用和禁用状态。

---

### /hookify-help

**Kullanım**: Hookify 系统帮助

**Tanım**: 获取 Hookify 系统的详细帮助信息。

---

## Hook 配置Örnek

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
