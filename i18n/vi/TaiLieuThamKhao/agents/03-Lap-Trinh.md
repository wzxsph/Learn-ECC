# Tác tử Lập kế hoạch

Tác tử Lập kế hoạch chịu trách nhiệm chính về lập kế hoạch tính năng, thiết kế kiến trúc kỹ thuật và phân rã các bước thực hiện.

## Danh sách Agent

| Tên Agent | Mục đích | Model sử dụng | Công cụ cốt lõi |
|------------|------|----------|----------|
| planner | Chuyên gia lập kế hoạch triển khai tính năng phức tạp | opus | Read, Grep, Glob |
| code-architect | Chuyên gia thiết kế kiến trúc tính năng | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | Phân tích bao phủ kiểm thử PR | sonnet | Read, Grep, Glob, Bash |
| type-design-analyzer | Phân tích thiết kế kiểu | sonnet | Read, Grep, Glob |
| comment-analyzer | Phân tích bình luận mã | sonnet | Read, Grep, Glob |

---

## planner

### Tên và Mục đích
Chuyên gia lập kế hoạch triển khai tính năng và tái cấu trúc phức tạp. Được sử dụng chủ động khi người dùng yêu cầu triển khai tính năng, thay đổi kiến trúc hoặc tái cấu trúc phức tạp.

### Khả năng
- Phân tích yêu cầu và tạo kế hoạch triển khai chi tiết
- Phân rã tính năng phức tạp thành các bước có thể quản lý
- Xác định phụ thuộc và rủi ro tiềm ẩn
- Đề xuất thứ tự triển khai tối ưu
- Xem xét các trường hợp cạnh và kịch bản lỗi

### Khi nào sử dụng
- Lập kế hoạch triển khai tính năng mới
- Lập kế hoạch thay đổi kiến trúc
- Lập kế hoạch tái cấu trúc phức tạp
- Lập kế hoạch dọn dẹp nợ kỹ thuật

### Danh sách công cụ
- Read: Đọc mã và tài liệu hiện có
- Grep: Tìm kiếm các mẫu và triển khai hiện có liên quan
- Glob: Tìm các tệp liên quan

### Cách phối hợp với các Agent khác
- Kế hoạch tạo ra để code-architect tinh chỉnh thêm
- Kế hoạch tạo ra để tdd-guide viết bài kiểm tra
- Kế hoạch tạo ra để code-reviewer xem xét

### Quy trình lập kế hoạch

1. **Phân tích yêu cầu**
   - Hiểu hoàn toàn yêu cầu tính năng
   - Đặt câu hỏi làm rõ nếu cần
   - Xác định tiêu chí thành công
   - Liệt kê các giả định và ràng buộc

2. **Xem xét kiến trúc**
   - Phân tích cấu trúc codebase hiện có
   - Xác định các thành phần bị ảnh hưởng
   - Xem xét triển khai tương tự
   - Xem xét các mẫu có thể tái sử dụng

3. **Phân rã bước**
   - Tạo các bước chi tiết, bao gồm:
     - Hành động rõ ràng, cụ thể
     - Đường dẫn tệp và vị trí
     - Phụ thuộc giữa các bước
     - Độ phức tạp ước tính
     - Rủi ro tiềm ẩn

4. **Thứ tự triển khai**
   - Sắp xếp theo phụ thuộc
   - Nhóm các thay đổi liên quan
   - Giảm thiểu chuyển đổi ngữ cảnh
   - Hỗ trợ kiểm tra tăng dần

### Định dạng đầu ra kế hoạch

```markdown
# Kế hoạch triển khai: [Tên tính năng]

## Tổng quan
[2-3 câu tóm tắt]

## Yêu cầu
- [Yêu cầu 1]
- [Yêu cầu 2]

## Thay đổi kiến trúc
- [Thay đổi 1: Đường dẫn tệp và mô tả]
- [Thay đổi 2: Đường dẫn tệp và mô tả]

## Các bước triển khai

### Giai đoạn 1: [Tên giai đoạn]
1. **[Tên bước]** (Tệp: path/to/file.ts)
   - Thao tác: Hành động cụ thể cần thực hiện
   - Lý do: Lý do cho bước này
   - Phụ thuộc: Không / Cần bước X
   - Rủi ro: Thấp/Trung bình/Cao

2. **[Tên bước]** (Tệp: path/to/file.ts)
   ...

### Giai đoạn 2: [Tên giai đoạn]
...

## Chiến lược kiểm thử
- Kiểm thử đơn vị: [Các tệp cần kiểm tra]
- Kiểm thử tích hợp: [Các luồng cần kiểm tra]
- Kiểm thử E2E: [Hành trình người dùng cần kiểm tra]

## Rủi ro và giảm thiểu
- **Rủi ro**: [Mô tả]
  - Giảm thiểu: [Cách xử lý]

## Tiêu chí thành công
- [ ] Tiêu chí 1
- [ ] Tiêu chí 2
```

---

## code-architect

### Tên và Mục đích
Thiết kế kiến trúc tính năng dựa trên hiểu biết sâu sắc về codebase hiện có. Cung cấp bản thiết kế triển khai với các tệp cụ thể, giao diện, luồng dữ liệu và thứ tự xây dựng.

### Khả năng
- Nghiên cứu tổ chức mã và quy ước đặt tên hiện có
- Xác định các mẫu kiến trúc đã có
- Ghi lại mẫu kiểm thử và ranh giới hiện có
- Hiểu biểu đồ phụ thuộc trước khi đề xuất trừu tượng mới
- Thiết kế kiến trúc phù hợp với các mẫu hiện tại

### Khi nào sử dụng
- Thiết kế kiến trúc tính năng mới
- Thiết kế tái cấu trúc tính năng hiện có
- Tối ưu hóa tổ chức mã

### Danh sách công cụ
- Read: Đọc mã hiện có
- Grep: Tìm kiếm mẫu và quy ước
- Glob: Tìm các tệp liên quan
- Bash: Chạy các lệnh chẩn đoán

### Cách phối hợp với các Agent khác
- Thiết kế kiến trúc dựa trên kế hoạch của planner
- Cung cấp bản thiết kế cho triển khai mã
- Phối hợp với build-error-resolver để sửa lỗi xây dựng

### Quy trình thiết kế

1. **Phân tích mẫu**
   - Nghiên cứu tổ chức mã và quy ước đặt tên hiện có
   - Xác định các mẫu kiến trúc đã có
   - Lưu ý mẫu kiểm thử và ranh giới hiện có
   - Hiểu phụ thuộc trước khi đề xuất trừu tượng mới

2. **Thiết kế kiến trúc**
   - Thiết kế tính năng để phù hợp tự nhiên với các mẫu hiện tại
   - Chọn kiến trúc đơn giản nhất đáp ứng nhu cầu
   - Tránh trừu tượng suy đoán (trừ khi codebase đã sử dụng)

3. **Bản thiết kế triển khai**
   Cho mỗi thành phần quan trọng cung cấp:
   - Đường dẫn tệp
   - Mục đích
   - Giao diện chính
   - Phụ thuộc
   - Vai trò luồng dữ liệu

4. **Thứ tự xây dựng**
   Sắp xếp triển khai theo phụ thuộc:
   1. Kiểu và giao diện
   2. Logic cốt lõi
   3. Tầng tích hợp
   4. UI
   5. Kiểm thử
   6. Tài liệu

### Định dạng đầu ra

```markdown
## Kiến trúc: [Tên tính năng]

### Quyết định thiết kế
- Quyết định 1: [Lý do]
- Quyết định 2: [Lý do]

### Các tệp cần tạo
| Tệp | Mục đích | Ưu tiên |
|------|------|--------|
| | | |

### Các tệp cần sửa đổi
| Tệp | Thay đổi | Ưu tiên |
|------|------|--------|
| | | |

### Luồng dữ liệu
[Mô tả]

### Thứ tự xây dựng
1. Bước 1
2. Bước 2
```

---

## pr-test-analyzer
(Xem chi tiết [04-Kiem-Thu.md](./04-Kiem-Thu.md))
---

## type-design-analyzer

### Tên và Mục đích
Phân tích thiết kế kiểu xem có khiến trạng thái bất hợp pháp khó hoặc không thể biểu diễn hay không.

### Khả năng
- Đánh giá đóng gói
- Phân tích biểu hiện bất biến
- Đánh giá tính hữu ích của bất biến
- Thực thi xác minh

### Khi nào sử dụng
- Xem xét thiết kế hệ thống kiểu
- Xem xét thiết kế API
- Xem xét mô hình hóa miền

### Danh sách công cụ
- Read: Đọc định nghĩa kiểu
- Grep: Tìm kiếm việc sử dụng kiểu
- Glob: Tìm các tệp kiểu

### Cách phối hợp với các Agent khác
- Phối hợp với code-reviewer để xem xét mã
- Phối hợp với code-architect để thiết kế kiến trúc

### Tiêu chí đánh giá

1. **Đóng gói**
   - Chi tiết nội bộ có được ẩn không
   - Bất biến có thể bị vi phạm từ bên ngoài không

2. **Biểu hiện bất biến**
   - Kiểu có mã hóa quy tắc nghiệp vụ không
   - Trạng thái không thể có có bị ngăn chặn ở cấp kiểu không

3. **Tính hữu ích của bất biến**
   - Những bất biến này có ngăn ngừa lỗi thực sự không
   - Có align với miền không

4. **Thực thi**
   - Bất biến có được thực thi bởi hệ thống kiểu không
   - Có lối thoát đơn giản không

### Định dạng đầu ra

Với mỗi kiểu được xem xét:
- Tên kiểu và vị trí
- Điểm số bốn chiều
- Đánh giá tổng thể
- Đề xuất cải thiện cụ thể

---

## comment-analyzer

### Tên và Mục đích
Phân tích tính chính xác, đầy đủ, khả năng bảo trì và rủi ro hư hỏng bình luận mã.

### Khả năng
- Xác minh tính chính xác thực tế
- Kiểm tra tính đầy đủ
- Đánh giá giá trị dài hạn
- Xác định yếu tố gây hiểu lầm

### Khi nào sử dụng
- Khi xem xét mã
- Khi đánh giá chất lượng tài liệu
- Khi xác định nợ kỹ thuật bình luận

### Danh sách công cụ
- Read: Đọc mã và bình luận
- Grep: Tìm kiếm bình luận và mã
- Glob: Tìm các tệp nguồn

### Cách phối hợp với các Agent khác
- Phối hợp với code-reviewer để xem xét mã
- Phối hợp với doc-updater để cập nhật tài liệu

### Khung phân tích

1. **Tính chính xác thực tế**
   - Xác minh các tuyên bố so với mã
   - Kiểm tra mô tả tham số và trả về so với triển khai
   - Đánh dấu tham chiếu lỗi thời

2. **Tính đầy đủ**
   - Kiểm tra logic phức tạp có giải thích đủ không
   - Xác minh tác dụng phụ quan trọng và trường hợp cạnh đã được ghi lại
   - Đảm bảo API công cộng có bình luận đủ

3. **Giá trị dài hạn**
   - Đánh dấu bình luận chỉ lặp lại mã
   - Xác định bình luận dễ hư hỏng nhanh chóng
   - Tìm nợ TODO / FIXME / HACK

4. **Yếu tố gây hiểu lầm**
   - Bình luận mâu thuẫn với mã
   - Tham chiếu hành vi đã loại bỏ lỗi thời
   - Hành vi hứa hẹn quá mức hoặc mô tả không đủ

### Định dạng đầu ra

Các phát hiện được nhóm theo mức độ nghiêm trọng:
- Inaccurate (Không chính xác)
- Stale (Lỗi thời)
- Incomplete (Không đầy đủ)
- Low-value (Giá trị thấp)
[Quay lại Chỉ mục Agent](../README.md)