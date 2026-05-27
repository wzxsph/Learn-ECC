# Hệ Thống Tài Liệu ECC

ECC (Everything Claude Code) là plugin Claude Code cung cấp 75 lệnh, 60 Agent, 16 Skills, 7 tài liệu quy tắc và hệ thống cấu hình Hook/MCP hoàn chỉnh.

---

## 1. Tổng Quan Hệ Thống Tài Liệu ECC

ECC cung cấp hỗ trợ workflow phát triển production cho Claude Code, bao gồm nhiều AI Agent framework (Claude Code, Codex, OpenCode, Cursor, Gemini).

### Tech Stack

| Tầng | Công nghệ | Phiên bản |
|------|------|------|
| Ngôn ngữ chính | JavaScript/Node.js | >=18 |
| Ngôn ngữ phụ | Python | >=3.11 |
| Package manager | Yarn | 4.9.2 |
| Test runner | Custom Node.js test runner | - |
| Coverage | c8 | 11.x |
| Code linting | ESLint + markdownlint-cli | - |

### Cấu Trúc Thư Mục

```
ECC/
├── agents/        # 60 Agent chuyên môn
├── commands/      # 75 file lệnh
├── skills/        # 16 thư mục Skills
├── rules/         # 7 tài liệu quy tắc (common + ngôn ngữ)
├── hooks/         # 3 tài liệu Hook
├── mcp/           # 6 cấu hình MCP server
├── scripts/       # 54 script tiện ích
├── README.md
```

---

## 2. Điều Hướng Nhanh

| Phân loại | Tài liệu | Mô tả |
|------|------|------|
| [Tổng quan lệnh](./commands/) | [commands/](commands/) | Danh sách đầy đủ 75 lệnh |
| [Chỉ mục Agent](./agents/) | [agents/](agents/) | 60 Agent chuyên môn |
| [Chỉ mục Skills](./skills/) | [skills/](skills/) | 16 Skills phân theo domain |
| [Chỉ mục Rules](./rules/) | [rules/](rules/) | 7 tài liệu quy tắc (common + ngôn ngữ) |
| [Chỉ mục Hooks](./hooks/) | [hooks/](hooks/) | Cấu hình và phát triển Hook system |
| [Chỉ mục MCP](./mcp/) | [mcp/](mcp/) | Cấu hình và phát triển MCP server |
| [Chỉ mục Scripts](./scripts/) | [scripts/](scripts/) | 54 script tiện ích |

---

## 3. Chỉ Mục Lệnh

Tổng cộng **75 lệnh**, phân theo nhóm:

### 3.1 Core Workflow (6)

| Lệnh | File | Mục đích |
|------|------|------|
| `/plan` | [01-Workflow-Co-Ban.md](commands/01-Workflow-Co-Ban.md) | Phân tích yêu cầu + đánh giá rủi ro + kế hoạch từng bước |
| `/code-review` | [01-Workflow-Co-Ban.md](commands/01-Workflow-Co-Ban.md) | Review chất lượng/bảo mật/bảo trì mã |
| `/build-fix` | [01-Workflow-Co-Ban.md](commands/01-Workflow-Co-Ban.md) | Tự động phát hiện ngôn ngữ + sửa lỗi build tăng dần |
| `/verify` | [01-Workflow-Co-Ban.md](commands/01-Workflow-Co-Ban.md) | Xác minh đầy đủ: build → lint → test → type check |
| `/quality-gate` | [01-Workflow-Co-Ban.md](commands/01-Workflow-Co-Ban.md) | Chạy ECC quality pipeline theo yêu cầu |
| `/tdd` | [01-Workflow-Co-Ban.md](commands/01-Workflow-Co-Ban.md) | Workflow TDD tổng quát |

### 3.2 Kiểm Thử (8)

| Lệnh | File | Mục đích |
|------|------|------|
| `/go-test` | [02-Kiem-Thu.md](commands/02-Kiem-Thu.md) | Go TDD (table-driven, 80%+ coverage) |
| `/kotlin-test` | [02-Kiem-Thu.md](commands/02-Kiem-Thu.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-Kiem-Thu.md](commands/02-Kiem-Thu.md) | Rust TDD (cargo test + integration test) |
| `/cpp-test` | [02-Kiem-Thu.md](commands/02-Kiem-Thu.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-Kiem-Thu.md](commands/02-Kiem-Thu.md) | Flutter TDD |
| `/e2e` | [02-Kiem-Thu.md](commands/02-Kiem-Thu.md) | Tạo + chạy Playwright e2e test |
| `/test-coverage` | [02-Kiem-Thu.md](commands/02-Kiem-Thu.md) | Báo cáo coverage test + phân tích khoảng trống |
| `/python-testing` | [02-Kiem-Thu.md](commands/02-Kiem-Thu.md) | Best practice Python testing |

### 3.3 Review Ngôn Ngữ (7)

| Lệnh | File | Mục đích |
|------|------|------|
| `/python-review` | [03-Xem-Xet-Ngon-Ngu.md](commands/03-Xem-Xet-Ngon-Ngu.md) | Python PEP 8, type hints, bảo mật |
| `/go-review` | [03-Xem-Xet-Ngon-Ngu.md](commands/03-Xem-Xet-Ngon-Ngu.md) | Go idioms, concurrency, xử lý lỗi |
| `/kotlin-review` | [03-Xem-Xet-Ngon-Ngu.md](commands/03-Xem-Xet-Ngon-Ngu.md) | Kotlin null safety, coroutines, architecture |
| `/rust-review` | [03-Xem-Xet-Ngon-Ngu.md](commands/03-Xem-Xet-Ngon-Ngu.md) | Rust ownership, lifecycle, unsafe |
| `/cpp-review` | [03-Xem-Xet-Ngon-Ngu.md](commands/03-Xem-Xet-Ngon-Ngu.md) | C++ memory safety, idiom hiện đại |
| `/flutter-review` | [03-Xem-Xet-Ngon-Ngu.md](commands/03-Xem-Xet-Ngon-Ngu.md) | Flutter/Dart pattern |
| `/fastapi-review` | [03-Xem-Xet-Ngon-Ngu.md](commands/03-Xem-Xet-Ngon-Ngu.md) | FastAPI architecture, async, Pydantic |

### 3.4 Sửa Lỗi Build (6)

| Lệnh | File | Mục đích |
|------|------|------|
| `/go-build` | [04-Sua-Loi-Build.md](commands/04-Sua-Loi-Build.md) | Sửa lỗi Go build + cảnh báo go vet |
| `/kotlin-build` | [04-Sua-Loi-Build.md](commands/04-Sua-Loi-Build.md) | Sửa lỗi Kotlin/Gradle compiler |
| `/rust-build` | [04-Sua-Loi-Build.md](commands/04-Sua-Loi-Build.md) | Sửa Rust build + vấn đề borrow checker |
| `/cpp-build` | [04-Sua-Loi-Build.md](commands/04-Sua-Loi-Build.md) | Sửa C++ CMake + vấn đề linker |
| `/gradle-build` | [04-Sua-Loi-Build.md](commands/04-Sua-Loi-Build.md) | Sửa Android/KMP Gradle errors |
| `/flutter-build` | [04-Sua-Loi-Build.md](commands/04-Sua-Loi-Build.md) | Flutter build fix |

### 3.5 Quy Hoạch và Kiến Trúc (7)

| Lệnh | File | Mục đích |
|------|------|------|
| `/plan-prd` | [05-Quy-Hoach-Kien-Truc.md](commands/05-Quy-Hoach-Kien-Truc.md) | Interactive PRD generator |
| `/prp-plan` | [05-Quy-Hoach-Kien-Truc.md](commands/05-Quy-Hoach-Kien-Truc.md) | Lập kế hoạch feature toàn diện |
| `/prp-prd` | [05-Quy-Hoach-Kien-Truc.md](commands/05-Quy-Hoach-Kien-Truc.md) | PRP workflow PRD generator |
| `/prp-implement` | [05-Quy-Hoach-Kien-Truc.md](commands/05-Quy-Hoach-Kien-Truc.md) | Thực thi PRP plan + vòng xác minh |
| `/prp-pr` | [05-Quy-Hoach-Kien-Truc.md](commands/05-Quy-Hoach-Kien-Truc.md) | Tạo PR từ PRP workflow |
| `/prp-commit` | [05-Quy-Hoach-Kien-Truc.md](commands/05-Quy-Hoach-Kien-Truc.md) | PRP verified commit |
| `/multi-plan` | [05-Quy-Hoach-Kien-Truc.md](commands/05-Quy-Hoach-Kien-Truc.md) | Lập kế hoạch đa mô hình (Codex + Gemini) |

### 3.6 Quản Lý Phiên (5)

| Lệnh | File | Mục đích |
|------|------|------|
| `/save-session` | [06-Quan-Ly-Phien.md](commands/06-Quan-Ly-Phien.md) | Lưu trạng thái phiên vào ~/.claude/session-data/ |
| `/resume-session` | [06-Quan-Ly-Phien.md](commands/06-Quan-Ly-Phien.md) | Load và khôi phục phiên đã lưu |
| `/sessions` | [06-Quan-Ly-Phien.md](commands/06-Quan-Ly-Phien.md) | Duyệt + tìm kiếm + quản lý lịch sử phiên |
| `/checkpoint` | [06-Quan-Ly-Phien.md](commands/06-Quan-Ly-Phien.md) | Tạo/xác minh workflow checkpoint |
| `/aside` | [06-Quan-Ly-Phien.md](commands/06-Quan-Ly-Phien.md) | Trả lời câu hỏi phụ mà không mất context |

### 3.7 Học Hỏi và Cải Tiến (10)

| Lệnh | File | Mục đích |
|------|------|------|
| `/learn` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Trích xuất pattern tái sử dụng từ phiên |
| `/learn-eval` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Trích xuất pattern + tự đánh giá chất lượng |
| `/evolve` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Phân tích instinct + đề xuất cấu trúc tiến hóa |
| `/promote` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Nâng cấp project instinct lên phạm vi global |
| `/instinct-status` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Hiển thị tất cả instinct đã học |
| `/instinct-export` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Export instinct ra file |
| `/instinct-import` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Import instinct từ file/URL |
| `/skill-create` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Phân tích git history + tạo skill file |
| `/skill-health` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Skill composition health dashboard |
| `/rules-distill` | [07-Hoc-Hoi-Cai-Tien.md](commands/07-Hoc-Hoi-Cai-Tien.md) | Quét skills + trích xuất nguyên tắc cross-domain |

### 3.8 Vòng Lặp và Tự Động Hóa (3)

| Lệnh | File | Mục đích |
|------|------|------|
| `/loop-start` | [08-Vong-Lap-Tu-Dong.md](commands/08-Vong-Lap-Tu-Dong.md) | Khởi động chế độ autonomous loop được quản lý |
| `/loop-status` | [08-Vong-Lap-Tu-Dong.md](commands/08-Vong-Lap-Tu-Dong.md) | Kiểm tra trạng thái loop đang chạy |
| `/santa-loop` | [08-Vong-Lap-Tu-Dong.md](commands/08-Vong-Lap-Tu-Dong.md) | Autonomous loop kiểu Santa |

### 3.9 Quản Lý Dự Án (6)

| Lệnh | File | Mục đích |
|------|------|------|
| `/projects` | [09-Quan-Ly-Du-An.md](commands/09-Quan-Ly-Du-An.md) | Liệt kê dự án đã biết + thống kê instinct |
| `/harness-audit` | [09-Quan-Ly-Du-An.md](commands/09-Quan-Ly-Du-An.md) | Audit agent harness configuration |
| `/model-route` | [09-Quan-Ly-Du-An.md](commands/09-Quan-Ly-Du-An.md) | Route task đến model phù hợp |
| `/pm2` | [09-Quan-Ly-Du-An.md](commands/09-Quan-Ly-Du-An.md) | PM2 process manager initialization |
| `/setup-pm` | [09-Quan-Ly-Du-An.md](commands/09-Quan-Ly-Du-An.md) | Cấu hình package manager (npm/pnpm/yarn/bun) |
| `/project-init` | [09-Quan-Ly-Du-An.md](commands/09-Quan-Ly-Du-An.md) | Khởi tạo dự án |

### 3.10 PR Workflow (6)

| Lệnh | File | Mục đích |
|------|------|------|
| `/pr` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Tạo GitHub PR từ nhánh hiện tại |
| `/review-pr` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Review GitHub PR |
| `/multi-workflow` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Phát triển đa mô hình cộng tác |
| `/multi-backend` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Phát triển backend đa mô hình |
| `/multi-frontend` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Phát triển frontend đa mô hình |
| `/multi-execute` | [10-Workflow-PR.md](commands/10-Workflow-PR.md) | Thực thi cộng tác đa mô hình |

### 3.11 Hookify System (4)

| Lệnh | File | Mục đích |
|------|------|------|
| `/hookify` | [11-He-Thong-Hookify.md](commands/11-He-Thong-Hookify.md) | Tạo hooks ngăn chặn hành vi xấu |
| `/hookify-list` | [11-He-Thong-Hookify.md](commands/11-He-Thong-Hookify.md) | Liệt kê tất cả quy tắc hookify đã cấu hình |
| `/hookify-configure` | [11-He-Thong-Hookify.md](commands/11-He-Thong-Hookify.md) | Tương tác enable/disable quy tắc hookify |
| `/hookify-help` | [11-He-Thong-Hookify.md](commands/11-He-Thong-Hookify.md) | Trợ giúp hệ thống Hookify |

### 3.12 Tài Liệu và Nghiên Cứu (3)

| Lệnh | File | Mục đích |
|------|------|------|
| `/update-docs` | [12-Tai-Lieu-Nghien-Cuu.md](commands/12-Tai-Lieu-Nghien-Cuu.md) | Cập nhật tài liệu dự án |
| `/update-codemaps` | [12-Tai-Lieu-Nghien-Cuu.md](commands/12-Tai-Lieu-Nghien-Cuu.md) | Tái tạo codemaps |
| `/ecc-guide` | [12-Tai-Lieu-Nghien-Cuu.md](commands/12-Tai-Lieu-Nghien-Cuu.md) | Hướng dẫn sử dụng ECC |

### 3.13 Refactor và Dọn Dẹp (2)

| Lệnh | File | Mục đích |
|------|------|------|
| `/refactor-clean` | [13-Tai-Cau-Don-Dep.md](commands/13-Tai-Cau-Don-Dep.md) | Xóa dead code + gộp trùng lặp |
| `/auto-update` | [13-Tai-Cau-Don-Dep.md](commands/13-Tai-Cau-Don-Dep.md) | Khả năng cập nhật tự động |

### 3.14 Các Lệnh Khác (7)

| Lệnh | File | Mục đích |
|------|------|------|
| `/jira` | [14-Cac-Lenh-Khac.md](commands/14-Cac-Lenh-Khac.md) | Tương tác Jira ticket |
| `/gan-build` | [14-Cac-Lenh-Khac.md](commands/14-Cac-Lenh-Khac.md) | Thao tác GAN build |
| `/gan-design` | [14-Cac-Lenh-Khac.md](commands/14-Cac-Lenh-Khac.md) | Thao tác GAN design |
| `/prune` | [14-Cac-Lenh-Khac.md](commands/14-Cac-Lenh-Khac.md) | Xóa instinct cũ (>30 ngày) |
| `/security-scan` | [14-Cac-Lenh-Khac.md](commands/14-Cac-Lenh-Khac.md) | Security scan |
| `/feature-dev` | [14-Cac-Lenh-Khac.md](commands/14-Cac-Lenh-Khac.md) | Trợ lý phát triển feature |
| `/cost-report` | [14-Cac-Lenh-Khac.md](commands/14-Cac-Lenh-Khac.md) | Báo cáo chi phí model |

---

## 4. Chỉ Mục Agent

Tổng cộng **60 Agent**, phân theo nhóm:

| Agent Category | File | Mô tả |
|------------|------|------|
| [Code Review](agents/01-Kiem-Duyet-Ma.md) | [01-Kiem-Duyet-Ma.md](agents/01-Kiem-Duyet-Ma.md) | 14 Agent review |
| [Build Fix](agents/02-Sua-Loi-Build.md) | [02-Sua-Loi-Build.md](agents/02-Sua-Loi-Build.md) | 14 Agent sửa build |
| [Planning](agents/03-Lap-Trinh.md) | [03-Lap-Trinh.md](agents/03-Lap-Trinh.md) | 5 Agent lập kế hoạch |
| [Testing](agents/04-Kiem-Thu.md) | [04-Kiem-Thu.md](agents/04-Kiem-Thu.md) | 2 Agent kiểm thử |
| [Security](agents/05-Bao-Mat.md) | [05-Bao-Mat.md](agents/05-Bao-Mat.md) | 3 Agent bảo mật |
| [Architecture](agents/06-Kien-Truc.md) | [06-Kien-Truc.md](agents/06-Kien-Truc.md) | 3 Agent kiến trúc |

---

## 5. Chỉ Mục Skills

**16 Skills domain**, phân theo domain:

| Domain | File | Mô tả |
|------|------|------|
| [Best Practices](skills/Thuc-Hanh-Tot-Nhat.md) | [Thuc-Hanh-Tot-Nhat.md](skills/Thuc-Hanh-Tot-Nhat.md) | Coding standard, xử lý lỗi, autonomous loop |
| [Programming Languages](skills/Ngon-Ngu-Lap-Trinh.md) | [Ngon-Ngu-Lap-Trinh.md](skills/Ngon-Ngu-Lap-Trinh.md) | Python/Go/Rust/Kotlin/C++ v.v |
| [Framework](skills/Framework.md) | [Framework.md](skills/Framework.md) | Django/Laravel/NestJS/Spring Boot v.v |
| [Testing](skills/Kiem-Thu.md) | [Kiem-Thu.md](skills/Kiem-Thu.md) | TDD/Unit test/Integration test/E2E |
| [Security](skills/Bao-Mat.md) | [Bao-Mat.md](skills/Bao-Mat.md) | Security review, vulnerability scanning |
| [Frontend & Design](skills/Frontend-va-Thiet-Ke.md) | [Frontend-va-Thiet-Ke.md](skills/Frontend-va-Thiet-Ke.md) | Frontend development, design system |
| [Backend & API](skills/Backend-va-API.md) | [Backend-va-API.md](skills/Backend-va-API.md) | Backend service, API design, database |
| [Deployment & DevOps](skills/Trien-Khai-va-DevOps.md) | [Trien-Khai-va-DevOps.md](skills/Trien-Khai-va-DevOps.md) | Docker/K8s/deployment strategy |
| [Monitoring & Observability](skills/Giam-Sat-va-Quan-Sat.md) | [Giam-Sat-va-Quan-Sat.md](skills/Giam-Sat-va-Quan-Sat.md) | Observability, network diagnostics |
| [Automation & Scripting](skills/Tu-Dong-Hoa-va-Script.md) | [Tu-Dong-Hoa-va-Script.md](skills/Tu-Dong-Hoa-va-Script.md) | Autonomous loop, continuous learning, agent engineering |
| [Search & Data Retrieval](skills/Tim-Kiem-va-Lay-Du-Lieu.md) | [Tim-Kiem-va-Lay-Du-Lieu.md](skills/Tim-Kiem-va-Lay-Du-Lieu.md) | Exa search, data scraping, MCP |
| [GitHub & Collaboration](skills/GitHub-va-Cong-Tac.md) | [GitHub-va-Cong-Tac.md](skills/GitHub-va-Cong-Tac.md) | GitHub workflow, code review |
| [AI & Machine Learning](skills/AI-va-Hoc-May.md) | [AI-va-Hoc-May.md](skills/AI-va-Hoc-May.md) | Neural network, PyTorch, MLOps |
| [Cloud Native & Infrastructure](skills/Cloud-Native-va-Ha-Tang.md) | [Cloud-Native-va-Ha-Tang.md](skills/Cloud-Native-va-Ha-Tang.md) | Kubernetes, Docker, Terraform |
| [Specialized Domain Skills](skills/Ky-Nang-Chuyen-Mon.md) | [Ky-Nang-Chuyen-Mon.md](skills/Ky-Nang-Chuyen-Mon.md) | Blockchain, game dev, audio/video, IoT |
| [Development Toolchain](skills/Bo-Cong-Cu-Phat-Trien.md) | [Bo-Cong-Cu-Phat-Trien.md](skills/Bo-Cong-Cu-Phat-Trien.md) | Testing framework, CI/CD, code quality |
| [Cutting Edge Technology](skills/Cong-Nghe-Tien-Tien.md) | [Cong-Nghe-Tien-Tien.md](skills/Cong-Nghe-Tien-Tien.md) | AI Agent, quantum computing, edge computing |

---

## 6. Chỉ Mục Rules

Tổng cộng **7 tài liệu quy tắc** (common + ngôn ngữ):

| Rule | File | Mô tả |
|------|------|------|
| [Git Workflow](rules/Git-Workflow.md) | [Git-Workflow.md](rules/Git-Workflow.md) | Git commit convention và PR workflow |
| [Hooks System](rules/He-Thong-Hooks.md) | [He-Thong-Hooks.md](rules/He-Thong-Hooks.md) | Hook configuration và usage guide |
| [Agent Orchestration](rules/Dien-Tro-Agent.md) | [Dien-Tro-Agent.md](rules/Dien-Tro-Agent.md) | Agent orchestration pattern |
| [Performance Optimization](rules/Toi-Uu-Hieu-Suat.md) | [Toi-Uu-Hieu-Suat.md](rules/Toi-Uu-Hieu-Suat.md) | Performance optimization guide |
| [Coding Style](rules/Phong-Cach-Lap-Trinh.md) | [Phong-Cach-Lap-Trinh.md](rules/Phong-Cach-Lap-Trinh.md) | Coding style convention |
| [Testing Rules](rules/Quy-Tac-Kiem-Thu.md) | [Quy-Tac-Kiem-Thu.md](rules/Quy-Tac-Kiem-Thu.md) | Testing requirement (80% coverage) |
| [Security Rules](rules/Quy-Tac-Bao-Mat.md) | [Quy-Tac-Bao-Mat.md](rules/Quy-Tac-Bao-Mat.md) | Security checklist |

---

## 7. Chỉ Mục Hooks

Tổng cộng **4 tài liệu**:

| Tài liệu | File | Mô tả |
|------|------|------|
| [Hook Types](hooks/Loai-Hook.md) | [Loai-Hook.md](hooks/Loai-Hook.md) | PreToolUse, PostToolUse, Stop types |
| [Built-in Hooks](hooks/Hooks-Dung-San.md) | [Hooks-Dung-San.md](hooks/Hooks-Dung-San.md) | Danh sách và sử dụng Hook built-in |
| [Custom Development](hooks/Phat-Trien-Tuy-Chinh.md) | [Phat-Trien-Tuy-Chinh.md](hooks/Phat-Trien-Tuy-Chinh.md) | Custom Hook development guide |
| [Configuration Format](hooks/Dinh-Dang-Cau-Hinh.md) | [Dinh-Dang-Cau-Hinh.md](hooks/Dinh-Dang-Cau-Hinh.md) | hooks.json configuration format |

---

## 8. Chỉ Mục MCP

Tổng cộng **6 MCP server configuration**:

| Tài liệu | File | Mô tả |
|------|------|------|
| [MCP Configuration Format](mcp/Dinh-Dang-Cau-Hinh-MCP.md) | [Dinh-Dang-Cau-Hinh-MCP.md](mcp/Dinh-Dang-Cau-Hinh-MCP.md) | MCP configuration file format |
| [Built-in Servers](mcp/May-Chu-Dung-San.md) | [May-Chu-Dung-San.md](mcp/May-Chu-Dung-San.md) | MCP server built-in |
| [Custom Development](mcp/Phat-Trien-Tuy-Chinh.md) | [Phat-Trien-Tuy-Chinh.md](mcp/Phat-Trien-Tuy-Chinh.md) | Custom MCP server development |

---

## 9. Chỉ Mục Scripts

Tổng cộng **54 tool script**:

| Tài liệu | File | Mô tả |
|------|------|------|
| [Tool Scripts](scripts/Trinh-Chay-Tien-Ich.md) | [Trinh-Chay-Tien-Ich.md](scripts/Trinh-Chay-Tien-Ich.md) | ecc.js, install-apply.js v.v |
| [Tool Function Library](scripts/Thu-Vien-Ham-Tien-Ich.md) | [Thu-Vien-Ham-Tien-Ich.md](scripts/Thu-Vien-Ham-Tien-Ich.md) | Shared function library |
| [Test Runner](scripts/Trinh-Chay-Kiem-Thu.md) | [Trinh-Chay-Kiem-Thu.md](scripts/Trinh-Chay-Kiem-Thu.md) | Test runner usage |
| [Build Scripts](scripts/Cac-Script-Build.md) | [Cac-Script-Build.md](scripts/Cac-Script-Build.md) | Build scripts |

---

## 10. Hướng Dẫn Đóng Góp

### Quy Tắc Đặt Tên File

- **Commands, Skills, Agents, Hooks**: lowercase + hyphen (ví dụ `code-review.md`)
- **Scripts**: camelCase hoặc kebab-case (ví dụ `session-start.js`)
- **Rules**: tổ chức theo ngôn ngữ/topic

### Command File Format

```yaml
---
description: "Mô tả ngắn"
argument-hint: "[Tham số tùy chọn]"
name: command-name
command: true
allowed_tools: ["Bash"]
---
```

### Agent File Format

```yaml
---
name: agent-name
description: Agent purpose
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

### Skill File Format

```markdown
# Skill Name

## When to Use
## How It Works
## Examples
```

### Quy Trình Submit

1. Tạo file mới hoặc sửa file hiện có
2. Đảm bảo tuân thủ quy tắc đặt tên
3. Chạy test: `node tests/run-all.js`
4. Chạy markdown lint: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. Tạo PR yêu cầu review

---

*Hệ Thống Tài Liệu ECC - Cung cấp workflow phát triển production cho Claude Code*