# 05 - Viết Rules

Học cách viết các quy tắc phát triển tùy chỉnh.

## Tổng quan

ECC [Rules](../../TaiLieuThamKhao/rules/代码风格.md) là bộ quy tắc phát triển bắt buộc, bao gồm code style, test, bảo mật, Git workflow, điều phối agent và tối ưu performance. Các quy tắc này đảm bảo chất lượng và tính nhất quán của code, thực thi thông qua kiểm tra và review tự động.

---

## 1. Format Rules

### 1.1 Format cơ bản

Rules sử dụng định dạng Markdown, bao gồm YAML frontmatter:

```yaml
---
name: rule-name
description: Mô tả rule
category: Phân loại
---
```

### 1.2 Lớp rule

| Lớp | Mô tả | Ưu tiên |
|------|------|--------|
| **Global rules** | `~/.claude/rules/` | Cao nhất |
| **Project rules** | Project `.claude/rules/` | Trung bình |
| **Local rules** | Session hiện tại | Thấp nhất |

### 1.3 Ưu tiên

Global rules > Project rules > Local rules

---

## 2. Chi tiết các loại Rules

### 2.1 Code style rules

Định nghĩa các nguyên tắc và best practices tổ chức code cốt lõi.

#### Yêu cầu cốt lõi

**Tính bất biến (Nguyên tắc quan trọng)**

```typescript
// Ví dụ sai - Thay đổi object gốc
function modify(user, name) {
  user.name = name;  // Thay đổi object gốc
  return user;
}

// Ví dụ đúng - Trả về object mới
function update(user, name) {
  return { ...user, name };  // Tạo bản sao mới
}
```

**Nguyên tắc thiết kế cốt lõi**

| Nguyên tắc | Mô tả | Thực hành |
|------|------|------|
| **KISS** | Keep It Simple | Ưu tiên giải pháp đơn giản nhất có thể |
| **DRY** | Don't Repeat Yourself | Tránh copy-paste dẫn đến drift implementation |
| **YAGNI** | You Aren't Gonna Need It | Refactor khi thực sự cần |

**Quy ước đặt tên**

| Loại | Quy ước | Ví dụ |
|------|------|------|
| Biến và function | camelCase | `calculateTotal` |
| Boolean | Tiền tố is/has/should/can | `isActive` |
| Interface, type, component | PascalCase | `UserProfile` |
| Constant | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |

### 2.2 Test rules

Đảm bảo tiêu chuẩn test bắt buộc về chất lượng code.

#### Coverage test tối thiểu: 80%

| Loại test | Mô tả |
|----------|------|
| Unit test | Function độc lập, utility class, component |
| Integration test | API endpoint, thao tác database |
| E2E test | User flow quan trọng |

#### TDD Workflow

```
1. Viết test (ĐỎ)   - Viết test thất bại trước
2. Chạy test          - Xác minh test thất bại
3. Viết implementation tối thiểu (XANH) - Viết code tối thiểu để test pass
4. Chạy test          - Xác minh test pass
5. Refactor (CẢI TIẾN)    - Cải thiện cấu trúc code
6. Xác minh coverage        - Đảm bảo đạt 80%+
```

### 2.3 Security rules

Checklist bảo mật bắt buộc.

#### Phải kiểm tra trước khi commit

- [ ] Không hardcode credentials (API keys, password, tokens)
- [ ] Tất cả user input được xác thực
- [ ] Protection SQL injection (parameterized query)
- [ ] Protection XSS (output escaping)
- [ ] CSRF protection enabled
- [ ] Xác thực authentication/authorization
- [ ] Rate limiting
- [ ] Thông báo lỗi không leak dữ liệu nhạy cảm

#### Quản lý keys

- **Tuyệt đối không** hardcode keys trong source code
- **Luôn** sử dụng biến môi trường hoặc key manager
- Xác minh các keys cần thiết khi khởi động
- Rotate bất kỳ keys nào có thể bị lộ

### 2.4 Git workflow rules

Quy tắc commit Git và PR workflow.

#### Format thông tin commit

```
<type>: <description>

<optional body>
```

**Các loại Type**:
- `feat`: Tính năng mới
- `fix`: Sửa lỗi
- `refactor`: Refactor
- `docs`: Documentation
- `test`: Test
- `chore`: Bảo trì
- `perf`: Performance
- `ci`: CI/CD

#### PR Workflow

1. Phân tích toàn bộ lịch sử commit
2. Sử dụng `git diff [base-branch]...HEAD` xem tất cả thay đổi
3. Soạn tóm tắt PR toàn diện
4. Bao gồm test plan và TODOs
5. Nếu là branch mới, push với flag `-u`

### 2.5 Agent orchestration rules

Mô hình multi-Agent协作.

#### Nguyên tắc sử dụng ngay

| Kịch bản | Agent |
|------|-------|
| Yêu cầu tính năng phức tạp | **planner** |
| Code vừa viết/sửa | **code-reviewer** |
| Sửa bug hoặc tính năng mới | **tdd-guide** |
| Quyết định kiến trúc | **architect** |

#### Thực thi nhiệm vụ song song

```markdown
# Tốt: Thực thi song song
Khởi động 3 Agent song song:
1. Agent 1: Phân tích bảo mật module xác thực
2. Agent 2: Review performance hệ thống cache
3. Agent 3: Kiểm tra type utility

# Xấu: Thực thi tuần tự không cần thiết
Agent 1, rồi Agent 2, rồi Agent 3
```

### 2.6 Performance optimization rules

Lựa chọn model và quản lý context.

#### Chiến lược lựa chọn model

| Model | Mục đích | Chi phí |
|------|------|------|
| **Haiku 4.5** | Agent nhẹ, gọi thường xuyên, pair programming | Thấp |
| **Sonnet 4.6** | Công việc phát triển chính, điều phối multi-agent | Trung bình |
| **Opus 4.5** | Quyết định kiến trúc phức tạp, suy luận sâu | Cao |

#### Quản lý cửa sổ context

Tránh trong 20% cuối cùng của cửa sổ context:
- Refactor quy mô lớn
- Implement tính năng qua nhiều file
- Debug tương tác phức tạp

---

## 3. Viết Rules

### 3.1 Điều kiện trigger

```yaml
---
name: no-console-log
description: Chặn commit code chứa console.log
trigger:
  - pre-commit
  - pre-push
---
```

### 3.2 Logic kiểm tra

```markdown
## Logic kiểm tra

1. Scan các file đã sửa
2. Tìm `console.log`, `console.error`, v.v.
3. Nếu tìm thấy, chặn commit và hiển thị cảnh báo
```

### 3.3 Đề xuất sửa

```markdown
## Đề xuất sửa

- Sử dụng thư viện log có cấu trúc (như winston, pino)
- Cấu hình log level (debug, info, warn, error)
- Đảm bảo log level phù hợp trong môi trường production
```

---

## 4. Xử lý vi phạm

### 4.1 Phân loại mức độ nghiêm trọng

| Cấp độ | Ý nghĩa | Thao tác |
|------|------|------|
| CRITICAL | Lỗ hổng bảo mật hoặc rủi ro mất dữ liệu | **Chặn** - Phải sửa trước khi merge |
| HIGH | Bug hoặc vấn đề chất lượng nghiêm trọng | **Cảnh báo** - Nên sửa trước khi merge |
| MEDIUM | Vấn đề maintainability | **Nhắc nhở** - Đề xuất sửa |
| LOW | Phong cách hoặc gợi ý nhỏ | **Ghi chú** - Tùy chọn |

### 4.2 Quy trình sửa

```
1. Phân tích nguyên nhân vi phạm
2. Xác định phương án sửa
3. Thực hiện sửa
4. Xác minh sửa
5. Submit lại review
```

---

## 5. Checklist chất lượng code

Phải kiểm tra trước khi đánh dấu hoàn thành:

- [ ] Code có thể đọc và đặt tên tốt
- [ ] Function ngắn (<50 dòng)
- [ ] File tập trung (<800 dòng)
- [ ] Không có nested sâu (>4 lớp)
- [ ] Xử lý lỗi phù hợp
- [ ] Không có giá trị hardcode (sử dụng constant hoặc config)
- [ ] Sử dụng pattern bất biến (không mutation)
- [ ] Coverage test ≥80%
- [ ] Tất cả kiểm tra bảo mật pass

---

## 6. Tạo rules đặc thù cho dự án

### 6.1 Vị trí project rules

```
Dự án/
└── .claude/
    └── rules/
        ├── README.md
        └── Các file rules tùy chỉnh
```

### 6.2 Kế thừa global rules

Project rules kế thừa global rules, có thể mở rộng trên đó:

```markdown
# Rules đặc thù của dự án

Kế thừa từ global rules, thêm các yêu cầu sau:

## Yêu cầu bổ sung

- [ ] Quy tắc coding đặc thù của dự án
- [ ] Yêu cầu test đặc thù
- [ ] Yêu cầu documentation đặc thù
```

---

## Bài tập

Hoàn thành [bài tập](./exercises/练习.md) trong phần này.

---

[Quay lại thư mục khả năng cốt lõi](../README.md)