# Formato de Configuração MCP

## Visão Geral

Os servidores MCP (Model Context Protocol) são configurados através da seção `mcpServers` no arquivo `~/.claude.json`. O Claude Code suporta dois tipos de servidores MCP:

- **Modo processo (process)**: Servidores iniciados via comando local
- **Modo HTTP (http)**: Servidores remotos acessados via HTTP/HTTPS

## Estrutura de Configuração JSON

```json
{
  "mcpServers": {
    "nome-do-servidor": {
      "type": "http",           // Opcional, padrão é process
      "command": "npx",        // Necessário para modo process
      "url": "https://...",    // Necessário para modo http
      "args": ["param1", "param2"],  // Opcional
      "env": {                 // Opcional, variáveis de ambiente
        "KEY": "value"
      },
      "headers": {             // Opcional, headers de requisição HTTP
        "Header-Name": "value"
      },
      "description": "Descrição do servidor"  // Opcional
    }
  }
}
```

## Descrição de Campos

| Campo | Tipo | Obrigatório | Descrição |
|------|------|-------------|----------|
| `type` | string | Não | Tipo de conexão: `process` (padrão) ou `http` |
| `command` | string | Sim, quando type=process | Comando de inicialização (como `npx`, `uvx`, `python3`) |
| `url` | string | Sim, quando type=http | URL HTTP/HTTPS do servidor MCP |
| `args` | array | Não | Array de argumentos passados ao comando |
| `env` | object | Não | Pares de variáveis de ambiente |
| `headers` | object | Não | Headers de requisição HTTP (apenas modo http) |
| `description` | string | Não | Descrição da funcionalidade do servidor |

## Exemplos de Configuração

### Exemplo de Modo Processo

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "Operações GitHub - PRs, issues, repos"
    }
  }
}
```

### Exemplo de Modo HTTP

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Deploy e projetos Vercel"
    }
  }
}
```

### Exemplo de Modo HTTP com Headers de Autenticação

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "YOUR_KEY_HERE"
      },
      "description": "Proxy de navegador AI"
    }
  }
}
```

## Melhores Práticas

1. **Gerenciamento de Variáveis de Ambiente**: Informações sensíveis (API keys, tokens) devem usar variáveis de ambiente, não hardcode
2. **Janela de Contexto**: Recomenda-se manter habilitados no máximo 10 servidores MCP, para preservar janela de contexto
3. **Desabilitar Servidores**: Usar variável de ambiente `ECC_DISABLED_MCPS=server1,server2` pode desabilitar servidores MCP empacotados

## Troubleshooting

- Garantir que comando é executável (npx, uvx etc. instalados)
- Verificar se variáveis de ambiente estão configuradas corretamente
- Verificar erros de conexão de servidor nos logs do Claude Code