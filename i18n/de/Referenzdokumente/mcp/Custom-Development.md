# 自定义 MCP 服务器开发

## 概述

MCP 服务器基于 Model Context Protocol 实现，用于扩展 Claude Code 的能力。本指南介绍如何开发和集成自定义 MCP 服务器。

## 服务器类型

### 进程模式服务器

通过本地进程启动，适合需要本地执行或访问本地资源的服务。

```json
{
  "command": "node",
  "args": ["/path/to/your/server.js"],
  "env": {
    "YOUR_API_KEY": "value"
  }
}
```

### HTTP 模式服务器

通过 HTTP/HTTPS 访问远程服务器，适合微服务架构或云服务。

```json
{
  "type": "http",
  "url": "https://your-mcp-server.example.com/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_TOKEN"
  }
}
```

## 开发流程

### 1. 使用 MCP SDK

官方提供多种语言的 SDK：

```bash
# Node.js
npm install @modelcontextprotocol/sdk

# Python
pip install mcp
```

### 2. 实现服务器

**Node.js 示例：**

```javascript
const { Server } = require('@modelcontextprotocol/sdk');
const { StdioServerTransport } = require('@modelcontextprotocol/sdk');

const server = new Server(
  {
    name: "my-custom-server",
    version: "1.0.0"
  },
  {
    capabilities: {
      tools: {},
      resources: {}
    }
  }
);

// 注册工具
server.setRequestHandler('tools/list', async () => {
  return {
    tools: [
      {
        name: "my_tool",
        description: "执行自定义操作",
        inputSchema: {
          type: "object",
          properties: {
            param: { type: "string" }
          }
        }
      }
    ]
  };
});

// 处理工具调用
server.setRequestHandler('tools/call', async (request) => {
  const { name, arguments: args } = request.params;

  if (name === "my_tool") {
    // 执行操作
    return { content: [{ type: "text", text: "结果" }] };
  }
});

// 启动服务器
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
}

main();
```

**Python 示例：**

```python
from mcp.server import Server
from mcp.server.stdio import stdio_server

server = Server("my-custom-server")

@server.list_tools()
async def list_tools():
    return [
        Tool(
            name="my_tool",
            description="执行自定义操作",
            inputSchema={
                "type": "object",
                "properties": {
                    "param": {"type": "string"}
                }
            }
        )
    ]

@server.call_tool()
async def call_tool(name, arguments):
    if name == "my_tool":
        # 执行操作
        return [TextContent(type="text", text="结果")]

async def main():
    async with stdio_server() as (read_stream, write_stream):
        await server.run(read_stream, write_stream, server.create_initialization_options())
```

## 配置与集成

### 1. 本地测试

```json
{
  "mcpServers": {
    "my-custom": {
      "command": "node",
      "args": ["/absolute/path/to/server.js"],
      "env": {
        "DEBUG": "true"
      }
    }
  }
}
```

### 2. 添加到 ECC

将配置添加到 `mcp-configs/mcp-servers.json`：

```json
{
  "mcpServers": {
    "my-custom": {
      "command": "npx",
      "args": ["-y", "/path/to/your/server"],
      "env": {
        "YOUR_API_KEY": "YOUR_KEY_HERE"
      },
      "description": "你的自定义服务器描述"
    }
  }
}
```

## 最佳实践

1. **错误处理**：始终返回结构化的错误响应
2. **输入验证**：验证所有输入参数，拒绝无效请求
3. **超时控制**：设置合理的超时时间
4. **日志记录**：记录关键操作便于调试
5. **资源清理**：确保连接和资源正确释放
6. **版本管理**：遵循语义化版本

## 常用工具类型

| 类型 | 用途 |
|------|------|
| `tools/list` | 列出可用工具 |
| `tools/call` | 调用工具 |
| `resources/list` | 列出资源 |
| `resources/read` | 读取资源 |
| `prompts/list` | 列出提示模板 |
| `prompts/get` | 获取提示 |

## 参考资源

- [MCP 官方文档](https://modelcontextprotocol.io/)
- [MCP SDK (Node.js)](https://github.com/modelcontextprotocol/typescript-sdk)
- [MCP SDK (Python)](https://github.com/modelcontextprotocol/python-sdk)
- [MCP 规范](https://modelcontextprotocol.io/specification)
