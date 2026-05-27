# MCP 설정 형식

## 개요

MCP (Model Context Protocol) 서버는 `~/.claude.json` 파일의 `mcpServers` 섹션을 통해 설정됩니다. Claude Code는 두 가지 유형의 MCP 서버를 지원합니다:

- **프로세스 패턴 (process)**: 로컬 명령으로 시작하는 서버
- **HTTP 패턴 (http)**: HTTP/HTTPS로 접근하는 원격 서버

## JSON 설정 구조

```json
{
  "mcpServers": {
    "서버 이름": {
      "type": "http",           // 선택, 기본 process
      "command": "npx",        // 프로세스 패턴必需
      "url": "https://...",    // HTTP 패턴必需
      "args": ["매개변수1", "매개변수2"],  // 선택
      "env": {                 // 선택, 환경 변수
        "KEY": "value"
      },
      "headers": {             // 선택, HTTP 요청 헤더
        "Header-Name": "value"
      },
      "description": "서버 설명"  // 선택
    }
  }
}
```

## 필드 설명

| 필드 | 유형 | 필수 | 설명 |
|------|------|------|------|
| `type` | string | 아니오 | 연결 유형: `process` (기본) 또는 `http` |
| `command` | string | type=process 시 필수 | 시작 명령 (예: `npx`, `uvx`, `python3`) |
| `url` | string | type=http 시 필수 | MCP 서버의 HTTP/HTTPS URL |
| `args` | array | 아니오 | 명령에 전달할 매개변수 배열 |
| `env` | object | 아니오 | 환경 변수 키-값 쌍 |
| `headers` | object | 아니오 | HTTP 요청 헤더 (http 패턴만) |
| `description` | string | 아니오 | 서버 기능 설명 |

## 설정 예제

### 프로세스 패턴 예제

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "GitHub 작업 - PRs, issues, repos"
    }
  }
}
```

### HTTP 패턴 예제

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Vercel 배포 및 프로젝트"
    }
  }
}
```

### 인증 헤더가 있는 HTTP 패턴

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "YOUR_KEY_HERE"
      },
      "description": "AI 브라우저 프록시"
    }
  }
}
```

## 모범 사례

1. **환경 변수 관리**: 민감한 정보 (API 키, 토큰)는 반드시 환경 변수를 사용하고, 하드코딩 금지
2. **컨텍스트 창**: 활성 MCP 서버를 10개 이하로 유지하여 컨텍스트 창 보존 권장
3. **비활성화 서버**: `ECC_DISABLED_MCPS=server1,server2` 환경 변수로 비활성화할 수 있음

## 문제 해결

- 명령이 실행 가능한지 확인 (npx, uvx 등 설치됨)
- 환경 변수가 올바르게 설정되었는지 검사
- Claude Code 로그에서 서버 연결 오류 확인