# 01-MCP 开发

学习模型上下文协议（Model Context Protocol）扩展开发。

## 什么是 MCP？

MCP（Model Context Protocol）是一种让 Claude 与外部工具和服务集成的协议。通过 MCP，你可以扩展 Claude 的能力，连接数据库、API、其他 AI 模型等。

> 参考: [MCP 配置格式](../参考文档/mcp/MCP配置格式.md)

## 学习目标

- 理解 MCP 架构和原理
- 创建 MCP 服务器
- 实现 MCP 工具
- 测试和调试 MCP 扩展

## 核心概念

### 1. MCP 服务器
MCP 服务器是提供工具和资源的进程。

### 2. 工具（Tools）
可被 Claude 调用执行的操作。

### 3. 资源（Resources）
Claude 可以读取的数据源。

### 4. 提示（Prompts）
预定义的提示模板。

## MCP 配置

MCP 配置文件位于 `.claude/mcp.json`：

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["./mcp-server.js"],
      "env": {
        "API_KEY": "your-key"
      }
    }
  }
}
```

## 下一步

- [自主代理](./02-自主代理.md) - 构建自治 Agent
- [返回专家之路目录](../README.md)
