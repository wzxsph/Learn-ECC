# Desenvolvimento Customizado de Servidores MCP

## Visão Geral

Servidores MCP baseados no Model Context Protocol são usados para expandir capacidades do Claude Code. Este guia apresenta como desenvolver e integrar servidores MCP customizados.

## Tipos de Servidor

### Servidor em Modo Processo

Iniciado via processo local, adequado para serviços que precisam de acesso local ou recursos locais.

```json
{
  "command": "node",
  "args": ["/path/to/your/server.js"],
  "env": {
    "YOUR_API_KEY": "value"
  }
}
```

### Servidor em Modo HTTP

Acessado via HTTP/HTTPS, adequado para arquitetura de microsserviços ou serviços em nuvem.

```json
{
  "type": "http",
  "url": "https://your-mcp-server.example.com/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_TOKEN"
  }
}
```

## Fluxo de Desenvolvimento

### 1. Usar MCP SDK

SDKs oficiais disponíveis em múltiplas linguagens:

```bash
# Node.js
npm install @modelcontextprotocol/sdk

# Python
pip install mcp
```

### 2. Implementar Servidor

**Exemplo Node.js:**

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

// Registrar ferramentas
server.setRequestHandler('tools/list', async () => {
  return {
    tools: [
      {
        name: "my_tool",
        description: "Executar operação customizada",
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

// Tratar chamada de ferramenta
server.setRequestHandler('tools/call', async (request) => {
  const { name, arguments: args } = request.params;

  if (name === "my_tool") {
    // Executar operação
    return { content: [{ type: "text", text: "resultado" }] };
  }
});

// Iniciar servidor
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
}

main();
```

**Exemplo Python:**

```python
from mcp.server import Server
from mcp.server.stdio import stdio_server

server = Server("my-custom-server")

@server.list_tools()
async def list_tools():
    return [
        Tool(
            name="my_tool",
            description="Executar operação customizada",
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
        # Executar operação
        return [TextContent(type="text", text="resultado")]

async def main():
    async with stdio_server() as (read_stream, write_stream):
        await server.run(read_stream, write_stream, server.create_initialization_options())
```

## Configuração e Integração

### 1. Teste Local

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

### 2. Adicionar ao ECC

Adicionar configuração ao `mcp-configs/mcp-servers.json`:

```json
{
  "mcpServers": {
    "my-custom": {
      "command": "npx",
      "args": ["-y", "/path/to/your/server"],
      "env": {
        "YOUR_API_KEY": "YOUR_KEY_HERE"
      },
      "description": "Descrição do seu servidor customizado"
    }
  }
}
```

## Melhores Práticas

1. **Tratamento de Erros**: Sempre retornar respostas de erro estruturadas
2. **Validação de Input**: Validar todos os parâmetros de input, rejeitar requisições inválidas
3. **Controle de Timeout**: Definir timeouts razoáveis
4. **Logging**: Registrar operações importantes para debugging
5. **Limpeza de Recursos**: Garantir que conexões e recursos são liberados corretamente
6. **Gerenciamento de Versão**: Seguir versionamento semântico

## Tipos Comuns de Ferramentas

| Tipo | Propósito |
|------|----------|
| `tools/list` | Listar ferramentas disponíveis |
| `tools/call` | Chamar ferramenta |
| `resources/list` | Listar recursos |
| `resources/read` | Ler recurso |
| `prompts/list` | Listar templates de prompt |
| `prompts/get` | Obter prompt |

## Recursos de Referência

- [Documentação oficial MCP](https://modelcontextprotocol.io/)
- [MCP SDK (Node.js)](https://github.com/modelcontextprotocol/typescript-sdk)
- [MCP SDK (Python)](https://github.com/modelcontextprotocol/python-sdk)
- [Especificação MCP](https://modelcontextprotocol.io/specification)