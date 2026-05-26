# Agent 치트 시트

## 내장 Agent

### 계획류
| Agent | 용도 | 특징 |
|-------|------|------|
| `planner` | [실시 계획](../참조문서/agents/6.%20架构类.md) | 작업 분해, 단계 생성 |
| `architect` | [시스템 설계](../참조문서/agents/6.%20架构类.md) | 아키텍처 결정, 기술 선택 |

### 검토류
| Agent | 용도 | 특징 |
|-------|------|------|
| `code-reviewer` | 코드 검토 | 품질 관리, 패턴 검사 |
| `security-reviewer` | 보안 검토 | 취약점 발견, OWASP |
| `rust-reviewer` | Rust 검토 | 생명주기, 빌림 검사 |

### 개발류
| Agent | 용도 | 특징 |
|-------|------|------|
| `tdd-guide` | [TDD 가이드](../참조문서/agents/1.%20代码审查类.md) | 테스트 우선, 증분 리팩토링 |
| `build-error-resolver` | [빌드 수정](../참조문서/agents/2.%20构建修复类.md) | 오류 위치 파악, 수정 제안 |

### 운영류
| Agent | 용도 | 특징 |
|-------|------|------|
| `e2e-runner` | [E2E 테스트](../참조문서/agents/1.%20代码审查类.md) | 주요 경로, 자동화 |
| `refactor-cleaner` | [리팩토링 정리](../참조문서/agents/1.%20代码审查类.md) | 죽은 코드 제거 |

## Agent 호출 방식

```
/[agent-name]                    # 직접 호출
/[agent-name] [parameters]       # 매개변수 포함
/[agent-name] --option value    # 옵션 매개변수
```

## 사용자 정의 Agent

Agent 파일 형식: `agents/*.md` + YAML frontmatter

```yaml
---
name: my-agent
description: 내 사용자 정의 Agent
tools: [Read, Bash, Edit]
model: sonnet
---
```

---

[빠른 참조 디렉토리로 돌아가기](./README.md)