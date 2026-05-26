# Bài tập: Tùy chỉnh Hooks

## Bài tập 1: Tạo PreToolUse Hook

### Mục tiêu
Tạo PreToolUse Hook đầu tiên, implement cảnh báo lệnh nguy hiểm.

### Kịch bản
Khi người dùng thử thực thi lệnh nguy hiểm (như `rm -rf`), hiển thị cảnh báo nhắc nhở.

### Các bước

1. **Thiết kế cấu hình Hook**

   ```json
   {
     "name": "dangerous-command-warning",
     "matcher": {
       "tool": "Bash"
     },
     "condition": "command.includes('rm -rf') || command.includes('drop table')",
     "hooks": [
       {
         "type": "notification",
         "when": "before",
         "message": "Cảnh báo: Phát hiện lệnh nguy hiểm! Vui lòng xác nhận an toàn.",
         "continue": true
       }
     ]
   }
   ```

2. **Tạo file Hook**
   Trong dự án tạo `hooks/dangerous-warning.json`

3. **Test Hook**
   Thiết kế kịch bản test:

   | Lệnh | Hành vi kỳ vọng |
   |------|----------|
   | `rm -rf /tmp/test` | Hiển thị cảnh báo |
   | `rm -rf ./project` | Hiển thị cảnh báo |
   | `ls -la` | Không cảnh báo |
   | `git commit` | Không cảnh báo |

4. **Mở rộng chức năng Hook**
   Thử thêm nhiều loại lệnh nguy hiểm hơn:

   ```json
   "condition": "command.includes('rm -rf') || 
                 command.includes('drop table') || 
                 command.includes('truncate') ||
                 command.includes('shutdown')"
   ```

5. **Thêm whitelist**
   Một số trường hợp cần cho phép lệnh nguy hiểm, thêm cơ chế whitelist:

   ```json
   "condition": "dangerousCommand && !whitelisted"
   ```

### Tiêu chuẩn nghiệm thu
- [ ] Đã tạo file cấu hình PreToolUse Hook
- [ ] Đã định nghĩa điều kiện lệnh nguy hiểm
- [ ] Đã thiết kế kịch bản test
- [ ] Đã thêm ít nhất 5 loại lệnh nguy hiểm được phát hiện

---

## Bài tập 2: Tạo PostToolUse Hook

### Mục tiêu
Tạo PostToolUse Hook, implement thông báo tự động sau khi test hoàn thành.

### Kịch bản
Khi test thực thi xong, tự động kiểm tra kết quả test và gửi thông báo.

### Các bước

1. **Thiết kế cấu hình Hook**

   ```json
   {
     "name": "test-result-notifier",
     "matcher": {
       "tool": "Bash"
     },
     "condition": "command.includes('npm test') || command.includes('pytest')",
     "hooks": [
       {
         "type": "notification",
         "when": "after",
         "message": "Test thực thi hoàn thành",
         "continue": true
       }
     ]
   }
   ```

2. **Tăng cường chức năng: Parse kết quả test**
   Tạo script xử lý JavaScript `scripts/parse-test-result.js`:

   ```javascript
   module.exports = function parseTestResult(output) {
     // Parse output của test
     const passed = output.match(/(\d+) passed/);
     const failed = output.match(/(\d+) failed/);
     
     return {
       passed: passed ? parseInt(passed[1]) : 0,
       failed: failed ? parseInt(failed[1]) : 0,
       status: failed ? 'FAILED' : 'PASSED'
     };
   };
   ```

3. **Tạo cấu hình Hook hoàn chỉnh**
   Kết hợp script để tạo cấu hình đầy đủ:

   ```json
   {
     "name": "test-result-checker",
     "matcher": {
       "tool": "Bash"
     },
     "condition": "command.includes('npm test')",
     "hooks": [
       {
         "type": "command",
         "when": "after",
         "command": "node scripts/parse-test-result.js",
         "continue": true
       }
     ]
   }
   ```

4. **Thiết kế thích ứng nhiều test framework**
   Hỗ trợ các test framework khác nhau:

   | Framework | Từ khóa output |
   |------|----------|
   | Jest | `Tests:` |
   | Pytest | `passed` |
   | Go test | `ok` |
   | RSpec | `examples` |

5. **Thêm kịch bản continuous integration**
   Thiết kế hành vi trong môi trường CI:

   ```json
   {
     "condition": "isCI && testCommand",
     "hooks": [
       {
         "type": "command",
         "when": "after",
         "command": "node scripts/ci-reporter.js",
         "continue": true
       }
     ]
   }
   ```

### Tiêu chuẩn nghiệm thu
- [ ] Đã tạo PostToolUse Hook
- [ ] Đã implement parse kết quả test
- [ ] Đã hỗ trợ nhiều test framework
- [ ] Đã thiết kế xử lý kịch bản CI

---

## Bài tập 3: Cấu hình quality gate Hook

### Mục tiêu
Tạo hệ thống quality gate hoàn chỉnh, tiến hành kiểm tra đa chiều trước khi commit code.

### Kịch bản
Trước `git commit` tự động chạy loạt quality check, đảm bảo chất lượng code.

### Các bước

1. **Định nghĩa các mục quality gate check**

   | Mục check | Nội dung | Hành vi khi thất bại |
   |--------|----------|----------|
   | Coverage test | >= 80% | Chặn commit |
   | Check lint | Không có lỗi | Chặn commit |
   | Quét bảo mật | Không có lỗ hổng | Cảnh báo + chặn |
   | Check format | Tuân thủ quy tắc | Tự động sửa |

2. **Thiết kế quy trình thực thi Hook**

   ```
   git commit
      ↓
   [PreToolUse Hook]
      ↓
   Chạy quality check
      ├── Kiểm tra coverage test
      ├── Kiểm tra Lint  
      ├── Quét bảo mật
      └── Kiểm tra format
      ↓
   [Phán đoán kết quả]
      ↓
   ├── Tất cả pass → Cho phép commit
   ├── Có cảnh báo → Hiển thị cảnh báo, tiếp tục commit
   └── Có lỗi → Chặn commit
   ```

3. **Tạo file cấu hình Hook**

   ```json
   {
     "name": "quality-gate",
     "matcher": {
       "tool": "Bash"
     },
     "condition": "command.includes('git commit')",
     "hooks": [
       {
         "type": "command",
         "when": "before",
         "command": "npm test && npm run lint && npm run security-scan",
         "continue": false
       }
     ]
   }
   ```

4. **Tạo script kiểm tra**

   `scripts/quality-gate.js`:

   ```javascript
   const { execSync } = require('child_process');
   
   function runChecks() {
     const checks = [
       { name: 'Test', command: 'npm test', required: true },
       { name: 'Lint', command: 'npm run lint', required: true },
       { name: 'Coverage', command: 'npm run coverage', required: true },
       { name: 'Bảo mật', command: 'npm run security-scan', required: false }
     ];
   
     let allPassed = true;
     for (const check of checks) {
       try {
         execSync(check.command, { stdio: 'inherit' });
         console.log(`✓ ${check.name} passed`);
       } catch (error) {
         console.error(`✗ ${check.name} failed`);
         if (check.required) allPassed = false;
       }
     }
     return allPassed;
   }
   
   process.exit(runChecks() ? 0 : 1);
   ```

5. **Thêm progressive quality gate**

   ```json
   {
     "name": "progressive-quality-gate",
     "stages": [
       { "name": "Check nhanh", "commands": ["npm run lint"], "blockOnFail": true },
       { "name": "Test", "commands": ["npm test"], "blockOnFail": true },
       { "name": "Quét bảo mật", "commands": ["npm run security-scan"], "blockOnFail": false },
       { "name": "Coverage", "commands": ["npm run coverage"], "blockOnFail": true }
     ]
   }
   ```

6. **Cấu hình cơ chế bypass**

   ```json
   {
     "condition": "command.includes('git commit') && !command.includes('--no-verify')",
     "message": "Quality gate đã bật. Sử dụng --no-verify để bypass check (không khuyến nghị)."
   }
   ```

### Tiêu chuẩn nghiệm thu
- [ ] Đã định nghĩa danh sách mục check đầy đủ
- [ ] Đã tạo file cấu hình Hook
- [ ] Đã implement script kiểm tra
- [ ] Đã hỗ trợ cơ chế bypass
- [ ] Đã tài liệu hóa cách sử dụng

---

## Học tập sâu

- Đọc tài liệu Hooks đầy đủ: [../../../TaiLieuThamKhao/hooks/Hook类型.md](../../../TaiLieuThamKhao/hooks/Hook类型.md)
- Xem ví dụ Hook trong dự án
- Học cách debug Hook