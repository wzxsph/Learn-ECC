# ECC 문서体系

ECC (Everything Claude Code) 是一个 Claude Code 플러그인，提供 75 个명령、60 个 Agent、16 个 Skills、7 个규칙문서和完整的 Hook/MCP 설정体系。

---

## 1. ECC 문서体系개요

ECC 为 Claude Code 提供生产级개발워크플로우지원，涵盖多个 AI Agent 프레임워크（Claude Code、Codex、OpenCode、Cursor、Gemini）。

### 텍스택

| 레이어 | 기술 | 버전 |
|------|------|------|
| 主要언어 | JavaScript/Node.js | >=18 |
| 次要언어 | Python | >=3.11 |
| 패키지 관리자 | Yarn | 4.9.2 |
| 테스트 실행기 | 自定义 Node.js 테스트 실행기 | - |
| 커버리지 | c8 | 11.x |
| 코드검사 | ESLint + markdownlint-cli | - |

### 디렉토리 구조

```
ECC/
├── agents/        # 60 个专业 Agent
├── commands/      # 75 个명령파일
├── skills/        # 16 个 Skills 디렉토리
├── rules/         # 7 个규칙문서（common + 언어特定）
├── hooks/         # 3 个 Hook 문서
├── mcp/           # 6 个 MCP 서버설정
├── scripts/       # 54 个도구 스크립트
├── README.md
```

---

## 2. 빠른 탐색

| 분류 | 문서 | 설명 |
|------|------|------|
| [명령总览](./commands/) | [commands/](commands/) | 75 个명령的완전한 목록 |
| [Agent 인덱스](./agents/) | [agents/](agents/) | 60 个专业 Agent |
| [Skills 인덱스](./skills/) | [skills/](skills/) | 16 个 Skills 按领域분류 |
| [규칙인덱스](./rules/) | [rules/](rules/) | 7 个규칙문서 (common + 언어特定) |
| [Hooks 인덱스](./hooks/) | [hooks/](hooks/) | Hook 시스템설정和개발 |
| [MCP 인덱스](./mcp/) | [mcp/](mcp/) | MCP 서버설정和개발 |
| [Scripts 인덱스](./scripts/) | [scripts/](scripts/) | 54 个도구 스크립트 |

---

## 3. 명령인덱스

共 **75 个명령**，按클래스别分组：

### 3.1 핵심 워크플로우 (6)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/plan` | [01-핵심 워크플로우.md](commands/01-핵심 워크플로우.md) | 需求분석+风险评估+分단계계획 |
| `/code-review` | [01-핵심 워크플로우.md](commands/01-핵심 워크플로우.md) | 코드 품질/보안/유지보수성리뷰 |
| `/build-fix` | [01-핵심 워크플로우.md](commands/01-핵심 워크플로우.md) | 자동检测언어+增量수정빌드오류 |
| `/verify` | [01-핵심 워크플로우.md](commands/01-핵심 워크플로우.md) | 完整검증루프：빌드→lint→테스트→유형검사 |
| `/quality-gate` | [01-핵심 워크플로우.md](commands/01-핵심 워크플로우.md) | 按需실행 ECC 품질流水线 |
| `/tdd` | [01-핵심 워크플로우.md](commands/01-핵심 워크플로우.md) | 通用 TDD 워크플로우 |

### 3.2 테스트 관련 (8)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/go-test` | [02-테스트 관련.md](commands/02-테스트 관련.md) | Go TDD (表格驱动，80%+ 커버리지) |
| `/kotlin-test` | [02-테스트 관련.md](commands/02-테스트 관련.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-테스트 관련.md](commands/02-테스트 관련.md) | Rust TDD (cargo test + 통합 테스트) |
| `/cpp-test` | [02-테스트 관련.md](commands/02-테스트 관련.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-테스트 관련.md](commands/02-테스트 관련.md) | Flutter TDD |
| `/e2e` | [02-테스트 관련.md](commands/02-테스트 관련.md) | 生成 + 실행 Playwright e2e 테스트 |
| `/test-coverage` | [02-테스트 관련.md](commands/02-테스트 관련.md) | 테스트커버리지보고서+差距분석 |
| `/python-testing` | [02-테스트 관련.md](commands/02-테스트 관련.md) | Python 테스트모범 사례 |

### 3.3 언어 리뷰 (7)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/python-review` | [03-언어 리뷰.md](commands/03-언어 리뷰.md) | Python PEP 8、타입 힌트、보안 |
| `/go-review` | [03-언어 리뷰.md](commands/03-언어 리뷰.md) | Go 惯用法、동시성、오류 처리 |
| `/kotlin-review` | [03-언어 리뷰.md](commands/03-언어 리뷰.md) | Kotlin 空보안、코루틴、아키텍처 |
| `/rust-review` | [03-언어 리뷰.md](commands/03-언어 리뷰.md) | Rust 소유권、라이프사이클、unsafe |
| `/cpp-review` | [03-언어 리뷰.md](commands/03-언어 리뷰.md) | C++ 메모리 안전성、现代 idiom |
| `/flutter-review` | [03-언어 리뷰.md](commands/03-언어 리뷰.md) | Flutter/Dart 패턴 |
| `/fastapi-review` | [03-언어 리뷰.md](commands/03-언어 리뷰.md) | FastAPI 아키텍처、비동기、Pydantic |

### 3.4 빌드 수정 (6)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/go-build` | [04-빌드 수정.md](commands/04-빌드 수정.md) | 수정 Go 빌드오류 + go vet 경고 |
| `/kotlin-build` | [04-빌드 수정.md](commands/04-빌드 수정.md) | 수정 Kotlin/Gradle 컴파일器오류 |
| `/rust-build` | [04-빌드 수정.md](commands/04-빌드 수정.md) | 수정 Rust 빌드 + 借用검사器문제 |
| `/cpp-build` | [04-빌드 수정.md](commands/04-빌드 수정.md) | 수정 C++ CMake + 链接器문제 |
| `/gradle-build` | [04-빌드 수정.md](commands/04-빌드 수정.md) | 수정 Android/KMP Gradle 오류 |
| `/flutter-build` | [04-빌드 수정.md](commands/04-빌드 수정.md) | Flutter 빌드 수정 |

### 3.5 계획과 아키텍처 (7)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/plan-prd` | [05-계획과 아키텍처.md](commands/05-계획과 아키텍처.md) | 交互式 PRD 生成器 |
| `/prp-plan` | [05-계획과 아키텍처.md](commands/05-계획과 아키텍처.md) | 全面的기능계획 |
| `/prp-prd` | [05-계획과 아키텍처.md](commands/05-계획과 아키텍처.md) | PRP 워크플로우 PRD 生成器 |
| `/prp-implement` | [05-계획과 아키텍처.md](commands/05-계획과 아키텍처.md) | 실행 PRP 계획+검증루프 |
| `/prp-pr` | [05-계획과 아키텍처.md](commands/05-계획과 아키텍처.md) | 从 PRP 워크플로우생성 PR |
| `/prp-commit` | [05-계획과 아키텍처.md](commands/05-계획과 아키텍처.md) | PRP 검증커밋 |
| `/multi-plan` | [05-계획과 아키텍처.md](commands/05-계획과 아키텍처.md) | 多모델协作계획 (Codex + Gemini) |

### 3.6 세션 관리 (5)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/save-session` | [06-세션 관리.md](commands/06-세션 관리.md) | 저장세션状态到 ~/.claude/session-data/ |
| `/resume-session` | [06-세션 관리.md](commands/06-세션 관리.md) | 加载并복구저장的세션 |
| `/sessions` | [06-세션 관리.md](commands/06-세션 관리.md) | 浏览+搜索+管理세션历史 |
| `/checkpoint` | [06-세션 관리.md](commands/06-세션 관리.md) | 생성/검증워크플로우검사点 |
| `/aside` | [06-세션 관리.md](commands/06-세션 관리.md) | 回答侧问而不丢失上下文 |

### 3.7 학습과 개선 (10)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/learn` | [07-학습과 개선.md](commands/07-학습과 개선.md) | 从세션中提取可复用패턴 |
| `/learn-eval` | [07-학습과 개선.md](commands/07-학습과 개선.md) | 提取패턴 + 自我评估품질 |
| `/evolve` | [07-학습과 개선.md](commands/07-학습과 개선.md) | 분석 instinct + 권장进化구조 |
| `/promote` | [07-학습과 개선.md](commands/07-학습과 개선.md) | 将프로젝트 instinct 提升到全局范围 |
| `/instinct-status` | [07-학습과 개선.md](commands/07-학습과 개선.md) | 표시모든학습的 instinct |
| `/instinct-export` | [07-학습과 개선.md](commands/07-학습과 개선.md) | 내보내기 instinct 到파일 |
| `/instinct-import` | [07-학습과 개선.md](commands/07-학습과 개선.md) | 从파일/URL 가져오기 instinct |
| `/skill-create` | [07-학습과 개선.md](commands/07-학습과 개선.md) | 분석 git 历史+生成 skill 파일 |
| `/skill-health` | [07-학습과 개선.md](commands/07-학습과 개선.md) | Skill 组合健康仪表板 |
| `/rules-distill` | [07-학습과 개선.md](commands/07-학습과 개선.md) | 扫描 skills + 提取跨领域원칙 |

### 3.8 루프와 자동화 (3)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/loop-start` | [08-루프와 자동화.md](commands/08-루프와 자동화.md) | 시작托管自主루프패턴 |
| `/loop-status` | [08-루프와 자동화.md](commands/08-루프와 자동화.md) | 검사실행中루프的状态 |
| `/santa-loop` | [08-루프와 자동화.md](commands/08-루프와 자동화.md) | Santa 风格自主루프 |

### 3.9 프로젝트 관리 (6)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/projects` | [09-프로젝트 관리.md](commands/09-프로젝트 관리.md) | 列出已知프로젝트 + instinct 통계 |
| `/harness-audit` | [09-프로젝트 관리.md](commands/09-프로젝트 관리.md) | 审计 agent harness 설정 |
| `/model-route` | [09-프로젝트 관리.md](commands/09-프로젝트 관리.md) | 라우팅태스크到合适모델 |
| `/pm2` | [09-프로젝트 관리.md](commands/09-프로젝트 관리.md) | PM2 프로세스管理器초기화 |
| `/setup-pm` | [09-프로젝트 관리.md](commands/09-프로젝트 관리.md) | 설정패키지 관리자 (npm/pnpm/yarn/bun) |
| `/project-init` | [09-프로젝트 관리.md](commands/09-프로젝트 관리.md) | 프로젝트초기화 |

### 3.10 PR 워크플로우 (6)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/pr` | [10-PR 워크플로우.md](commands/10-PR 워크플로우.md) | 从当前브랜치생성 GitHub PR |
| `/review-pr` | [10-PR 워크플로우.md](commands/10-PR 워크플로우.md) | 리뷰 GitHub PR |
| `/multi-workflow` | [10-PR 워크플로우.md](commands/10-PR 워크플로우.md) | 多모델协作개발 |
| `/multi-backend` | [10-PR 워크플로우.md](commands/10-PR 워크플로우.md) | 백엔드多모델개발 |
| `/multi-frontend` | [10-PR 워크플로우.md](commands/10-PR 워크플로우.md) | 프론트엔드多모델개발 |
| `/multi-execute` | [10-PR 워크플로우.md](commands/10-PR 워크플로우.md) | 多모델协作실행 |

### 3.11 Hookify 시스템 (4)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/hookify` | [11-Hookify 시스템.md](commands/11-Hookify 시스템.md) | 생성 hooks 防止不良行为 |
| `/hookify-list` | [11-Hookify 시스템.md](commands/11-Hookify 시스템.md) | 列出모든설정的 hookify 규칙 |
| `/hookify-configure` | [11-Hookify 시스템.md](commands/11-Hookify 시스템.md) | 交互式활성화/비활성화 hookify 규칙 |
| `/hookify-help` | [11-Hookify 시스템.md](commands/11-Hookify 시스템.md) | Hookify 시스템帮助 |

### 3.12 문서와 연구 (3)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/update-docs` | [12-문서와 연구.md](commands/12-문서와 연구.md) | 업데이트프로젝트문서 |
| `/update-codemaps` | [12-문서와 연구.md](commands/12-문서와 연구.md) | 重新生成 codemaps |
| `/ecc-guide` | [12-문서와 연구.md](commands/12-문서와 연구.md) | ECC 用户가이드 |

### 3.13 리팩토링과 정리 (2)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/refactor-clean` | [13-리팩토링과 정리.md](commands/13-리팩토링과 정리.md) | 삭제死코드+머지重复 |
| `/auto-update` | [13-리팩토링과 정리.md](commands/13-리팩토링과 정리.md) | 자동업데이트能力 |

### 3.14 기타 명령 (7)

| 명령 | 파일 | 용도 |
|------|------|------|
| `/jira` | [14-기타 명령.md](commands/14-기타 명령.md) | Jira 工单交互 |
| `/gan-build` | [14-기타 명령.md](commands/14-기타 명령.md) | GAN 빌드操作 |
| `/gan-design` | [14-기타 명령.md](commands/14-기타 명령.md) | GAN 설계操作 |
| `/prune` | [14-기타 명령.md](commands/14-기타 명령.md) | 삭제陈旧 instinct (>30天) |
| `/security-scan` | [14-기타 명령.md](commands/14-기타 명령.md) | 보안扫描 |
| `/feature-dev` | [14-기타 명령.md](commands/14-기타 명령.md) | 기능개발助手 |
| `/cost-report` | [14-기타 명령.md](commands/14-기타 명령.md) | 모델成本보고서 |

---

## 4. Agent 인덱스

共 **60 个 Agent**，按클래스别分组：

| Agent 클래스别 | 파일 | 설명 |
|------------|------|------|
| [코드 리뷰 담당](agents/1.%20코드 리뷰 담당.md) | [1. 코드 리뷰 담당.md](agents/1.%20코드 리뷰 담당.md) | 14 个리뷰 Agent |
| [빌드 수정클래스](agents/2.%20빌드 수정클래스.md) | [2. 빌드 수정클래스.md](agents/2.%20빌드 수정클래스.md) | 14 个빌드 수정 Agent |
| [계획類](agents/3.%20계획類.md) | [3. 계획類.md](agents/3.%20계획類.md) | 5 个계획 Agent |
| [테스트類](agents/4.%20테스트類.md) | [4. 테스트類.md](agents/4.%20테스트類.md) | 2 个테스트 Agent |
| [보안類](agents/5.%20보안類.md) | [5. 보안類.md](agents/5.%20보안類.md) | 3 个보안 Agent |
| [아키텍처類](agents/6.%20아키텍처類.md) | [6. 아키텍처類.md](agents/6.%20아키텍처類.md) | 3 个아키텍처 Agent |

---

## 5. Skills 인덱스

$**16 个 Skills 领域**，按领域분류：

| 领域 | 파일 | 설명 |
|------|------|------|
| [모범 사례](skills/모범 사례.md) | [모범 사례.md](skills/모범 사례.md) | 인코딩표준、오류 처리、自主루프 |
| [프로그래밍 언어](skills/프로그래밍 언어.md) | [프로그래밍 언어.md](skills/프로그래밍 언어.md) | Python/Go/Rust/Kotlin/C++ 等 |
| [프레임워크](skills/프레임워크.md) | [프레임워크.md](skills/프레임워크.md) | Django/Laravel/NestJS/Spring Boot 等 |
| [테스트](skills/테스트.md) | [테스트.md](skills/테스트.md) | TDD/단위 테스트/통합 테스트/E2E |
| [보안](skills/보안.md) | [보안.md](skills/보안.md) | 보안리뷰、漏洞扫描 |
| [프론트엔드와 디자인](skills/프론트엔드와 디자인.md) | [프론트엔드와 디자인.md](skills/프론트엔드와 디자인.md) | 프론트엔드개발、설계시스템 |
| [백엔드와 API](skills/백엔드와 API.md) | [백엔드와 API.md](skills/백엔드와 API.md) | 백엔드서비스、API 설계、데이터베이스 |
| [배포와 DevOps](skills/배포와 DevOps.md) | [배포와 DevOps.md](skills/배포와 DevOps.md) | Docker/K8s/배포전략 |
| [모니터링과 관측 가능성](skills/모니터링과 관측 가능성.md) | [모니터링과 관측 가능성.md](skills/모니터링과 관측 가능성.md) | 관측 가능성、网络진단 |
| [자동화와 스크립트](skills/자동화와 스크립트.md) | [자동화와 스크립트.md](skills/자동화와 스크립트.md) | 自主루프、持续학습、프록시工程 |
| [검색과 데이터 취득](skills/검색과 데이터 취득.md) | [검색과 데이터 취득.md](skills/검색과 데이터 취득.md) | Exa 搜索、데이터抓取、MCP |
| [GitHub와 협업](skills/GitHub와 협업.md) | [GitHub와 협업.md](skills/GitHub와 협업.md) | GitHub 워크플로우、코드 리뷰 |
| [AI와 머신러닝](skills/AI와 머신러닝.md) | [AI와 머신러닝.md](skills/AI와 머신러닝.md) | 神经网络、PyTorch、MLOps |
| [클라우드 네이티브와 인프라](skills/클라우드 네이티브와 인프라.md) | [클라우드 네이티브와 인프라.md](skills/클라우드 네이티브와 인프라.md) | Kubernetes、Docker、Terraform |
| [특수 분야 기술](skills/특수 분야 기술.md) | [특수 분야 기술.md](skills/특수 분야 기술.md) | 区块链、游戏개발、音视频、IoT |
| [개발 도구 체인](skills/개발 도구 체인.md) | [개발 도구 체인.md](skills/개발 도구 체인.md) | 테스트프레임워크、CI/CD、코드 품질 |
| [최첨단 기술](skills/최첨단 기술.md) | [최첨단 기술.md](skills/최첨단 기술.md) | AI Agent、量子计算、边缘计算 |

---

## 6. 규칙인덱스

共 **7 个규칙문서**（common + 언어特定）：

| 규칙 | 파일 | 설명 |
|------|------|------|
| [Git 워크플로우](rules/Git 워크플로우.md) | [Git 워크플로우.md](rules/Git 워크플로우.md) | Git 커밋사양和 PR 워크플로우 |
| [Hooks 시스템](rules/Hooks 시스템.md) | [Hooks 시스템.md](rules/Hooks 시스템.md) | Hook 설정和사용가이드 |
| [Agent 오케스트레이션](rules/Agent 오케스트레이션.md) | [Agent 오케스트레이션.md](rules/Agent 오케스트레이션.md) | Agent 编排패턴 |
| [성능 최적화](rules/성능 최적화.md) | [성능 최적화.md](rules/성능 최적화.md) | 성능 최적화가이드 |
| [코딩 스타일](rules/코딩 스타일.md) | [코딩 스타일.md](rules/코딩 스타일.md) | 인코딩风格사양 |
| [테스트 규칙](rules/테스트 규칙.md) | [테스트 규칙.md](rules/테스트 규칙.md) | 테스트요구（80% 커버리지） |
| [보안 규칙](rules/보안 규칙.md) | [보안 규칙.md](rules/보안 규칙.md) | 보안검사清单 |

---

## 7. Hooks 인덱스

共 **4 个문서**：

| 문서 | 파일 | 설명 |
|------|------|------|
| [Hook 유형](hooks/Hook 유형.md) | [Hook 유형.md](hooks/Hook 유형.md) | PreToolUse、PostToolUse、Stop 유형 |
| [内置 Hooks](hooks/내장 Hooks.md) | [내장 Hooks.md](hooks/내장 Hooks.md) | 内置 Hook 列表和사용 |
| [사용자 정의 개발](hooks/사용자 정의 개발.md) | [사용자 정의 개발.md](hooks/사용자 정의 개발.md) | 自定义 Hook 개발가이드 |
| [설정 형식](hooks/설정 형식.md) | [설정 형식.md](hooks/설정 형식.md) | hooks.json 설정 형식 |

---

## 8. MCP 인덱스

共 **6 个 MCP 서버설정**：

| 문서 | 파일 | 설명 |
|------|------|------|
| [MCP 설정 형식](mcp/MCP 설정 형식.md) | [MCP 설정 형식.md](mcp/MCP 설정 형식.md) | MCP 설정파일형식 |
| [내장 서버](mcp/내장 서버.md) | [내장 서버.md](mcp/내장 서버.md) | 内置 MCP 서버 |
| [사용자 정의 개발](mcp/사용자 정의 개발.md) | [사용자 정의 개발.md](mcp/사용자 정의 개발.md) | 自定义 MCP 서버개발 |

---

## 9. Scripts 인덱스

共 **54 个도구 스크립트**：

| 문서 | 파일 | 설명 |
|------|------|------|
| [도구 스크립트](scripts/도구 스크립트.md) | [도구 스크립트.md](scripts/도구 스크립트.md) | ecc.js、install-apply.js 等 |
| [도구 함수 라이브러리](scripts/도구 함수 라이브러리.md) | [도구 함수 라이브러리.md](scripts/도구 함수 라이브러리.md) | 共享함수库 |
| [테스트 실행기](scripts/테스트 실행기.md) | [테스트 실행기.md](scripts/테스트 실행기.md) | 테스트 실행기사용 |
| [빌드 스크립트](scripts/빌드 스크립트.md) | [빌드 스크립트.md](scripts/빌드 스크립트.md) | 빌드 스크립트 |

---

## 10. 贡献가이드

### 파일命名사양

- **명령、Skills、Agents、Hooks**: 小写 + 连字符（如 `code-review.md`）
- **스크립트**: camelCase 或 kebab-case（如 `session-start.js`）
- **규칙**: 按언어/主题组织

### 명령파일형식

```yaml
---
description: "简短描述"
argument-hint: "[선택参数]"
name: command-name
command: true
allowed_tools: ["Bash"]
---
```

### Agent 파일형식

```yaml
---
name: agent-name
description: Agent 용도
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

### Skill 파일형식

```markdown
# Skill 名称

## When to Use
## How It Works
## Examples
```

### 커밋流程

1. 생성新파일或修改现있음파일
2. 确保遵循命名사양
3. 실행테스트: `node tests/run-all.js`
4. 실행 markdown lint: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. 생성 PR 요청리뷰

---

*ECC 문서体系 - 为 Claude Code 提供生产级개발워크플로우*
