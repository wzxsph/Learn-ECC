# Hooks定制Exercícios

## 练习目标

掌握 Claude Code 钩子的创建、配置和调试。

## 练习内容

### 练习 1：创建 PreToolUse 钩子
实现钩子功能：
- 在工具执行前记录调用参数
- 支持白名单/黑名单过滤

### 练习 2：创建 PostToolUse 钩子
实现钩子功能：
- 在工具执行后格式化输出
- 记录执行时间

### 练习 3：配置异步钩子
创建异步钩子处理：
- 外部 API 调用
- 文件系统监控

## 钩子模板

```json
{
  "name": "my-hook",
  "matcher": {
    "tool": "Bash",
    "path": "src/**/*.js"
  },
  "hooks": [
    {
      "type": "PostToolUse",
      "command": "echo 'Tool executed: ${tool_name}'"
    }
  ]
}
```

## 验证
1. 触发条件场景
2. 检查钩子输出
3. 验证日志记录
