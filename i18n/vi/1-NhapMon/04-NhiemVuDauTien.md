# 04 - Nhiệm vụ đầu tiên

## Mục tiêu học tập

- Hiểu quy trình phát triển đầy đủ
- Nắm vững quy trình hoàn chỉnh /ecc:plan → /ecc:code-review → /ecc:build-fix → commit
- Có thể độc lập hoàn thành phát triển một tính năng đơn giản

---

## Quy trình phát triển đầy đủ

ECC khuyến nghị sử dụng quy trình phát triển sau:

```
Step 1: /ecc:plan        → Lập kế hoạch nhiệm vụ
Step 2: /ecc:code-review → Review code
Step 3: /ecc:build-fix   → Sửa build
Step 4: Commit code     → Git commit
```

---

## Step 1: Sử dụng /ecc:plan lập kế hoạch

### Input

Giả sử chúng ta cần implement tính năng máy tính đơn giản:

```
/ecc:plan Implement một máy tính hỗ trợ bốn phép toán cộng trừ nhân chia
```

### Output kỳ vọng

Lệnh /ecc:plan sẽ:

1. **Nhắc lại yêu cầu** - "Implement một máy tính hỗ trợ cộng, trừ, nhân, chia bốn phép toán cơ bản..."
2. **Đánh giá rủi ro** - "Cần lưu ý: trường hợp chia cho không..."
3. **Kế hoạch từng bước** -
   - Phase 1: Tạo core function máy tính
   - Phase 2: Implement bốn phép toán
   - Phase 3: Thêm xử lý lỗi
   - Phase 4: Viết unit test
4. **Chờ xác nhận** - "Vui lòng xác nhận kế hoạch có thể bắt đầu không"

### Sau khi xác nhận

Sau khi user xác nhận, ECC sẽ chờ user input để bắt đầu từng phase.

### Mẹo nhỏ

- Sử dụng chế độ file PRD để có kế hoạch chi tiết hơn
- Tính năng phức tạp nên viết PRD trước rồi mới lập kế hoạch

---

## Step 2: Sử dụng /ecc:code-review review

### Input

Sau khi viết code xong, chạy:

```
/ecc:code-review
```

### Output kỳ vọng

/ecc:code-review sẽ:

1. Thu thập các file đã thay đổi
2. Thực hiện review 7 danh mục:
   - Độ chính xác
   - Type safety
   - Tuân thủ pattern
   - Bảo mật
   - Performance
   - Tính đầy đủ
   - Khả năng bảo trì
3. Tạo báo cáo review, ghi chú các cấp độ CRITICAL/HIGH/MEDIUM/LOW

### Ví dụ báo cáo review

```
## Báo cáo Code Review

### CRITICAL
- [ ] Hardcoded credentials: dòng 23 trong src/auth.js

### HIGH
- [ ] Function quá dài: dòng 45-78 trong src/calculator.js (34 dòng)
- [ ] Thiếu xử lý lỗi: function chia trong src/calculator.js

### MEDIUM
- [ ] Naming không chuẩn: biến `tmp` nên đổi thành `temporary`

### Đề xuất thao tác
1. Remove hardcoded API Key
2. Refactor function chia trong calculator.js, thêm kiểm tra chia cho không
3. Chia nhỏ function quá dài
```

### Mẹo nhỏ

- Bắt buộc chạy /ecc:code-review trước khi commit
- Vấn đề CRITICAL và HIGH phải sửa

---

## Step 3: Sử dụng /ecc:build-fix sửa

### Input

Nếu build thất bại, chạy:

```
/ecc:build-fix
```

### Output kỳ vọng

/ecc:build-fix sẽ:

1. **Detect build system** - "Phát hiện TypeScript project"
2. **Chạy build** - "Chạy tsc --noEmit..."
3. **Parse lỗi** - Phân nhóm theo file
4. **Sửa từng bước** - Sửa một lỗi một lần
5. **Xác minh** - Build lại sau mỗi lần sửa

### Ví dụ output

```
[build-fix] Phát hiện TypeScript project
[build-fix] Chạy tsc --noEmit...
[build-fix] Phát hiện 3 lỗi:

Lỗi 1: src/calculator.ts:15 - Kiểu 'string' không thể gán cho kiểu 'number'
Lỗi 2: src/calculator.ts:28 - Thuộc tính 'result' không tồn tại
Lỗi 3: src/calculator.ts:42 - Tham số 'b' không được sử dụng

[build-fix] Bắt đầu sửa lỗi 1...
[build-fix] Sửa xong, đang xác minh...
[build-fix] Build thành công!
```

### Mẹo nhỏ

- Sửa build là incremental, sửa một lỗi một lần
- Có thể Ctrl+C bất kỳ lúc nào để dừng

---

## Step 4: Commit code

### Quy trình commit

1. Đảm bảo tất cả test pass
2. Đảm bảo build thành công
3. Đảm bảo các vấn đề review đã sửa

### Quy tắc commit Git

ECC dùng định dạng conventional commits:

```
<type>: <description>

[optional body]
```

**Các loại Type**:
- `feat` - Tính năng mới
- `fix` - Sửa bug
- `refactor` - Refactor
- `docs` - Documentation
- `test` - Test
- `chore` - Bảo trì thường ngày
- `perf` - Tối ưu performance
- `ci` - Cấu hình CI

### Ví dụ commit

```
feat: Implement bốn phép toán cơ bản cho máy tính

- Thêm function cộng
- Thêm function trừ
- Thêm function nhân
- Thêm function chia (có kiểm tra chia cho không)

Fixes: Issue #123
```

### Mẹo nhỏ

- Dùng `/ecc:verify quick` để xác minh nhanh trước khi commit
- Tính năng lớn nên commit nhiều lần, tính năng nhỏ commit một lần

---

## Ví dụ đầy đủ

### Kịch bản

Implement một utility function xử lý chuỗi đơn giản: `truncate(str, maxLength)`, cắt chuỗi đến độ dài chỉ định.

### Step 1: Lập kế hoạch

```
/ecc:plan Implement function truncate, cắt chuỗi đến độ dài chỉ định
```

Output:
```
## Nhắc lại yêu cầu
Implement function truncate(str, maxLength), phần vượt quá maxLength thay bằng ...

## Đánh giá rủi ro
- Xử lý khi maxLength nhỏ hơn 3
- Xử lý khi str rỗng hoặc null
- Xử lý ký tự Unicode

## Kế hoạch từng bước
1. Tạo src/string-utils.ts
2. Implement function truncate
3. Thêm test các trường hợp biên
4. Thêm JSDoc documentation

Vui lòng xác nhận có thể bắt đầu.
```

### Step 2: Implement

( Sau khi user xác nhận, ECC hỗ trợ implement code)

### Step 3: Review

```
/ecc:code-review
```

Output:
```
## Báo cáo Code Review
Trạng thái: APPROVED

### Kết quả review
- Độ chính xác: Pass
- Type safety: Pass
- Tuân thủ pattern: Pass
- Bảo mật: Pass
- Performance: Pass
- Tính đầy đủ: Đề xuất thêm nhiều test biên hơn
- Khả năng bảo trì: Pass

### Đề xuất
- Thêm test chuỗi rỗng
- Thêm test giá trị null
```

### Step 4: Build verification

```
/ecc:build-fix
```

```
[build-fix] Phát hiện TypeScript project
[build-fix] Chạy tsc --noEmit...
[build-fix] Build thành công!
```

### Step 5: Commit

```
feat: Implement function truncate cắt chuỗi

- Hỗ trợ độ dài chỉ định
- Phần vượt quá thay bằng ...
- Xử lý chuỗi rỗng và giá trị null
```

---


## Tóm tắt

1. **Quy trình đầy đủ**: /ecc:plan → Implement → /ecc:code-review → /ecc:build-fix → Commit
2. **/ecc:plan** Dùng để lập kế hoạch, đảm bảo hướng đi đúng
3. **/ecc:code-review** Dùng để review, phát hiện vấn đề
4. **/ecc:build-fix** Dùng để sửa lỗi build
5. **Commit** Dùng định dạng conventional commits

---

## Bước tiếp theo

- Sau khi hoàn thành khóa học, làm [exercises/BaiTapNhapMon.md](./exercises/BaiTapNhapMon.md)
- Vào Stage 2: Nâng cao khả năng cốt lõi