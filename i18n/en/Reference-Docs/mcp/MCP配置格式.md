# MCP Configuration Format

## Overview

MCP (Model Context Protocol) servers are configured via the `mcpServers` section in `~/.claude.json`. Claude Code supports two types of MCP servers:

- **Process mode (process)**: Servers started via local command
- **HTTP mode (http)**: Remote servers accessed via HTTP/HTTPS

## JSON Configuration Structure

```json
{
  "mcpServers": {
    "server-name": {
      "type": "http",           // Optional, defaults to process
      "command": "npx",        // Required for process mode
      "url": "https://...",    // Required for HTTP mode
      "args": ["arg1", "arg2"],  // Optional
      "env": {                 // Optional, environment variables
        "KEY": "value"
      },
      "headers": {             // Optional, HTTP request headers
        "Header-Name": "value"
      },
      "description": "Server description"  // Optional
    }
  }
}
```

## Field Description

| Field | Type | Required | Description |
|------|------|------|------|
| `type` | string | No | Connection type: `process` (default) or `http` |
| `command` | string | Required when type=process | Startup command (e.g., `npx`, `uvx`, `python3`) |
| `url` | string | Required when type=http | HTTP/HTTPS URL for MCP server |
| `args` | array | No | Argument array passed to command |
| `env` | object | No | Environment variable key-value pairs |
| `headers` | object | No | HTTP request headers (HTTP mode only) |
| `description` | string | No | Server capability description |

## Configuration Examples

### Process Mode Example

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "GitHub operations - PRs, issues, repos"
    }
  }
}
```

### HTTP Mode Example

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Vercel deployment and projects"
    }
  }
}
```

### HTTP Mode with Auth Headers

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "YOUR_KEY_HERE"
      },
      "description": "AI browser agent"
    }
  }
}
```

## Best Practices

1. **Environment variable management**: Sensitive information (API keys, tokens) must use environment variables, not hardcoded
2. **Context window**: Recommended to keep no more than 10 MCP servers enabled to preserve context window
3. **Disable servers**: Use `ECC_DISABLED_MCPS=server1,server2` environment variable to disable bundled MCP servers

## Troubleshooting

- Ensure command is executable (npx, uvx, etc. are installed)
- Check if environment variables are correctly set
- Check server connection errors in Claude Code logs