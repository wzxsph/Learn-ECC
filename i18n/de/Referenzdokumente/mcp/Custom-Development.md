# Benutzerdefinierte MCP-Server-Entwicklung

## Ueberblick

MCP-Server basieren auf dem Model Context Protocol und werden verwendet um die Faehigkeiten von Claude Code zu erweitern. Dieses Handbuch beschreibt wie man benutzerdefinierte MCP-Server entwickelt und integriert.

## Server-Typen

### Process-Modus-Server

Startet ueber lokalen Prozess, geeignet fuer Dienste die lokale Ausführung oder Zugriff auf lokale Ressourcen benoetigen.

```json
{
  "command": "node",
  "args": ["/path/to/your/server.js"],
  "env": {
    "YOUR_API_KEY": "value"
  }
}
```

### HTTP-Modus-Server

Zugreifbar ueber HTTP/HTTPS aufRemote-Server, geeignet fuer Microservice-Architektur oder Cloud-Dienste.

```json
{
  "type": "http",
  "url": "https://your-mcp-server.example.com/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_TOKEN"
  }
}
```

## Entwicklungsprozess

### 1. MCP SDK verwenden

Offiziell werden SDKs in mehreren Sprachen bereitgestellt:

```bash
# Node.js
npm install @modelcontextprotocol/sdk

# Python
pip install mcp
```

### 2. Server implementieren

**Node.js Beispiel:**

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

// Tools registrieren
server.setRequestHandler('tools/list', async () => {
  return {
    tools: [
      {
        name: "my_tool",
        description: "Custom operation ausfuehren",
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

// Tool-Aufrufe bearbeiten
server.setRequestHandler('tools/call', async (request) => {
  const { name, arguments: args } = request.params;

  if (name === "my_tool") {
    // Operation ausfuehren
    return { content: [{ type: "text", text: "Ergebnis" }] };
  }
});

// Server starten
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
}

main();
```

**Python Beispiel:**

```python
from mcp.server import Server
from mcp.server.stdio import stdio_server

server = Server("my-custom-server")

@server.list_tools()
async def list_tools():
    return [
        Tool(
            name="my_tool",
            description="Custom operation ausfuehren",
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
        # Operation ausfuehren
        return [TextContent(type="text", text="Ergebnis")]

async def main():
    async with stdio_server() as (read_stream, write_stream):
        await server.run(read_stream, write_stream, server.create_initialization_options())
```

## Konfiguration und Integration

### 1. Lokal testen

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

### 2. Zu ECC hinzufuegen

Konfiguration zu `mcp-configs/mcp-servers.json` hinzufuegen:

```json
{
  "mcpServers": {
    "my-custom": {
      "command": "npx",
      "args": ["-y", "/path/to/your/server"],
      "env": {
        "YOUR_API_KEY": "YOUR_KEY_HERE"
      },
      "description": "Ihre benutzerdefinierte Serverbeschreibung"
    }
  }
}
```

## Best Practices

1. **Fehlerbehandlung**: Immer strukturierte Fehlerantworten zurueckgeben
2. **Eingabevalidierung**: Alle Eingabeparameter validieren, ungueltige Anfragen ablehnen
3. **Timeout-Kontrolle**: Angemessene Timeouts setzen
4. **Logging**: Wichtige Operationen fuer Debugging protokollieren
5. **Ressourcenbereinigung**: Sicherstellen dass Verbindungen und Ressourcen korrekt freigegeben werden
6. **Versionsverwaltung**: Semantische Versionierung befolgen

## Haeufig verwendete Tool-Typen

| Typ | Verwendungszweck |
|------|------|
| `tools/list` | Verfuegbare Tools auflisten |
| `tools/call` | Tool aufrufen |
| `resources/list` | Ressourcen auflisten |
| `resources/read` | Ressource lesen |
| `prompts/list` | Prompt-Vorlagen auflisten |
| `prompts/get` | Prompt abrufen |

## Referenzressourcen

- [MCP offizielle Dokumentation](https://modelcontextprotocol.io/)
- [MCP SDK (Node.js)](https://github.com/modelcontextprotocol/typescript-sdk)
- [MCP SDK (Python)](https://github.com/modelcontextprotocol/python-sdk)
- [MCP Spezifikation](https://modelcontextprotocol.io/specification)