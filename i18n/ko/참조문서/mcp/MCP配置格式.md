# MCP 설정 형식

## 개요

MCP (Model Context Protocol) 서버통과 `~/.claude.json` 파일中的 `mcpServers` 节进行설정。Claude Code 지원两种유형的 MCP 서버：

- **프로세스패턴 (process)**：통과本地명령시작的서버
- **HTTP 패턴 (http)**：통과 HTTP/HTTPS 访问的远程서버

## JSON 설정구조

```json
{
  "mcpServers": {
    "서버名称": {
      "type": "http",           // 선택，기본 process
      "command": "npx",        // 프로세스패턴必需
      "url": "https://...",    // HTTP 패턴必需
      "args": ["参数1", "参数2"],  // 선택
      "env": {                 // 선택，环境변수
        "KEY": "value"
      },
      "headers": {             // 선택，HTTP 요청头
        "Header-Name": "value"
      },
      "description": "서버描述"  // 선택
    }
  }
}
```

## 字段설명

| 字段 | 유형 | 必需 | 설명 |
|------|------|------|------|
| `type` | string | 否 | 연결유형：`process`（기본）或 `http` |
| `command` | string | type=process 时必需 | 시작명령（如 `npx`, `uvx`, `python3`） |
| `url` | string | type=http 时必需 | MCP 서버的 HTTP/HTTPS URL |
| `args` | array | 否 | 传递给명령的参数数组 |
| `env` | object | 否 | 环境변수键值对 |
| `headers` | object | 否 | HTTP 요청头（仅 http 패턴） |
| `description` | string | 否 | 서버기능描述 |

## 설정예제

### 프로세스패턴예제

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

### HTTP 패턴예제

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Vercel 배포和프로젝트"
    }
  }
}
```

### 带인증头的 HTTP 패턴

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "YOUR_KEY_HERE"
      },
      "description": "AI 浏览器프록시"
    }
  }
}
```

## 모범 사례

1. **环境변수管理**：敏感정보（API keys, tokens）필수사용环境변수，不要硬인코딩
2. **上下文窗口**：권장保持활성화不超过 10 个 MCP 서버，以保留上下文窗口
3. **비활성화서버**：사용 `ECC_DISABLED_MCPS=server1,server2` 环境변수可비활성화打패키지的 MCP 서버

## 故障排除

- 确保명령可실행（npx, uvx 等已설치）
- 검사环境변수是否正确설정
- 查看 Claude Code 로그中的서버연결오류
