# MCP Формат конфигурации

## Обзор

MCP (Model Context Protocol) 服务器Через `~/.claude.json` 文件中的 `mcpServers` 节进行配置。Claude Code Поддерживать两种Тип的 MCP сервер：

- **进程模式 (process)**：Через本地命令启动的服务器
- **HTTP 模式 (http)**：Через HTTP/HTTPS 访问的远程服务器

## JSON 配置结构

```json
{
  "mcpServers": {
    "服务器Название": {
      "type": "http",           // 可选，默认 process
      "command": "npx",        // 进程模式必需
      "url": "https://...",    // HTTP 模式必需
      "args": ["参数1", "参数2"],  // 可选
      "env": {                 // 可选，环境变量
        "KEY": "value"
      },
      "headers": {             // 可选，HTTP 请求头
        "Header-Name": "value"
      },
      "description": "服务器Описание"  // 可选
    }
  }
}
```

## 字段Описание

| 字段 | Тип | 必需 | Описание |
|------|------|------|------|
| `type` | string | 否 | 连接Тип：`process`（默认）或 `http` |
| `command` | string | type=process 时必需 | 启动命令（如 `npx`, `uvx`, `python3`） |
| `url` | string | type=http 时必需 | MCP сервер的 HTTP/HTTPS URL |
| `args` | array | 否 | 传递给命令的参数数组 |
| `env` | object | 否 | 环境变量键值对 |
| `headers` | object | 否 | HTTP 请求头（仅 http 模式） |
| `description` | string | 否 | 服务器功能Описание |

## 配置Пример

### 进程模式Пример

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "GitHub 操作 - PRs, issues, repos"
    }
  }
}
```

### HTTP 模式Пример

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Vercel 部署和项目"
    }
  }
}
```

### 带认证头的 HTTP 模式

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "YOUR_KEY_HERE"
      },
      "description": "AI 浏览器代理"
    }
  }
}
```

## Лучшие практики

1. **环境变量Управлять**：敏感信息（API keys, tokens）Обязательно使用环境变量，不要硬编码
2. **上下文窗口**：建议保持启用不超过 10 个 MCP сервер，以保留上下文窗口
3. **禁用服务器**：使用 `ECC_DISABLED_MCPS=server1,server2` 环境变量可禁用打包的 MCP сервер

## 故障排除

- 确保命令可执行（npx, uvx 等已安装）
- 检查环境变量是否正确设置
- 查看 Claude Code 日志中的服务器连接错误
