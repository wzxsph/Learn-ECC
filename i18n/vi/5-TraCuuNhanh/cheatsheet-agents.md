# Tra cứu Agent

## Agent tích hợp sẵn

### Lớp lập kế hoạch
| Agent | Mục đích | Đặc điểm |
|-------|------|------|
| `planner` | [Lập kế hoạch thực hiện](../TaiLieuThamKhao/agents/6.%20架构类.md) | Phân rã nhiệm vụ, tạo các bước |
| `architect` | [Thiết kế hệ thống](../TaiLieuThamKhao/agents/6.%20架构类.md) | Quyết định kiến trúc, lựa chọn công nghệ |

### Lớp kiểm tra
| Agent | Mục đích | Đặc điểm |
|-------|------|------|
| `code-reviewer` | Kiểm tra mã | Kiểm soát chất lượng, kiểm tra mẫu |
| `security-reviewer` | Kiểm tra bảo mật | Phát hiện lỗ hổng, OWASP |
| `rust-reviewer` | Kiểm tra Rust | Vòng đời, kiểm tra mượn |

### Lớp phát triển
| Agent | Mục đích | Đặc điểm |
|-------|------|------|
| `tdd-guide` | [Hướng dẫn TDD](../TaiLieuThamKhao/agents/1.%20代码审查类.md) | Kiểm thử trước, tái cấu trúc tăng dần |
| `build-error-resolver` | [Sửa lỗi build](../TaiLieuThamKhao/agents/2.%20构建修复类.md) | Định vị lỗi, đề xuất sửa chữa |

### Lớp vận hành
| Agent | Mục đích | Đặc điểm |
|-------|------|------|
| `e2e-runner` | [Kiểm thử E2E](../TaiLieuThamKhao/agents/1.%20代码审查类.md) | Luồng chính, tự động hóa |
| `refactor-cleaner` | [Tái cấu trúc và dọn dẹp](../TaiLieuThamKhao/agents/1.%20代码审查类.md) | Loại bỏ mã chết |

## Cách gọi Agent

```
/[agent-name]                    # Gọi trực tiếp
/[agent-name] [parameters]       # Có tham số
/[agent-name] --option value    # Tham số tùy chọn
```

## Agent tùy chỉnh

Định dạng tệp Agent: `agents/*.md` + YAML frontmatter

```yaml
---
name: my-agent
description: Agent tùy chỉnh của tôi
tools: [Read, Bash, Edit]
model: sonnet
---
```

---

[Quay lại mục lục tra cứu nhanh](./README.md)