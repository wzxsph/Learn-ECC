# Hooks ÖzelleştirmeAlıştırma

## AlıştırmaHedef

掌握 Claude Code 钩子的创建、配置和调试。

## Alıştırma内容

### Alıştırma 1：创建 PreToolUse 钩子
Gerçekleştir钩子功能：
- 在工具执行前记录调用参数
- Destekle白名单/黑名单过滤

### Alıştırma 2：创建 PostToolUse 钩子
Gerçekleştir钩子功能：
- 在工具执行后格式化输出
- 记录执行时间

### Alıştırma 3：配置异步钩子
创建异步钩子处理：
- 外部 API 调用
- 文件系统监控

## 钩子Şablon

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

## Doğrulama
1. 触发条件Senaryo
2. 检查钩子输出
3. Doğrulama日志记录
