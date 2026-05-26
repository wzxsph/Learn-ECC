# 01-MCP Geliştirme

学习模型上下文协议（Model Context Protocol）扩展开发。

## 什么是 MCP？

MCP（Model Context Protocol）是一种让 Claude 与外部工具和服务集成的协议。Vasıtasıyla MCP，你Olabilir扩展 Claude 的能力，连接数据库、API、其他 AI 模型等。

> 参考: [MCP Yapılandırma Formatı](../ReferansBelgeleri/mcp/MCPYapılandırma Formatı.md)

## Öğrenme Hedefleri

- 理解 MCP 架构和原理
- 创建 MCP Sunucusu
- Gerçekleştir MCP 工具
- Test和调试 MCP 扩展

## Temel Kavramlar

### 1. MCP Sunucusu
MCP Sunucusu是Sağla工具和资源的进程。

### 2. 工具（Tools）
可被 Claude 调用执行的操作。

### 3. 资源（Resources）
Claude Olabilir读取的数据源。

### 4. İpucu（Prompts）
预定义的İpucuŞablon。

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

## Sonraki Adım

- [Otonom Ajanlar](./02-Otonom Ajanlar.md) - 构建自治 Agent
- [Geri DönUzmanlık YoluDizin](../README.md)
