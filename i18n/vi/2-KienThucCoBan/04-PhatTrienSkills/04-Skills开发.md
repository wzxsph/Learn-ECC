# 04 - Phát triển Skills

Học cách tạo Skills workflow có thể tái sử dụng.

## Tổng quan

ECC cung cấp **234+ [Skills](../../TaiLieuThamKhao/skills/最佳实践.md)**，按领域分类，涵盖 best practices、编程语言、框架、测试、安全、前端与设计、后端与API、部署与DevOps 等 16 个领域。

---

## 1. Format Skill

### 1.1 Cấu trúc cơ bản

Skills sử dụng định dạng Markdown, bao gồm cấu trúc chương rõ ràng:

```markdown
# Tên Skill

## When to Use
## How It Works
## Examples
```

### 1.2 YAML Frontmatter

```yaml
---
name: skill-name
description: Mô tả mục đích Skill
category: Phân loại
---
```

### 1.3 Cấu trúc chương

| Chương | Mô tả |
|------|------|
| **When to Use** | Khi nào sử dụng Skill này |
| **How It Works** | Nguyên lý hoạt động và các bước |
| **Examples** | Ví dụ sử dụng |

---

## 2. Chỉ mục Skills

### 2.1 Phân loại theo lĩnh vực

| Lĩnh vực | Mô tả |
|------|------|
| [最佳实践](../../TaiLieuThamKhao/skills/最佳实践.md) | Tiêu chuẩn coding, xử lý lỗi, vòng lặp tự trị |
| [编程语言](../../TaiLieuThamKhao/skills/编程语言.md) | Python/Go/Rust/Kotlin/C++ 等 |
| [框架](../../TaiLieuThamKhao/skills/框架.md) | Django/Laravel/NestJS/Spring Boot 等 |
| [测试](../../TaiLieuThamKhao/skills/测试.md) | TDD/单元测试/集成测试/E2E |
| [安全](../../TaiLieuThamKhao/skills/安全.md) | Review bảo mật, quét lỗ hổng |
| [前端与设计](../../TaiLieuThamKhao/skills/前端与设计.md) | Phát triển frontend, hệ thống thiết kế |
| [后端与API](../../TaiLieuThamKhao/skills/后端与API.md) | Dịch vụ backend, thiết kế API, database |
| [部署与DevOps](../../TaiLieuThamKhao/skills/部署与DevOps.md) | Docker/K8s/Chiến lược deploy |
| [监控与可观测性](../../TaiLieuThamKhao/skills/监控与可观测性.md) | Khả năng quan sát, chẩn đoán mạng |
| [自动化与脚本](../../TaiLieuThamKhao/skills/自动化与脚本.md) | Vòng lặp tự trị, học tập liên tục, engineering đại lý |
| [搜索与数据获取](../../TaiLieuThamKhao/skills/搜索与数据获取.md) | Tìm kiếm Exa, thu thập dữ liệu, MCP |
| [GitHub与协作](../../TaiLieuThamKhao/skills/GitHub与协作.md) | Workflow GitHub, code review |
| [AI与机器学习](../../TaiLieuThamKhao/skills/AI与机器学习.md) | Mạng nơ-ron, PyTorch, MLOps |
| [云原生与基础设施](../../TaiLieuThamKhao/skills/云原生与基础设施.md) | Kubernetes, Docker, Terraform |
| [特殊领域技能](../../TaiLieuThamKhao/skills/特殊领域技能.md) | Blockchain, phát triển game, âm thanh video, IoT |
| [开发工具链](../../TaiLieuThamKhao/skills/开发工具链.md) | Framework test, CI/CD, chất lượng code |
| [前沿技术](../../TaiLieuThamKhao/skills/前沿技术.md) | AI Agent, tính toán lượng tử, tính toán biên |

### 2.2 Skills phổ biến

| Skill | Mục đích |
|-------|------|
| TDD Workflow | Quy trình chuẩn phát triển driven bằng test |
| Code Review | Checklist kiểm tra chất lượng code |
| Security Review | Checklist OWASP Top 10 |
| API Design | Nguyên tắc thiết kế RESTful API |
| Docker Deploy | Best practices deploy container |
| CI/CD Pipeline | Tích hợp và deploy liên tục |

---

## 3. Tạo Skill tùy chỉnh

### 3.1 Các bước tạo

1. **Xác định mục đích**：Rõ ràng vấn đề Skill cần giải quyết
2. **Viết cấu trúc**：Theo cấu trúc When to Use / How It Works / Examples
3. **Thêm YAML frontmatter**：Cung cấp metadata
4. **Lưu vào thư mục skills/**：Đặt tên bằng chữ thường + dấu gạch nối

### 3.2 Ví dụ Skill

```markdown
---
name: my-custom-skill
description: Workflow thực hiện nhiệm vụ cụ thể
category: Tùy chỉnh
---

# My Custom Skill

## When to Use

Sử dụng Skill này khi cần thực hiện nhiệm vụ X.

## How It Works

1. Bước 1: Chuẩn bị dữ liệu
2. Bước 2: Thực hiện thao tác cốt lõi
3. Bước 3: Xác minh kết quả
4. Bước 4: Dọn dẹp tài nguyên

## Examples

### Ví dụ 1: Cách dùng cơ bản

```
Thực hiện My Custom Skill để hoàn thành X
```

### Ví dụ 2: Cách dùng nâng cao

```
Thực hiện My Custom Skill với tham số tùy chỉnh
```
```

### 3.3 Quy tắc đặt tên

| Loại | Quy tắc | Ví dụ |
|------|------|------|
| Tên file | Chữ thường + dấu gạch nối | `code-review.md`, `tdd-workflow.md` |
| Thư mục | Tổ chức theo lĩnh vực | `skills/编程语言/` |
| Vị trí | Project skills/ hoặc toàn cục ~/.claude/skills/ | |

---

## 4. Thiết kế Workflow

### 4.1 Định nghĩa các bước

Định nghĩa rõ ràng từng bước:

```markdown
## How It Works

1. **Phân tích yêu cầu**：Hiểu nhu cầu người dùng
2. **Thiết kế phương án**：Xây dựng phương án thực hiện
3. **Implement code**：Viết code
4. **Xác minh test**：Đảm bảo chất lượng
5. **Cập nhật tài liệu**：Ghi lại thay đổi
```

### 4.2 Phân nhánh có điều kiện

Xử lý các kịch bản khác nhau:

```markdown
## How It Works

1. Phân tích input
2. Phân nhánh theo điều kiện:
   - Nếu là A, thực hiện bước X
   - Nếu là B, thực hiện bước Y
   - Ngược lại, thực hiện bước Z
3. Tổng hợp kết quả
```

### 4.3 Xử lý lỗi

Xử lý lỗi chuẩn hóa:

```markdown
## How It Works

1. Thực hiện thao tác
2. Kiểm tra kết quả
   - Thành công: Tiếp tục bước tiếp theo
   - Thất bại: Ghi log lỗi và đề xuất sửa
3. Trả về kết quả cuối cùng
```

---

## 5. Template có tham số

### 5.1 Định nghĩa tham số

```markdown
## How It Works

1. Tiếp nhận tham số đầu vào:
   - `name`: Tên (bắt buộc)
   - `type`: Loại (tùy chọn, mặc định：`standard`)
2. Xử lý tham số
3. Trả về kết quả
```

### 5.2 Thay thế biến

```markdown
## Examples

### Cách dùng cơ bản

Sử dụng tham số mặc định:
```
Thực hiện Skill, tên là {{name}}
```
```

### Cách dùng tùy chỉnh

Chỉ định loại tùy chỉnh:
```
Thực hiện Skill, tên là {{name}}, loại là {{type}}
```
```

---

## 6. Phát hành và chia sẻ

### 6.1 Lưu trữ cục bộ

| Vị trí | Mô tả |
|------|------|
| Project `skills/` | Skills dành riêng cho dự án |
| `~/.claude/skills/` | Skills khả dụng toàn cục |

### 6.2 Import/Export

```bash
# Export Skill ra file
/skill-export my-skill

# Import Skill từ file
/skill-import path/to/skill.md
```

### 6.3 Chợ Skill

ECC hỗ trợ cài đặt Skills chia sẻ từ chợ.

---

## 7. Workflow tạo Skill

Sử dụng lệnh `/skill-create` phân tích lịch sử git và tạo file Skill:

```bash
# Phân tích lịch sử git và tạo Skill
/skill-create

# Chỉ định phạm vi phân tích
/skill-create --days 30

# Chỉ định đường dẫn output
/skill-create --output ./my-skills/
```

---

## 8. Best practices

### 8.1 Nguyên tắc thiết kế

| Nguyên tắc | Mô tả |
|------|------|
| **Đơn trách nhiệm** | Mỗi Skill chỉ làm một việc |
| **Có thể tổ hợp** | Skills có thể tổ hợp với nhau |
| **Có thể tái sử dụng** | Thiết kế chung chứ không cụ thể |
| **Có thể test** | Cung cấp bước xác minh |

### 8.2 Chất lượng tài liệu

| Yêu cầu | Mô tả |
|------|------|
| When to Use rõ ràng | Để người dùng biết khi nào sử dụng |
| How It Works chi tiết | Các bước rõ ràng có thể thực hiện |
| Examples thiết thực | Cung cấp ví dụ thực tế |
| Mô tả xử lý lỗi | Hướng dẫn người dùng xử lý lỗi |

### 8.3 Đề xuất bảo trì

- Cập nhật định kỳ để phản ánh best practices mới nhất
- Cải thiện dựa trên phản hồi của người dùng
- Giữ đơn giản, tránh phức tạp quá mức
- Thêm số version để theo dõi

---

## Bài tập

Hoàn thành [bài tập](./exercises/练习.md) trong phần này.

---

[Quay lại thư mục khả năng cốt lõi](../README.md)