# ECC 문서 체계

ECC (Everything Claude Code)는 75개의 명령, 60개의 Agent, 16개의 Skills, 7개의 규칙 문서, 완전한 Hook/MCP 설정 체계를 제공하는 Claude Code 플러그인입니다.

---

## 1. ECC 문서 체계 개요

ECC는 여러 AI Agent 프레임워크(Claude Code, Codex, OpenCode, Cursor, Gemini)를 지원하는 Claude Code용 프로덕션급 개발 워크플로우를 제공합니다.

### 텍스택

| 레이어 | 기술 | 버전 |
|------|------|------|
| 주요 언어 | JavaScript/Node.js | >=18 |
| 보조 언어 | Python | >=3.11 |
| 패키지 관리자 | Yarn | 4.9.2 |
| 테스트 실행기 | 커스텀 Node.js 테스트 실행기 | - |
| 커버리지 | c8 | 11.x |
| 코드 검사 | ESLint + markdownlint-cli | - |

### 디렉토리 구조

```
ECC/
├── agents/        # 60개의 전문 Agent
├── commands/      # 75개의 명령 파일
├── skills/        # 16개의 Skills 디렉토리
├── rules/         # 7개의 규칙 문서(common + 언어 특정)
├── hooks/         # 3개의 Hook 문서
├── mcp/           # 6개의 MCP 서버 설정
├── scripts/       # 54개의 도구 스크립트
├── README.md
```

---

## 2. 빠른 탐색

| 분류 | 문서 | 설명 |
|------|------|------|
| [명령 총览](./commands/) | [commands/](commands/) | 75개 명령의 완전한 목록 |
| [Agent 인덱스](./agents/) | [agents/](agents/) | 60개의 전문 Agent |
| [Skills 인덱스](./skills/) | [skills/](skills/) | 16개의 Skills 분야별 분류 |
| [규칙 인덱스](./rules/) | [rules/](rules/) | 7개의 규칙 문서 (common + 언어 특정) |
| [Hooks 인덱스](./hooks/) | [hooks/](hooks/) | Hook 시스템 설정 및 개발 |
| [MCP 인덱스](./mcp/) | [mcp/](mcp/) | MCP 서버 설정 및 개발 |
| [Scripts 인덱스](./scripts/) | [scripts/](scripts/) | 54개의 도구 스크립트 |

---

## 3. 명령 인덱스

총 **75개 명령**, 분야별 그룹:

### 3.1 핵심 워크플로우 (6)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/plan` | [01-핵심 워크플로우.md](commands/01-핵심%20워크플로우.md) | 요구 분석 + 위험 평가 + 단계별 계획 |
| `/code-review` | [01-핵심 워크플로우.md](commands/01-핵심%20워크플로우.md) | 코드 품질/보안/유지보수성 리뷰 |
| `/build-fix` | [01-핵심 워크플로우.md](commands/01-핵심%20워크플로우.md) | 자동 언어 감지 + 증분 수정 빌드 오류 |
| `/verify` | [01-핵심 워크플로우.md](commands/01-핵심%20워크플로우.md) | 완전한 검증 루프: 빌드 → lint → 테스트 → 유형 검사 |
| `/quality-gate` | [01-핵심 워크플로우.md](commands/01-핵심%20워크플로우.md) | 요청 시 ECC 품질 파이프라인 실행 |
| `/tdd` | [01-핵심 워크플로우.md](commands/01-핵심%20워크플로우.md) | 범용 TDD 워크플로우 |

### 3.2 테스트 관련 (8)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/go-test` | [02-테스트 관련.md](commands/02-테스트%20관련.md) | Go TDD (표驱动, 80%+ 커버리지) |
| `/kotlin-test` | [02-테스트 관련.md](commands/02-테스트%20관련.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-테스트 관련.md](commands/02-테스트%20관련.md) | Rust TDD (cargo test + 통합 테스트) |
| `/cpp-test` | [02-테스트 관련.md](commands/02-테스트%20관련.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-테스트 관련.md](commands/02-테스트%20관련.md) | Flutter TDD |
| `/e2e` | [02-테스트 관련.md](commands/02-테스트%20관련.md) | Playwright e2e 테스트 생성 + 실행 |
| `/test-coverage` | [02-테스트 관련.md](commands/02-테스트%20관련.md) | 테스트 커버리지 보고서 + 격차 분석 |
| `/python-testing` | [02-테스트 관련.md](commands/02-테스트%20관련.md) | Python 테스트 모범 사례 |

### 3.3 언어 리뷰 (7)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/python-review` | [03-언어 리뷰.md](commands/03-언어%20리뷰.md) | Python PEP 8, 타입 힌트, 보안 |
| `/go-review` | [03-언어 리뷰.md](commands/03-언어%20리뷰.md) | Go 관용구, 동시성, 오류 처리 |
| `/kotlin-review` | [03-언어 리뷰.md](commands/03-언어%20리뷰.md) | Kotlin null 보안, 코루틴, 아키텍처 |
| `/rust-review` | [03-언어 리뷰.md](commands/03-언어%20리뷰.md) | Rust 소유권, 라이프사이클, unsafe |
| `/cpp-review` | [03-언어 리뷰.md](commands/03-언어%20리뷰.md) | C++ 메모리 안전성, 현대 idiom |
| `/flutter-review` | [03-언어 리뷰.md](commands/03-언어%20리뷰.md) | Flutter/Dart 패턴 |
| `/fastapi-review` | [03-언어 리뷰.md](commands/03-언어%20리뷰.md) | FastAPI 아키텍처, 비동기, Pydantic |

### 3.4 빌드 수정 (6)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/go-build` | [04-빌드 수정.md](commands/04-빌드%20수정.md) | Go 빌드 오류 수정 + go vet 경고 |
| `/kotlin-build` | [04-빌드 수정.md](commands/04-빌드%20수정.md) | Kotlin/Gradle 컴파일러 오류 수정 |
| `/rust-build` | [04-빌드 수정.md](commands/04-빌드%20수정.md) | Rust 빌드 + 빌로우 검사기 문제 수정 |
| `/cpp-build` | [04-빌드 수정.md](commands/04-빌드%20수정.md) | C++ CMake + 링커 문제 수정 |
| `/gradle-build` | [04-빌드 수정.md](commands/04-빌드%20수정.md) | Android/KMP Gradle 오류 수정 |
| `/flutter-build` | [04-빌드 수정.md](commands/04-빌드%20수정.md) | Flutter 빌드 수정 |

### 3.5 계획과 아키텍처 (7)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/plan-prd` | [05-계획과 아키텍처.md](commands/05-계획과%20아키텍처.md) | 대화형 PRD 생성기 |
| `/prp-plan` | [05-계획과 아키텍처.md](commands/05-계획과%20아키텍처.md) | 종합 기능 계획 |
| `/prp-prd` | [05-계획과 아키텍처.md](commands/05-계획과%20아키텍처.md) | PRP 워크플로우 PRD 생성기 |
| `/prp-implement` | [05-계획과 아키텍처.md](commands/05-계획과%20아키텍처.md) | PRP 계획 실행 + 검증 루프 |
| `/prp-pr` | [05-계획과 아키텍처.md](commands/05-계획과%20아키텍처.md) | PRP 워크플로우에서 PR 생성 |
| `/prp-commit` | [05-계획과 아키텍처.md](commands/05-계획과%20아키텍처.md) | PRP 검증 커밋 |
| `/multi-plan` | [05-계획과 아키텍처.md](commands/05-계획과%20아키텍처.md) | 다중 모델 협력 계획 (Codex + Gemini) |

### 3.6 세션 관리 (5)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/save-session` | [06-세션 관리.md](commands/06-세션%20관리.md) | 세션 상태를 ~/.claude/session-data/에 저장 |
| `/resume-session` | [06-세션 관리.md](commands/06-세션%20관리.md) | 저장된 세션 로드 및 복구 |
| `/sessions` | [06-세션 관리.md](commands/06-세션%20관리.md) | 세션 기록 浏览 + 검색 + 관리 |
| `/checkpoint` | [06-세션 관리.md](commands/06-세션%20관리.md) | 워크플로우 검사점 생성/검증 |
| `/aside` | [06-세션 관리.md](commands/06-세션%20관리.md) | 컨텍스트 손실 없이 질문에 답변 |

### 3.7 학습과 개선 (10)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/learn` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | 세션에서 재사용 가능한 패턴 추출 |
| `/learn-eval` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | 패턴 + 자기 평가 품질 추출 |
| `/evolve` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | 분석 직관 + 권장进化 구조 |
| `/promote` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | 프로젝트 직관을 전역 범위로 승격 |
| `/instinct-status` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | 모든 학습 직관 표시 |
| `/instinct-export` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | 직관을 파일로 내보내기 |
| `/instinct-import` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | 파일/URL에서 직관 가져오기 |
| `/skill-create` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | git histor에서 skill 파일 생성 |
| `/skill-health` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | Skill 조합 건강 대시보드 |
| `/rules-distill` | [07-학습과 개선.md](commands/07-학습과%20개선.md) | 스캔 skills + 분야 간 원칙 추출 |

### 3.8 루프와 자동화 (3)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/loop-start` | [08-루프와 자동화.md](commands/08-루프와%20자동화.md) | 관리형 자율 루프 패턴 시작 |
| `/loop-status` | [08-루프와 자동화.md](commands/08-루프와%20자동화.md) | 실행 중 루프 상태 검사 |
| `/santa-loop` | [08-루프와 자동화.md](commands/08-루프와%20자동화.md) | 산타 스타일 자율 루프 |

### 3.9 프로젝트 관리 (6)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/projects` | [09-프로젝트 관리.md](commands/09-프로젝트%20관리.md) | 알려진 프로젝트 + 직관 통계 나열 |
| `/harness-audit` | [09-프로젝트 관리.md](commands/09-프로젝트%20관리.md) | Agent harness 설정 감사 |
| `/model-route` | [09-프로젝트 관리.md](commands/09-프로젝트%20관리.md) | 작업을 적절한 모델로 라우팅 |
| `/pm2` | [09-프로젝트 관리.md](commands/09-프로젝트%20관리.md) | PM2 프로세스 관리자 초기화 |
| `/setup-pm` | [09-프로젝트 관리.md](commands/09-프로젝트%20관리.md) | 패키지 관리자 설정 (npm/pnpm/yarn/bun) |
| `/project-init` | [09-프로젝트 관리.md](commands/09-프로젝트%20관리.md) | 프로젝트 초기화 |

### 3.10 PR 워크플로우 (6)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/pr` | [10-PR 워크플로우.md](commands/10-PR%20워크플로우.md) | 현재 브랜치에서 GitHub PR 생성 |
| `/review-pr` | [10-PR 워크플로우.md](commands/10-PR%20워크플로우.md) | GitHub PR 리뷰 |
| `/multi-workflow` | [10-PR 워크플로우.md](commands/10-PR%20워크플로우.md) | 다중 모델 협력 개발 |
| `/multi-backend` | [10-PR 워크플로우.md](commands/10-PR%20워크플로우.md) | 백엔드 다중 모델 개발 |
| `/multi-frontend` | [10-PR 워크플로우.md](commands/10-PR%20워크플로우.md) | 프론트엔드 다중 모델 개발 |
| `/multi-execute` | [10-PR 워크플로우.md](commands/10-PR%20워크플로우.md) | 다중 모델 협력 실행 |

### 3.11 Hookify 시스템 (4)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/hookify` | [11-Hookify 시스템.md](commands/11-Hookify%20시스템.md) | 부적절한 행동 방지를 위한 hooks 생성 |
| `/hookify-list` | [11-Hookify 시스템.md](commands/11-Hookify%20시스템.md) | 설정된 모든 hookify 규칙 나열 |
| `/hookify-configure` | [11-Hookify 시스템.md](commands/11-Hookify%20시스템.md) | 대화형 hookify 규칙 활성화/비활성화 |
| `/hookify-help` | [11-Hookify 시스템.md](commands/11-Hookify%20시스템.md) | Hookify 시스템 도움말 |

### 3.12 문서와 연구 (3)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/update-docs` | [12-문서와 연구.md](commands/12-문서와%20연구.md) | 프로젝트 문서 업데이트 |
| `/update-codemaps` | [12-문서와 연구.md](commands/12-문서와%20연구.md) | codemaps 다시 생성 |
| `/ecc-guide` | [12-문서와 연구.md](commands/12-문서와%20연구.md) | ECC 사용자 가이드 |

### 3.13 리팩토링과 정리 (2)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/refactor-clean` | [13-리팩토링과 정리.md](commands/13-리팩토링과%20정리.md) | 죽은 코드 + 미사용 코드 삭제 |
| `/auto-update` | [13-리팩토링과 정리.md](commands/13-리팩토링과%20정리.md) | 자동 업데이트 기능 |

### 3.14 기타 명령 (7)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/jira` | [14-기타 명령.md](commands/14-기타%20명령.md) | Jira 티켓 상호작용 |
| `/gan-build` | [14-기타 명령.md](commands/14-기타%20명령.md) | GAN 빌드 작업 |
| `/gan-design` | [14-기타 명령.md](commands/14-기타%20명령.md) | GAN 설계 작업 |
| `/prune` | [14-기타 명령.md](commands/14-기타%20명령.md) | 오래된 직관 삭제 (>30일) |
| `/security-scan` | [14-기타 명령.md](commands/14-기타%20명령.md) | 보안 스캔 |
| `/feature-dev` | [14-기타 명령.md](commands/14-기타%20명령.md) | 기능 개발 어시스턴트 |
| `/cost-report` | [14-기타 명령.md](commands/14-기타%20명령.md) | 모델 비용 보고서 |

---

## 4. Agent 인덱스

총 **60개 Agent**, 분야별 그룹:

| Agent 분야 | 파일 | 설명 |
|------------|------|------|
| [코드 리뷰 담당](agents/1.%20코드%20리뷰%20담당.md) | [1. 코드 리뷰 담당.md](agents/1.%20코드%20리뷰%20담당.md) | 14개의 리뷰 Agent |
| [빌드 수정클래스](agents/2.%20빌드%20수정클래스.md) | [2. 빌드 수정클래스.md](agents/2.%20빌드%20수정클래스.md) | 14개의 빌드 수정 Agent |
| [계획類](agents/3.%20계획類.md) | [3. 계획類.md](agents/3.%20계획類.md) | 5개의 계획 Agent |
| [테스트類](agents/4.%20테스트類.md) | [4. 테스트類.md](agents/4.%20테스트類.md) | 2개의 테스트 Agent |
| [보안類](agents/5.%20보안類.md) | [5. 보안類.md](agents/5.%20보안類.md) | 3개의 보안 Agent |
| [아키텍처類](agents/6.%20아키텍처類.md) | [6. 아키텍처類.md](agents/6.%20아키텍처類.md) | 3개의 아키텍처 Agent |

---

## 5. Skills 인덱스

**16개 Skills 분야**, 분야별 분류:

| 분야 | 파일 | 설명 |
|------|------|------|
| [모범 사례](skills/모범%20사례.md) | [모범 사례.md](skills/모범%20사례.md) | 인코딩 표준, 오류 처리, 자율 루프 |
| [프로그래밍 언어](skills/프로그래밍%20언어.md) | [프로그래밍 언어.md](skills/프로그래밍%20언어.md) | Python/Go/Rust/Kotlin/C++ 등 |
| [프레임워크](skills/프레임워크.md) | [프레임워크.md](skills/프레임워크.md) | Django/Laravel/NestJS/Spring Boot 등 |
| [테스트](skills/테스트.md) | [테스트.md](skills/테스트.md) | TDD/단위 테스트/통합 테스트/E2E |
| [보안](skills/보안.md) | [보안.md](skills/보안.md) | 보안 리뷰, 취약점 스캔 |
| [프론트엔드와 디자인](skills/프론트엔드와%20디자인.md) | [프론트엔드와 디자인.md](skills/프론트엔드와%20디자인.md) | 프론트엔드 개발, 설계 시스템 |
| [백엔드와 API](skills/백엔드와%20API.md) | [백엔드와 API.md](skills/백엔드와%20API.md) | 백엔드 서비스, API 설계, 데이터베이스 |
| [배포와 DevOps](skills/배포와%20DevOps.md) | [배포와 DevOps.md](skills/배포와%20DevOps.md) | Docker/K8s/배포 전략 |
| [모니터링과 관측 가능성](skills/모니터링과%20관측%20가능성.md) | [모니터링과 관측 가능성.md](skills/모니터링과%20관측%20가능성.md) | 관측 가능성, 네트워크 진단 |
| [자동화와 스크립트](skills/자동화와%20스크립트.md) | [자동화와 스크립트.md](skills/자동화와%20스크립트.md) | 자율 루프, 지속 학습, 프록시 엔지니어링 |
| [검색과 데이터 취득](skills/검색과%20데이터%20취득.md) | [검색과 데이터 취득.md](skills/검색과%20데이터%20취득.md) | Exa 검색, 데이터 가져오기, MCP |
| [GitHub와 협업](skills/GitHub와%20협업.md) | [GitHub와 협업.md](skills/GitHub와%20협업.md) | GitHub 워크플로우, 코드 리뷰 |
| [AI와 머신러닝](skills/AI와%20머신러닝.md) | [AI와 머신러닝.md](skills/AI와%20머신러닝.md) | 신경망, PyTorch, MLOps |
| [클라우드 네이티브와 인프라](skills/클라우드%20네이티브와%20인프라.md) | [클라우드 네이티브와 인프라.md](skills/클라우드%20네이티브와%20인프라.md) | Kubernetes, Docker, Terraform |
| [특수 분야 기술](skills/특수%20분야%20기술.md) | [특수 분야 기술.md](skills/특수%20분야%20기술.md) | 블록체인, 게임 개발, 음성/비디오, IoT |
| [개발 도구 체인](skills/개발%20도구%20체인.md) | [개발 도구 체인.md](skills/개발%20도구%20체인.md) | 테스트 프레임워크, CI/CD, 코드 품질 |
| [최첨단 기술](skills/최첨단%20기술.md) | [최첨단 기술.md](skills/최첨단%20기술.md) | AI Agent, 양자 컴퓨팅, 에지 컴퓨팅 |

---

## 6. 규칙 인덱스

총 **7개의 규칙 문서** (common + 언어 특정):

| 규칙 | 파일 | 설명 |
|------|------|------|
| [Git 워크플로우](rules/Git%20워크플로우.md) | [Git 워크플로우.md](rules/Git%20워크플로우.md) | Git 커밋 사양 및 PR 워크플로우 |
| [Hooks 시스템](rules/Hooks%20시스템.md) | [Hooks 시스템.md](rules/Hooks%20시스템.md) | Hook 설정 및 사용 가이드 |
| [Agent 오케스트레이션](rules/Agent%20오케스트레이션.md) | [Agent 오케스트레이션.md](rules/Agent%20오케스트레이션.md) | Agent 편집 패턴 |
| [성능 최적화](rules/성능%20최적화.md) | [성능 최적화.md](rules/성능%20최적화.md) | 성능 최적화 가이드 |
| [코딩 스타일](rules/코딩%20스타일.md) | [코딩 스타일.md](rules/코딩%20스타일.md) | 인코딩 스타일 사양 |
| [테스트 규칙](rules/테스트%20규칙.md) | [테스트 규칙.md](rules/테스트%20규칙.md) | 테스트 요구 (80% 커버리지) |
| [보안 규칙](rules/보안%20규칙.md) | [보안 규칙.md](rules/보안%20규칙.md) | 보안 검사 목록 |

---

## 7. Hooks 인덱스

총 **4개의 문서**:

| 문서 | 파일 | 설명 |
|------|------|------|
| [Hook 유형](hooks/Hook%20유형.md) | [Hook 유형.md](hooks/Hook%20유형.md) | PreToolUse, PostToolUse, Stop 유형 |
| [내장 Hooks](hooks/내장%20Hooks.md) | [내장 Hooks.md](hooks/내장%20Hooks.md) | 내장 Hook 목록 및 사용 |
| [사용자 정의 개발](hooks/사용자%20정의%20개발.md) | [사용자 정의 개발.md](hooks/사용자%20정의%20개발.md) | 커스텀 Hook 개발 가이드 |
| [설정 형식](hooks/설정%20형식.md) | [설정 형식.md](hooks/설정%20형식.md) | hooks.json 설정 형식 |

---

## 8. MCP 인덱스

총 **6개의 MCP 서버 설정**:

| 문서 | 파일 | 설명 |
|------|------|------|
| [MCP 설정 형식](mcp/MCP%20설정%20형식.md) | [MCP 설정 형식.md](mcp/MCP%20설정%20형식.md) | MCP 설정 파일 형식 |
| [내장 서버](mcp/내장%20서버.md) | [내장 서버.md](mcp/내장%20서버.md) | 내장 MCP 서버 |
| [사용자 정의 개발](mcp/사용자%20정의%20개발.md) | [사용자 정의 개발.md](mcp/사용자%20정의%20개발.md) | 커스텀 MCP 서버 개발 |

---

## 9. Scripts 인덱스

총 **54개의 도구 스크립트**:

| 문서 | 파일 | 설명 |
|------|------|------|
| [도구 스크립트](scripts/도구%20스크립트.md) | [도구 스크립트.md](scripts/도구%20스크립트.md) | ecc.js, install-apply.js 등 |
| [도구 함수 라이브러리](scripts/도구%20함수%20라이브러리.md) | [도구 함수 라이브러리.md](scripts/도구%20함수%20라이브러리.md) | 공유 함수 라이브러리 |
| [테스트 실행기](scripts/테스트%20실행기.md) | [테스트 실행기.md](scripts/테스트%20실행기.md) | 테스트 실행기 사용 |
| [빌드 스크립트](scripts/빌드%20스크립트.md) | [빌드 스크립트.md](scripts/빌드%20스크립트.md) | 빌드 스크립트 |

---

## 10. 기여 가이드

### 파일 명명 사양

- **명령, Skills, Agents, Hooks**: 소문자 + 하이픈 (예: `code-review.md`)
- **스크립트**: camelCase 또는 kebab-case (예: `session-start.js`)
- **규칙**: 언어/주제별 구성

### 명령 파일 형식

```yaml
---
description: "간단한 설명"
argument-hint: "[선택 매개변수]"
name: command-name
command: true
allowed_tools: ["Bash"]
---
```

### Agent 파일 형식

```yaml
---
name: agent-name
description: Agent 용도
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

### Skill 파일 형식

```markdown
# Skill 이름

## When to Use
## How It Works
## Examples
```

### 커밋 프로세스

1. 새 파일 생성 또는 기존 파일 수정
2. 명명 사양 준수 확인
3. 테스트 실행: `node tests/run-all.js`
4. Markdown lint 실행: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. PR로 리뷰 요청

---

*ECC 문서 체계 - Claude Code용 프로덕션급 개발 워크플로우 제공*