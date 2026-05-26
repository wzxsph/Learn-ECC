# Bài tập: Agent协作

## Bài tập 1: Sử dụng Agent审查类

### Mục tiêu
Học cách gọi Agent审查 code và hiểu cơ chế phản hồi của nó.

### Kịch bản
Giả sử bạn viết đoạn code Python sau (không cần thực sự chạy, chỉ trong suy nghĩ):

```python
def get_user_data(user_id, include_password=False):
    user = db.query("SELECT * FROM users WHERE id = " + user_id)
    if include_password:
        return user
    return {"name": user.name, "email": user.email}
```

### Các bước

1. **Phân tích đoạn code này**
   Suy nghĩ về các vấn đề có thể có:
   - Lỗ hổng SQL injection
   - Rủi ro lộ password
   - Thiếu xử lý lỗi

2. **Chọn Agent phù hợp**
   Với đoạn code này, nên dùng:
   - `security-reviewer` - Kiểm tra lỗ hổng bảo mật
   - `python-reviewer` - Kiểm tra best practices Python

3. **Mô phỏng phản hồi Agent**
   Ghi lại phản hồi mà bạn cho rằng Agent sẽ đưa ra:

   ```markdown
   ## Phản hồi code review

   ### Vấn đề bảo mật (CRITICAL)
   - SQL injection: Ghép trực tiếp user_id vào câu SQL
   - Lộ password: Tùy chọn include_password có thể lộ password

   ### Vấn đề chất lượng (HIGH)
   - Thiếu xử lý lỗi
   - Thiếu xác thực input

   ### Đề xuất cải thiện
   - Sử dụng parameterized query
   - Không bao giờ trả về trường password
   - Thêm xử lý exception
   ```

4. **Soạn kế hoạch sửa**
   Sử dụng tư duy của `/plan`, soạn kế hoạch sửa:

   ```markdown
   # Kế hoạch sửa

   1. Sửa SQL injection: Sử dụng parameterized query
   2. Loại bỏ trả về password: Không bao giờ trả về password
   3. Thêm xác thực input: Xác thực format user_id
   4. Thêm xử lý lỗi: Xử lý trường hợp user không tồn tại
   ```

### Tiêu chuẩn nghiệm thu
- [ ] Có thể nhận diện vấn đề bảo mật trong code
- [ ] Hiểu cấu trúc phản hồi của Agent审查
- [ ] Có thể soạn kế hoạch sửa

---

## Bài tập 2: Sử dụng Agent构建修复类

### Mục tiêu
Học cách sử dụng Agent build để chẩn đoán và sửa lỗi build.

### Kịch bản
Giả sử dự án Go có các lỗi build sau:

```
# github.com/myproject/pkg
./main.go:15: undefined: " fmt"
./main.go:23: undefined: calculateTotal
./main.go:31: too many arguments to function call
```

### Các bước

1. **Phân tích loại lỗi**
   Phân loại lỗi:

   | Lỗi | Loại | Mức độ nghiêm trọng |
   |------|------|----------|
   | undefined: " fmt" | Lỗi import | HIGH |
   | undefined: calculateTotal | Function không định nghĩa | HIGH |
   | too many arguments | Lỗi tham số | MEDIUM |

2. **Thiết kế quy trình sửa**
   Sử dụng Agent协作:

   ```markdown
   # Quy trình sửa lỗi build

   1. /go-build - Chạy build để lấy đầy đủ lỗi
   2. /build-fix - Gọi Agent sửa lỗi build
   3. Phân tích ưu tiên lỗi: Lỗi high priority sửa trước
   4. Sửa từng cái và xác minh
   5. /verify - Xác nhận build thành công
   ```

3. **Thiết kế phương án sửa cho mỗi lỗi**

   **Lỗi 1: undefined: " fmt"**
   ```go
   // Vấn đề: Dấu ngoặc thừa
   import "fmt"  // Cách viết đúng
   
   // Sửa: Bỏ dấu ngoặc
   ```

   **Lỗi 2: undefined: calculateTotal**
   ```go
   // Vấn đề: Function không định nghĩa hoặc sai tên
   // Sửa: Kiểm tra định nghĩa function, có thể cần thêm hoặc đổi tên
   ```

   **Lỗi 3: too many arguments**
   ```go
   // Vấn đề: Số tham số khi gọi function không khớp
   // Sửa: Kiểm tra signature function, sửa tham số gọi
   ```

4. **Ghi lại kinh nghiệm sửa lỗi**
   Tạo tổng kết pattern lỗi:

   ```markdown
   # Pattern lỗi build Go thường gặp

   ## Lỗi thường gặp
   1. undefined: Vấn đề import/function
   2. too many/few arguments: Tham số không khớp
   3. undefined: " xxx": Lỗi cú pháp import

   ## Chiến lược sửa
   - Trước xem số lượng lỗi, rồi xem vị trí lỗi
   - Lỗi high priority có thể连锁 dẫn đến lỗi low priority
   - Sửa xong build lại xác minh
   ```

### Tiêu chuẩn nghiệm thu
- [ ] Có thể phân tích loại lỗi build và ưu tiên
- [ ] Đã thiết kế quy trình sửa hoàn chỉnh
- [ ] Đã ghi lại pattern lỗi thường gặp

---

## Bài tập 3: Workflow tổ hợp Agent

### Mục tiêu
Thiết kế một nhiệm vụ phức tạp, sử dụng nhiều Agent协作完成.

### Kịch bản
Phát triển module xác thực người dùng mới, bao gồm các chức năng:
- Đăng ký người dùng (có xác thực độ mạnh password)
- Đăng nhập người dùng (có protection chống brute force)
- Tạo và xác minh JWT token

### Các bước

1. **Phân rã nhiệm vụ**
   Phân rã nhiệm vụ thành sub-task:

   ```markdown
   # Nhiệm vụ phát triển module xác thực

   ## Sub-task
   1. Thiết kế database (Schema)
   2. API đăng ký người dùng
   3. API đăng nhập người dùng
   4. Xử lý JWT Token
   5. Cơ chế bảo mật (độ mạnh password, protection brute force)
   ```

2. **Phân công Agent**
   Phân công Agent cho mỗi sub-task:

   | Sub-task | Agent | Lý do |
   |--------|-------|------|
   | Thiết kế database | architect | Cần góc nhìn kiến trúc |
   | API đăng ký | tdd-guide + security-reviewer | TDD + Bảo mật |
   | API đăng nhập | tdd-guide + security-reviewer | TDD + Bảo mật |
   | Xử lý JWT | security-reviewer | Nhạy cảm bảo mật |
   | Cơ chế bảo mật | security-reviewer (ưu tiên) | Bảo mật ưu tiên |

3. **Thiết kế workflow**

   ```
   [Bắt đầu]
      ↓
   /plan (Lập kế hoạch tổng thể)
      ↓
   ┌─────────────────────────────────────┐
   │  Thực thi song song:                  │
   │  - architect: Thiết kế database     │
   │  - tdd-guide: Thiết kế chiến lược test│
   └─────────────────────────────────────┘
      ↓
   [Giai đoạn实施]
      ↓
   ┌─────────────────────────────────────┐
   │  Kết hợp串行 + song song:             │
   │  1. tdd-guide: Viết test đăng ký    │
   │  2. Implement chức năng đăng ký      │
   │  3. security-reviewer: Review bảo mật │
   │  ────────────────────────────────────│
   │  1. tdd-guide: Viết test đăng nhập   │
   │  2. Implement chức năng đăng nhập    │
   │  3. security-reviewer: Review bảo mật │
   └─────────────────────────────────────┘
      ↓
   [Giai đoạn整合]
      ↓
   ┌─────────────────────────────────────┐
   │  - architect: Thiết kế kiến trúc JWT │
   │  - security-reviewer: Review bảo mật tổng thể│
   │  - code-reviewer: Review chất lượng code│
   │  - /verify: Xác minh tổng thể         │
   └─────────────────────────────────────┘
      ↓
   [Hoàn thành]
   ```

4. **Viết tài liệu workflow**

   ```markdown
   # Workflow phát triển module xác thực người dùng

   ## Giai đoạn 1: Lập kế hoạch
   - Sử dụng /plan soạn kế hoạch tổng thể
   - Sử dụng architect thiết kế database và kiến trúc JWT

   ## Giai đoạn 2: Phát triển TDD
   - Sử dụng tdd-guide hướng dẫn viết test
   - Viết test trước, rồi implement chức năng
   - Xác minh từng sub-task riêng lẻ

   ## Giai đoạn 3: Review bảo mật
   - Sử dụng security-reviewer review từng API
   - Đặc biệt chú ý: Xử lý password, Bảo mật Token, Protection injection

   ## Giai đoạn 4: Tích hợp xác minh
   - Sử dụng code-reviewer review tổng thể
   - Sử dụng /verify xác minh tính đầy đủ của chức năng

   ## Tổng kết Agent协作
   - architect: Thiết kế kiến trúc (1 lần)
   - tdd-guide: Hướng dẫn test (2 lần)
   - security-reviewer: Review bảo mật (3 lần)
   - code-reviewer: Review chất lượng (1 lần)
   ```

### Tiêu chuẩn nghiệm thu
- [ ] Phân rã nhiệm vụ hợp lý, bao gồm 5+ sub-task
- [ ] Chiến lược phân công Agent rõ ràng
- [ ] Thiết kế workflow rõ ràng
- [ ] Tài liệu hóa workflow để tái sử dụng

---

## Học tập sâu

- Đọc tài liệu Agent đầy đủ: [../../../TaiLieuThamKhao/agents/01-Kiem-Duyet-Ma.md](../../../TaiLieuThamKhao/agents/01-Kiem-Duyet-Ma.md)
- Xem danh sách tất cả Agent khả dụng
- Học cách tùy chỉnh hành vi Agent