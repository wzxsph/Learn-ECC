# Custom MCP Server Development

## Tổng quan

MCP server dựa trên Model Context Protocol implementation, dùng để mở rộng capability của Claude Code. Hướng dẫn này giới thiệu cách develop và integrate custom MCP server.

## Server Type

### Process Mode Server

Start qua local process, phù hợp với service cần local execution hoặc access local resource.

```json
{
  "command": "node",
  "args": ["/path/to/your/server.js"],
  "env": {
    "YOUR_API_KEY": "value"
  }
}
```

### HTTP Mode Server

Access remote server qua HTTP/HTTPS, phù hợp với microservice architecture hoặc cloud service.

```json
{
  "type": "http",
  "url": "https://your-mcp-server.example.com/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_TOKEN"
  }
}
```

## Development Process

### 1. Sử dụng MCP SDK

Official cung cấp multi-language SDK:

```bash
# Node.js
npm install @modelcontextprotocol/sdk

# Python
pip install mcp
```

### 2. Implement Server

**Node.js Example:**

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

// Register tool
server.setRequestHandler('tools/list', async () => {
  return {
    tools: [
      {
        name: "my_tool",
        description: "Execute custom operation",
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

// Handle tool call
server.setRequestHandler('tools/call', async (request) => {
  const { name, arguments: args } = request.params;

  if (name === "my_tool") {
    // Execute operation
    return { content: [{ type: "text", text: "result" }] };
  }
});

// Start server
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
}

main();
```

**Python Example:**

```python
from mcp.server import Server
from mcp.server.stdio import stdio_server

server = Server("my-custom-server")

@server.list_tools()
async def list_tools():
    return [
        Tool(
            name="my_tool",
            description="Execute custom operation",
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
        # Execute operation
        return [TextContent(type="text", text="result")]

async def main():
    async with stdio_server() as (read_stream, write_stream):
        await server.run(read_stream, write_stream, server.create_initialization_options())
```

## Configuration và Integration

### 1. Local Test

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

### 2. Thêm vào ECC

Thêm configuration vào `mcp-configs/mcp-servers.json`:

```json
{
  "mcpServers": {
    "my-custom": {
      "command": "npx",
      "args": ["-y", "/path/to/your/server"],
      "env": {
        "YOUR_API_KEY": "YOUR_KEY_HERE"
      },
      "description": "Your custom server description"
    }
  }
}
```

## Best Practice

1. **Error Handling**: Luôn return structured error response
2. **Input Validation**: Validate tất cả input parameter, reject invalid request
3. **Timeout Control**: Set reasonable timeout
4. **Logging**: Log key operation cho debugging
5. **Resource Cleanup**: Ensure connection và resource được release đúng cách
6. **Version Management**: Follow semantic versioning

## Common Tool Type

| Loại | Mục đích |
|------|------|
| `tools/list` | List available tools |
| `tools/call` | Call tool |
| `resources/list` | List resources |
| `resources/read` | Read resource |
| `prompts/list` | List prompt template |
| `prompts/get` | Get prompt |

## Reference Resource

- [MCP Official Documentation](https://modelcontextprotocol.io/)
- [MCP SDK (Node.js)](https://github.com/modelcontextprotocol/typescript-sdk)
- [MCP SDK (Python)](https://github.com/modelcontextprotocol/python-sdk)
- [MCP Specification](https://modelcontextprotocol.io/specification)