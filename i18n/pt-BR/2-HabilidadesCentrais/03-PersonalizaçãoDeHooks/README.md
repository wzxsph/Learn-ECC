# 03 Hooks 定制

> 学会编写 PreToolUse、PostToolUse 和 Stop Hooks，自动化质量门禁

## 模块目标

完成本模块后，你将能够：

- 理解 Claude Code 的 Hook 类型和触发机制
- 创建 PreToolUse Hook 进行工具调用前验证
- 创建 PostToolUse Hook 进行工具调用后处理
- 配置质量门禁 Hook 实现自动化检查

## Hook 类型概述

| Hook 类型 | 触发时机 | 典型用途 |
|-----------|----------|----------|
| **PreToolUse** | 工具调用前 | 参数验证、权限检查、安全扫描 |
| **PostToolUse** | 工具调用后 | 结果格式化、日志记录、自动化处理 |
| **Stop** | 会话结束时 | 会话总结、状态保存、清理工作 |

## PreToolUse Hook

### 工作原理

```
用户输入 → PreToolUse Hook → 工具执行 → 结果返回
```

### 常见场景

1. **安全验证**
   - 检查危险命令（如 `rm -rf`）
   - 验证文件路径安全性
   - 检查敏感数据访问

2. **参数验证**
   - 验证工具参数格式
   - 检查必需参数是否存在
   - 类型检查

3. **权限控制**
   - 检查是否授权执行该操作
   - 验证项目访问权限

### 示例 Hook 配置

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('rm -rf')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "警告: 检测到危险命令 rm -rf，请在执行前确认！"
    }
  ]
}
```

## PostToolUse Hook

### 工作原理

```
工具执行 → PostToolUse Hook → 结果返回 → 用户
```

### 常见场景

1. **结果格式化**
   - 美化输出格式
   - 提取关键信息
   - 简化长输出

2. **自动化处理**
   - 构建成功后自动运行测试
   - 测试失败后自动记录日志
   - 提交前自动格式化代码

3. **质量检查**
   - 检查返回的错误信息
   - 验证输出安全性
   - 分析性能数据

### 示例 Hook 配置

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm test')"
  },
  "hooks": [
    {
      "type": "command",
      "command": "echo '测试完成: 检查覆盖率...'",
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

### 常见场景

1. **会话总结**
   - 生成会话摘要
   - 记录完成的任务
   - 列出待办事项

2. **状态保存**
   - 保存草稿文件
   - 持久化进度
   - 清理临时文件

3. **通知发送**
   - 发送完成通知
   - 汇总报告
   - 触发下一步流程

## Hook 配置结构

```json
{
  "matcher": {
    "tool": "工具名称",
    "operation": "操作类型",
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

## 常用 Hook 场景

### 场景 1: Git 提交前检查

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

### 场景 2: 构建成功通知

```json
{
  "matcher": {
    "tool": "Bash",
    "condition": "command.includes('npm run build')"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "构建完成！正在运行测试...",
      "executeAfter": true
    }
  ]
}
```

### 场景 3: 会话结束总结

```json
{
  "matcher": {
    "type": "stop"
  },
  "hooks": [
    {
      "type": "notification",
      "message": "会话结束。完成: {summary}",
      "command": "node scripts/save-session.js"
    }
  ]
}
```

## Hook 开发最佳实践

1. **保持简单**: Hook 逻辑应该简单直接
2. **快速执行**: PreToolUse Hook 应在 200ms 内完成
3. **优雅降级**: 失败时应该继续执行，不阻塞工具
4. **清晰日志**: 使用 `[HookName]` 前缀标记日志
5. **测试覆盖**: 为 Hook 编写测试

## 配套教材

完整的 Hooks 文档位于：`../../DocumentosDeReferência/hooks/Hook类型.md`

## 下一步

- 学习[练习题目](./exercises/练习.md)
- 阅读 [Hooks 完整文档](../../DocumentosDeReferência/hooks/Hook类型.md)