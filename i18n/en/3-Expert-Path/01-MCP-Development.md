# 01 - MCP Development

Learn Model Context Protocol extension development.

## What is MCP?

MCP (Model Context Protocol) is a protocol that enables Claude to integrate with external tools and services. Through MCP, you can extend Claude's capabilities, connecting to databases, APIs, other AI models, and more.

> Reference: [MCP Configuration Format](../Reference-Docs/mcp/MCPConfiguration-Format.md)

## Learning Objectives

- Understand MCP architecture and principles
- Create MCP servers
- Implement MCP tools
- Test and debug MCP extensions

## Core Concepts

### 1. MCP Server
MCP servers are processes that provide tools and resources.

### 2. Tools
Operations that can be invoked by Claude.

### 3. Resources
Data sources that Claude can read.

### 4. Prompts
Pre-defined prompt templates.

## MCP Configuration

MCP configuration file is located at `.claude/mcp.json`:

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

## Next Steps

- [Autonomous Agents](./02-Autonomous-Agents.md) - Build autonomous Agents
- [Return to Expert Path README](../README.md)