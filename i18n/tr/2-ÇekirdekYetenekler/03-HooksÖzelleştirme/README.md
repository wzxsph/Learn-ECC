# 03 Hooks Özelleştirme

> 学会编写 PreToolUse、PostToolUse 和 Stop Hooks，自动化质量门禁

## 模块Hedef

Bitir本模块后，你将Yapabilir：

- 理解 Claude Code 的 Hook Türleri和触发机制
- 创建 PreToolUse Hook 进行工具调用前Doğrulama
- 创建 PostToolUse Hook 进行工具调用后处理
- 配置质量门禁 Hook Gerçekleştir自动化检查

## Hook TürleriGenel Bakış

| Hook Türleri | 触发时机 | 典型Kullanım |
|-----------|----------|----------|
| **PreToolUse** | 工具调用前 | 参数Doğrulama、权限检查、Güvenlik扫描 |
| **PostToolUse** | 工具调用后 | 结果格式化、日志记录、自动化处理 |
| **Stop** | 会话结束时 | 会话总结、状态保存、清理工作 |

## PreToolUse Hook

### 工作原理

```
用户输入 → PreToolUse Hook → 工具执行 → 结果Geri Dön
```

### 常见Senaryo

1. **GüvenlikDoğrulama**
   - 检查危险命令（如 `rm -rf`）
   - Doğrulama文件路径Güvenlik性
   - 检查敏感数据访问

2. **参数Doğrulama**
   - Doğrulama工具参数格式
   - 检查必需参数是否存在
   - Tür检查

3. **权限控制**
   - 检查是否授权执行该操作
   - Doğrulama项目访问权限

### Örnek Hook 配置

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('rm -rf')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "Uyarı: 检测到危险命令 rm -rf，请在执行前确认！"
    }
  ]
}
```

## PostToolUse Hook

### 工作原理

```
工具执行 → PostToolUse Hook → 结果Geri Dön → 用户
```

### 常见Senaryo

1. **结果格式化**
   - 美化输出格式
   - 提取关键信息
   - 简化长输出

2. **自动化处理**
   - 构建成功后自动运行Test
   - Test失败后自动记录日志
   - 提交前自动格式化代码

3. **质量检查**
   - 检查Geri Dön的错误信息
   - Doğrulama输出Güvenlik性
   - 分析性能数据

### Örnek Hook 配置

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm test')"
  },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'TestBitir: 检查覆盖率...'",
      "executeAfter": true
    }
  ]
}
```

## Stop Hook

### 工作原理

```
会话结束信号 → Stop Hook → 执行清理 → 会话终止
```

### 常见Senaryo

1. **会话总结**
   - 生成会话摘要
   - 记录Bitir的Görev
   - 列出待办事项

2. **状态保存**
   - 保存草稿文件
   - 持久化进度
   - 清理临时文件

3. **通知发送**
   - 发送Bitir通知
   - 汇总报告
   - 触发Sonraki Adım流程

## Hook 配置结构

```json
{
  "matcher": {
    "tool": "工具Ad",
    "operation": "操作Tür",
    "condition": "条件表达式"
  },
  "hooks": [
    {
      "type": "notification | command | stop",
      "when": "before | after",
      "message": "通知消息",
      "command": "要执行的命令",
      "continue": true
    }
  ]
}
```

## 常用 Hook Senaryo

### Senaryo 1: Git 提交前检查

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('git commit')"
  },
  "hooks": [
    {
      "type": "command",
      "command": "npm test",
      "continue": false
    }
  ]
}
```

### Senaryo 2: 构建成功通知

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm run build')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "构建Bitir！正在运行Test...",
      "executeAfter": true
    }
  ]
}
```

### Senaryo 3: 会话结束总结

```json
{
  "matcher": {
    "type": "stop"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "会话结束。Bitir: {summary}",
      "command": "node scripts/save-session.js"
    }
  ]
}
```

## Hook 开发En İyi Uygulamalar

1. **保持简单**: Hook 逻辑Olmalı简单直接
2. **快速执行**: PreToolUse Hook 应在 200ms 内Bitir
3. **优雅降级**: 失败时Olmalı继续执行，不阻塞工具
4. **清晰日志**: 使用 `[HookName]` 前缀标记日志
5. **Test覆盖**: 为 Hook 编写Test

## Destekleyici Materyaller

完整的 Hooks 文档位于：`../../ReferansBelgeleri/hooks/HookTür.md`

## Sonraki Adım

- 学习[Alıştırma题目](./exercises/Alıştırma.md)
- 阅读 [Hooks 完整文档](../../ReferansBelgeleri/hooks/HookTür.md)