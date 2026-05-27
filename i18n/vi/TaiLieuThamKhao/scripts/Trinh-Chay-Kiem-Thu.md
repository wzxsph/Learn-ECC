# Trình chạy kiểm thử

Tài liệu này giới thiệu cấu trúc và cách chạy kiểm thử của dự án ECC.

## Điểm vào kiểm thử

**Đường dẫn**: `tests/run-all.js`

Trình chạy kiểm thử tập trung, tự động khám phá và chạy tất cả file kiểm thử.

### Cách sử dụng

```bash
# Chạy tất cả kiểm thử
node tests/run-all.js

# Chạy một file kiểm thử
node tests/lib/utils.test.js
node tests/hooks/hooks.test.js
```

### Khám phá file kiểm thử

Trình chạy kiểm thử khám phá file kiểm thử qua glob pattern `tests/**/*.test.js`.

Quy tắc:
- File phải nằm trong thư mục `tests/`
- Tên file phải kết thúc bằng `.test.js`
- Cấu trúc thư mục được giữ để hiển thị

### Định dạng output

```
╔══════════════════════════════════════════════════════════╗
║           Everything Claude Code - Test Suite             ║
╚══════════════════════════════════════════════════════════╝

━━━ Running lib/utils.test.js ━━━
(output kiểm thử...)
━━━ Running hooks/hooks.test.js ━━━
(output kiểm thử...)

╔══════════════════════════════════════════════════════════╗
║                     Final Results                        ║
╠══════════════════════════════════════════════════════════╣
║  Total Tests:    42                                      ║
║  Passed:         42  ✓                                   ║
║  Failed:         0                                        ║
╚══════════════════════════════════════════════════════════╝
```

### Thống kê kiểm thử

Trình chạy kiểm thử parse output của mỗi file để tổng hợp:
- `Passed: <số lượng>`
- `Failed: <số lượng>`

---

## Cấu trúc kiểm thử

### Bố cục thư mục

```
tests/
├── run-all.js              # Trình chạy kiểm thử chính
├── lib/                    # Kiểm thử utility
│   ├── utils.test.js
│   ├── package-manager.test.js
│   └── ...
├── hooks/                  # Kiểm thử tích hợp Hook
│   └── hooks.test.js
├── integration/            # Kiểm thử tích hợp
├── scripts/                # Kiểm thử script
├── commands/               # Kiểm thử command
├── docs/                   # Kiểm thử tài liệu
├── opencode-config.test.js
├── plugin-manifest.test.js
├── test_*.py               # Kiểm thử Python
└── conftest.py             # Cấu hình pytest
```

### Loại kiểm thử

1. **Kiểm thử đơn vị** (`*.test.js`)
   - Kiểm thử hàm và utility độc lập
   - Sử dụng Node.js assert native hoặc Jest style

2. **Kiểm thử tích hợp** (`tests/integration/`)
   - Kiểm thử nhiều component cùng làm việc
   - Kiểm thử luồng Hook hoàn chỉnh

3. **Kiểm thử Python** (`test_*.py`)
   - Kiểm thử format pytest
   - Sử dụng `conftest.py` để cấu hình

---

## Cấu hình kiểm thử

### Kiểm thử Node.js

File kiểm thử sử dụng Node.js assert module chuẩn:

```javascript
const assert = require('assert');

test('example test', () => {
  assert.strictEqual(actual, expected, 'Optional message');
});
```

### Kiểm thử Python (pytest)

Vị trí: `tests/conftest.py`

File cấu hình pytest, cung cấp shared fixtures và cấu hình.

```bash
# Chạy kiểm thử Python
python -m pytest tests/

# Chạy kiểm thử cụ thể
python -m pytest tests/test_builder.py
```

---

## Thực hành tốt nhất kiểm thử

### Mẫu AAA

```
test('mô tả kiểm thử', () => {
  // Arrange - Chuẩn bị dữ liệu kiểm thử
  const input = 'test data';

  // Act - Thực hiện thao tác được kiểm thử
  const result = myFunction(input);

  // Assert - Xác minh kết quả
  assert.strictEqual(result, expected);
});
```

### Đặt tên kiểm thử

Sử dụng tên mô tả, giải thích rõ hành vi được kiểm thử:

```javascript
test('returns empty array when no markets match query', () => {});
test('throws error when API key is missing', () => {});
test('falls back to substring search when Redis is unavailable', () => {});
```

### Cô lập kiểm thử

Mỗi kiểm thử nên:
- Chạy độc lập, không phụ thuộc các kiểm thử khác
- Dọn dẹp dữ liệu kiểm thử riêng
- Sử dụng mock để cô lập external dependencies

---

## Tích hợp CI

Trong `package.json`, cấu hình script kiểm thử:

```json
{
  "scripts": {
    "test": "validate-commands.js && tests/run-all.js"
  }
}
```

Trình chạy kiểm thử được thiết kế để hoạt động trong môi trường CI:
- Exit code khác 0 cho biết fail
- Output JSON cho parse tự động
- Thông báo lỗi rõ ràng cho debug