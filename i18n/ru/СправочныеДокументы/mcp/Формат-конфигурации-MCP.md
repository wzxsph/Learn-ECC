# MCP Формат конфигурации

## Обзор

MCP (Model Context Protocol) Через `~/.claude.json`  `mcpServers` . Claude Code ПоддерживатьТип MCP сервер：

- ** (process)**：Через
- **HTTP  (http)**：Через HTTP/HTTPS 

## JSON 

```json
{
  "mcpServers": {
    "Название": {
      "type": "http",           // ,  process
      "command": "npx",        // 
      "url": "https://...",    // HTTP 
      "args": ["1", "2"],  // 
      "env": {                 // , 
        "KEY": "value"
      },
      "headers": {             // , HTTP 
        "Header-Name": "value"
      },
      "description": "Описание"  // 
    }
  }
}
```

## Описание

|  | Тип |  | Описание |
|------|------|------|------|
| `type` | string |  | Тип：`process` () `http` |
| `command` | string | type=process  |  ( `npx`, `uvx`, `python3`) |
| `url` | string | type=http  | MCP сервер HTTP/HTTPS URL |
| `args` | array |  |  |
| `env` | object |  |  |
| `headers` | object |  | HTTP  ( http ) |
| `description` | string |  | функцияОписание |

## Пример

### Пример

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "GitHub  - PRs, issues, repos"
    }
  }
}
```

### HTTP Пример

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Vercel "
    }
  }
}
```

###  HTTP 

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "YOUR_KEY_HERE"
      },
      "description": "AI "
    }
  }
}
```

## Лучшие практики

1. **Управлять**： (API keys, tokens)обязательно используется, 
2. ****： 10  MCP сервер, 
3. ****： `ECC_DISABLED_MCPS=server1,server2`  MCP сервер

## 

-  (npx, uvx )
- проверка
-  Claude Code ошибка
