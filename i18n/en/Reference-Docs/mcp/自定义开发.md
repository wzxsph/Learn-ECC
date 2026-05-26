# Custom MCP Server Development

## Overview

MCP servers are based on the Model Context Protocol implementation, used to extend Claude Code capabilities. This guide introduces how to develop and integrate custom MCP servers.

## Server Types

### Process Mode Server

Started via local process, suitable for services that need local execution or access to local resources.

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

Remote servers accessed via HTTP/HTTPS, suitable for microservice architecture or cloud services.

```json
{
  "type": "http",
  "url": "https://your-mcp-server.example.com/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_TOKEN"
  }
}
```

## Development Flow

### 1. Use MCP SDK

Official SDKs provided in multiple languages:

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

// Register tools
server.setRequestHandler('tools/list', async () => {
  return {
    tools: [
      {
        name: "my_tool",
        description: "Perform custom operation",
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

// Handle tool calls
server.setRequestHandler('tools/call', async (request) => {
  const { name, arguments: args } = request.params;

  if (name === "my_tool") {
    // Perform operation
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
            description="Perform custom operation",
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
        # Perform operation
        return [TextContent(type="text", text="result")]

async def main():
    async with stdio_server() as (read_stream, write_stream):
        await server.run(read_stream, write_stream, server.create_initialization_options())
```

## Configuration and Integration

### 1. Local Testing

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

### 2. Add to ECC

Add configuration to `mcp-configs/mcp-servers.json`:

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

## Best Practices

1. **Error handling**: Always return structured error responses
2. **Input validation**: Validate all input parameters, reject invalid requests
3. **Timeout control**: Set reasonable timeout times
4. **Logging**: Log key operations for debugging
5. **Resource cleanup**: Ensure connections and resources are properly released
6. **Version management**: Follow semantic versioning

## Common Tool Types

| Type | Purpose |
|------|------|
| `tools/list` | List available tools |
| `tools/call` | Call tool |
| `resources/list` | List resources |
| `resources/read` | Read resource |
| `prompts/list` | List prompt templates |
| `prompts/get` | Get prompt |

## Reference Resources

- [MCP Official Documentation](https://modelcontextprotocol.io/)
- [MCP SDK (Node.js)](https://github.com/modelcontextprotocol/typescript-sdk)
- [MCP SDK (Python)](https://github.com/modelcontextprotocol/python-sdk)
- [MCP Specification](https://modelcontextprotocol.io/specification)