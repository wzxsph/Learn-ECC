# 01 - Giới thiệu [ECC](../TaiLieuThamKhao/README.md)

## [ECC](../TaiLieuThamKhao/README.md) là gì?

[ECC](../TaiLieuThamKhao/README.md) (Everything Claude Code) là hệ thống plugin Claude Code, cung cấp [Agent](../TaiLieuThamKhao/agents/1.%20代码审查类.md), [Skills](../TaiLieuThamKhao/skills/最佳实践.md), hooks, rules, [Commands](../TaiLieuThamKhao/commands/01-核心工作流.md) và cấu hình MCP production-grade. Xuất phát từ dự án đoạt giải Anthropic Hackathon, đã được sử dụng thực tế hơn 10 tháng.

## [ECC](../TaiLieuThamKhao/README.md) có thể làm gì?

### 1. Hệ thống [Agent](../TaiLieuThamKhao/agents/1.%20代码审查类.md)
[ECC](../TaiLieuThamKhao/README.md) cung cấp 60+ [Agent](../TaiLieuThamKhao/agents/1.%20代码审查类.md) chuyên nghiệp, bao gồm review đa ngôn ngữ, sửa lỗi build, thiết kế kiến trúc, v.v.:

| [Agent](../TaiLieuThamKhao/agents/1.%20代码审查类.md) | Mục đích |
|-------|------|
| `planner` | Lập kế hoạch triển khai tính năng |
| `code-reviewer` | Review chất lượng và bảo mật code |
| `security-reviewer` | Phân tích lỗ hổng bảo mật |
| `build-error-resolver` | Sửa lỗi build |
| `architect` | Thiết kế kiến trúc hệ thống |
| `tdd-guide` | Hướng dẫn phát triển TDD |

### 2. Workflow [Skills](../TaiLieuThamKhao/skills/最佳实践.md)
[Skills](../TaiLieuThamKhao/skills/最佳实践.md) là bề mặt workflow chính, [ECC](../TaiLieuThamKhao/README.md) cung cấp 232+ [Skills](../TaiLieuThamKhao/skills/最佳实践.md):

- `tdd-workflow` - Phát triển driven bằng test
- `e2e-testing` - Test đầu cuối
- `security-review` - Checklist review bảo mật
- `continuous-learning-v2` - Học tập liên tục dựa trên instinct
- `eval-harness` - Evaluation vòng lặp verification

### 3. [Commands](../TaiLieuThamKhao/commands/01-核心工作流.md)
Giữ lại tính tương thích với lệnh gạch chéo truyền thống (75+ lệnh):

| Lệnh | Chức năng |
|------|------|
| `/plan` | Tạo kế hoạch triển khai |
| `/code-review` | Review code |
| `/build-fix` | Sửa lỗi build |
| `/learn` | Trích xuất pattern từ session |
| `/skill-create` | Tạo Skill từ lịch sử Git |
| `/sessions` | Quản lý lịch sử session |

### 4. Tự động hóa Hooks
Hệ thống hook linh hoạt (8 loại sự kiện):
- `SessionStart` - Load context khi bắt đầu session
- `SessionEnd` - Lưu trạng thái khi kết thúc session
- `PreToolUse` - Validate trước khi thực thi tool
- `PostToolUse` - Format sau khi thực thi tool

### 5. Hệ thống Rules
Quy tắc phát triển toàn diện (common + ngôn ngữ cụ thể):
- **common/** - Nguyên tắc chung (bảo mật, coding style, test, performance)
- **typescript/** - TypeScript/JavaScript
- **python/** - Python
- **golang/** - Go
- **swift/** - Swift
- **php/** - PHP
- **arkts/** - HarmonyOS/ArkTS

### 6. Dịch vụ MCP
Hỗ trợ cấu hình 14+ MCP server: GitHub, Supabase, Vercel, Railway, Context7, Exa, Playwright, v.v.

## Hỗ trợ đa nền tảng

[ECC](../TaiLieuThamKhao/README.md) hỗ trợ nhiều công cụ lập trình AI:

| Công cụ | Agents | Commands | Skills | Hooks |
|------|--------|----------|--------|-------|
| Claude Code | 60 | 75 | 232 | 8 types |
| Cursor | 48 | Chia sẻ | Chia sẻ | 15 types |
| Codex | Chia sẻ | Dạng指令式 | 32 | - |
| OpenCode | 12 | 35 | 37 | 11 types |
| GitHub Copilot | - | 6 prompts | Dạng指令式 | - |

## Khái niệm cốt lõi

### [Agent](../TaiLieuThamKhao/agents/1.%20代码审查类.md)
Người thực thi sub-task có thể tái sử dụng, mỗi [Agent](../TaiLieuThamKhao/agents/1.%20代码审查类.md) có trách nhiệm rõ ràng. Xây dựng workflow phức tạp bằng cách kết hợp nhiều [Agent](../TaiLieuThamKhao/agents/1.%20代码审查类.md).

### [Skill](../TaiLieuThamKhao/skills/最佳实践.md)
Định nghĩa workflow hoàn thành task cụ thể, là bề mặt workflow chính của [ECC](../TaiLieuThamKhao/README.md).

### Hook
Script tự động trigger tại thời điểm cụ thể, hỗ trợ tự động hóa workflow.

### Rule
Ràng buộc mà Claude Code phải tuân thủ, đảm bảo chất lượng output.

### MCP Server
Server hỗ trợ Model Context Protocol, kết nối với dịch vụ bên ngoài.

---

## Tiếp theo

- [Cài đặt](./02-CaiDat.md) - Bắt đầu sử dụng [ECC](../TaiLieuThamKhao/README.md)
- [Quay lề thư mục nhập môn](../README.md)