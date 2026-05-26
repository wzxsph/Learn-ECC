# 01-Разработка MCP

学习模型上下文协议（Model Context Protocol）扩展开发。

## 什么是 MCP？

MCP（Model Context Protocol）是一种让 Claude 与外部工具和服务集成的协议。Через MCP，你Можно扩展 Claude 的能力，连接数据库、API、其他 AI 模型等。

> 参考: [MCP Формат конфигурации](../СправочныеДокументы/mcp/MCPФормат конфигурации.md)

## Цели обучения

- 理解 MCP 架构和原理
- 创建 MCP сервер
- Реализовать MCP 工具
- Тестирование和调试 MCP 扩展

## Основные концепции

### 1. MCP сервер
MCP сервер是Предоставлять工具和资源的进程。

### 2. 工具（Tools）
可被 Claude 调用执行的操作。

### 3. 资源（Resources）
Claude Можно读取的数据源。

### 4. Подсказка（Prompts）
预定义的ПодсказкаШаблон。

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

## Следующий шаг

- [Автономные агенты](./02-Автономные агенты.md) - 构建自治 Agent
- [НазадПуть к мастерствуСодержание](../README.md)
