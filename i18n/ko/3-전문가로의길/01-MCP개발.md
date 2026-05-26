# 01-MCP 개발

모델 컨텍스트 프로토콜（Model Context Protocol）확장 개발을 학습합니다.

## MCP란?

MCP（Model Context Protocol）는 Claude와 외부 도구 및 서비스를 통합하기 위한 프로토콜입니다. MCP를 통해 Claude의 기능을 확장하여 데이터베이스, API, 다른 AI 모델 등에 연결할 수 있습니다.

> 참고: [MCP 설정 형식](../참조문서/mcp/MCP配置格式.md)

## 학습 목표

- MCP 아키텍처와 원리 이해
- MCP 서버 생성
- MCP 도구 구현
- MCP 확장 테스트 및 디버깅

## 핵심 개념

### 1. MCP 서버
MCP 서버는 도구와 리소스를 제공하는 프로세스입니다.

### 2. 도구（Tools）
Claude가 호출하여 실행할 수 있는 작업입니다.

### 3. 리소스（Resources）
Claude가 읽을 수 있는 데이터 소스입니다.

### 4. 프롬프트（Prompts）
사전 정의된 프롬프트 템플릿입니다.

## MCP 설정

MCP 설정 파일은 `.claude/mcp.json`에 위치합니다：

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

## 다음 단계

- [자율 에이전트](./02-자율-에이전트.md) - 자율형 Agent 구축
- [전문가之路 디렉토리로 돌아가기](../README.md)