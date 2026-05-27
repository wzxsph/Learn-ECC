# Tác tử Bảo mật

Tác tử Bảo mật chịu trách nhiệm chính về phát hiện lỗ hổng bảo mật, xem xét tuân thủ và bảo vệ thông tin nhạy cảm.

## Danh sách Agent

| Tên Agent | Mục đích | Model sử dụng | Công cụ cốt lõi |
|------------|------|----------|----------|
| security-reviewer | Chuyên gia phát hiện và sửa lỗ hổng bảo mật | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| silent-failure-hunter | Chuyên gia phát hiện lỗi thầm lặng | sonnet | Read, Grep, Glob, Bash |
| healthcare-reviewer | Xem xét mã ứng dụng y tế (PHI/Bảo mật lâm sàng) | opus | Read, Grep, Glob |
| opensource-sanitizer | Chuyên gia kiểm tra rò rỉ dự án nguồn mở | sonnet | Read, Grep, Glob, Bash |

---

## security-reviewer

### Tên và Mục đích
Chuyên gia phát hiện và sửa lỗ hổng bảo mật. Được sử dụng chủ động sau khi viết mã xử lý đầu vào người dùng, xác thực, endpoint API hoặc dữ liệu nhạy cảm. Đánh dấu secrets, SSRF, injection, mã hóa không an toàn và lỗ hổng OWASP Top 10.

### Khả năng
- Phát hiện lỗ hổng - Nhận diện OWASP Top 10 và vấn đề bảo mật phổ biến
- Phát hiện Secrets - Phát hiện API keys, passwords, tokens được hardcode
- Xác thực đầu vào - Đảm bảo tất cả đầu vào người dùng được làm sạch đúng cách
- Xác thực/Ủy quyền - Xác minh kiểm soát truy cập phù hợp
- Bảo mật phụ thuộc - Kiểm tra npm packages có lỗ hổng
- Best practice bảo mật - Thực thi các mẫu mã hóa an toàn

### Khi nào sử dụng
- Endpoint API mới
- Thay đổi mã xác thực
- Xử lý đầu vào người dùng
- Thay đổi truy vấn cơ sở dữ liệu
- Upload file
- Mã thanh toán
- Tích hợp API bên ngoài
- Cập nhật phụ thuộc

### Danh sách công cụ
- Read: Đọc mã
- Write: Ghi sửa bảo mật
- Edit: Sửa sửa bảo mật
- Bash: Chạy npm audit, eslint
- Grep: Tìm kiếm mẫu nhạy cảm
- Glob: Tìm tệp

### Cách phối hợp với các Agent khác
- code-reviewer chia sẻ kiểm tra chất lượng mã
- healthcare-reviewer xử lý vấn đề bảo mật cụ thể của y tế
- opensource-sanitizer xử lý kiểm tra trước phát hành nguồn mở

### Quét ban đầu

```bash
npm audit --audit-level=high
npx eslint . --plugin security
```

Tìm kiếm secrets được hardcode, xem xét các vùng rủi ro cao: auth, endpoint API, truy vấn DB, upload file, thanh toán, webhooks.

### Kiểm tra OWASP Top 10

1. **Injection** - Truy vấn có được parameterize không? Đầu vào người dùng có được làm sạch không? ORM có được sử dụng an toàn không?
2. **Broken Authentication** - Mật khẩu có được hash (bcrypt/argon2) không? JWT có được xác minh không? Session có an toàn không?
3. **Sensitive Data** - HTTPS có được enforce không? Secrets có trong env vars không? PII có được mã hóa không? Logs có được làm sạch không?
4. **XXE** - XML parser có được cấu hình an toàn không? External entities có bị disable không?
5. **Broken Access Control** - Mỗi route có kiểm tra auth không? CORS có được cấu hình đúng không?
6. **Security Misconfiguration** - Default credentials có được thay đổi không? Debug mode production có được tắt không? Security headers có được set không?
7. **XSS** - Output có được escape không? CSP có được set không? Framework có tự động escape không?
8. **Insecure Deserialization** - Đầu vào người dùng có được deserialize an toàn không?
9. **Known Vulnerabilities** - Phụ thuộc có được cập nhật không? npm audit có pass không?
10. **Insufficient Logging** - Sự kiện bảo mật có được ghi lại không? Alerts có được cấu hình không?

### Xem xét mẫu mã

Đánh dấu ngay các mẫu này:

| Mẫu | Mức độ nghiêm trọng | Sửa |
|------|--------|------|
| Hardcoded secrets | CRITICAL | Sử dụng `process.env` |
| Shell commands với đầu vào người dùng | CRITICAL | Sử dụng API an toàn hoặc execFile |
| String concatenation SQL | CRITICAL | Truy vấn parameterized |
| `innerHTML = userInput` | HIGH | Sử dụng `textContent` hoặc DOMPurify |
| `fetch(userProvidedUrl)` | HIGH | Whitelist domains cho phép |
| Plaintext password comparison | CRITICAL | Sử dụng `bcrypt.compare()` |
| Route không có auth check | CRITICAL | Thêm auth middleware |
| Balance check không có lock | CRITICAL | Sử dụng `FOR UPDATE` trong transaction |
| Không có rate limiting | HIGH | Thêm `express-rate-limit` |
| Ghi log password/secrets | MEDIUM | Làm sạch log output |

### Nguyên tắc chính

1. **Phòng thủ theo chiều sâu** - Nhiều lớp bảo mật
2. **Ít quyền nhất** - Quyền tối thiểu cần thiết
3. **Thất bại an toàn** - Lỗi không nên expose dữ liệu
4. **Không tin tưởng đầu vào** - Xác minh và làm sạch mọi thứ
5. **Cập nhật thường xuyên** - Giữ phụ thuộc cập nhật

### False positives phổ biến

- Environment variables trong `.env.example` (không phải secrets thực)
- Test credentials trong file kiểm thử (nếu được đánh dấu rõ ràng)
- API keys công khai thực sự
- SHA256/MD5 dùng cho checksum (không phải mật khẩu)

**Luôn xác minh context trước khi đánh dấu.**

### Ứng phó khẩn cấp

Nếu phát hiện lỗ hổng CRITICAL:
1. Ghi lại báo cáo chi tiết
2. Thông báo ngay cho project lead
3. Cung cấp ví dụ mã an toàn
4. Xác minh sửa có hiệu quả
5. Nếu credentials bị expose thì rotate secrets

---

## silent-failure-hunter

### Tên và Mục đích
Xem xét mã để tìm lỗi thầm lặng, errors bị nuốt, fallback sai và thiếu error propagation.

### Khả năng
- Phát hiện empty catch blocks
- Phát hiện logging không đủ
- Phát hiện fallback nguy hiểm
- Phát hiện vấn đề error propagation
- Phát hiện thiếu xử lý lỗi

### Khi nào sử dụng
- Khi xem xét mã
- Khi tìm thấy lỗi lạ
- Khi pipeline có vẻ xanh nhưng skip data
- Khi xem xét exception handling

### Danh sách công cụ
- Read: Đọc mã
- Grep: Tìm kiếm mẫu xử lý lỗi
- Glob: Tìm tệp nguồn
- Bash: Chạy kiểm thử

### Cách phối hợp với các Agent khác
- code-reviewer chia sẻ kiểm tra chất lượng mã
- security-reviewer xử lý vấn đề liên quan đến bảo mật
- mle-reviewer xử lý vấn đề ML pipeline

### Các mục tiêu săn lùng

#### 1. Empty Catch Blocks
- `catch {}` hoặc exceptions bị bỏ qua
- Chuyển thành `null` / mảng rỗng nhưng không có context lỗi

#### 2. Logging không đủ
- Log context không đủ
- Severity lỗi không đúng
- log-and-forget handling

#### 3. Fallback nguy hiểm
- Default values ẩn thất bại thực sự
- `.catch(() => [])`
- Đường dẫn graceful nhưng làm cho bug downstream khó chẩn đoán hơn

#### 4. Vấn đề Error Propagation
- Stack trace bị mất
- Generic rethrows
- Thiếu async handling

#### 5. Thiếu xử lý lỗi
- Paths không có timeout hoặc xử lý lỗi cho network/file/database
- Transaction work không có rollback

### Định dạng đầu ra

Mỗi phát hiện:
- Vị trí
- Mức độ nghiêm trọng
- Vấn đề
- Tác động
- Đề xuất sửa

---

## healthcare-reviewer

### Tên và Mục đích
Xem xét mã ứng dụng y tế cho bảo mật lâm sàng, độ chính xác CDSS, tuân thủ PHI và tính toàn vẹn dữ liệu y tế. Được thiết kế cho EMR/EHR, clinical decision support và health information systems.

### Khả năng
- Độ chính xác CDSS - Xác minh tương tác thuốc, quy tắc xác thực liều lượng và clinical scoring
- Bảo vệ PHI/PII - Quét patient data exposure trong logs, errors, responses, URLs và client storage
- Tính toàn vẹn dữ liệu lâm sàng - Đảm bảo audit trails, khóa records và bảo vệ cascade
- Độ chính xác dữ liệu y tế - Xác minh ICD-10/SNOMED mappings, laboratory reference ranges và drug database entries
- Tuân thủ tích hợp - Xác minh xử lý và phục hồi lỗi HL7/FHIR messages

### Khi nào sử dụng
- Phát triển hệ thống EMR/EHR
- Clinical decision support systems
- Health information systems
- Ứng dụng xử lý dữ liệu y tế

### Danh sách công cụ
- Read: Đọc mã y tế
- Grep: Tìm kiếm mẫu PHI
- Glob: Tìm tệp liên quan đến y tế

### Cách phối hợp với các Agent khác
- security-reviewer xử lý lỗ hổng bảo mật chung
- code-reviewer xử lý vấn đề chất lượng mã
- silent-failure-hunter xử lý vấn đề lỗi thầm lặng

### Kiểm tra chính

#### CDSS Engine
- [ ] Tất cả các cặp tương tác thuốc tạo ra alerts đúng (hai chiều)
- [ ] Quy tắc xác thực liều lượng trigger khi giá trị ngoài phạm vi
- [ ] Clinical scoring match với spec publish
- [ ] Không có false negatives (tương tác bị bỏ qua = sự kiện an toàn bệnh nhân)
- [ ] Input sai định dạng tạo ra lỗi, không pass thầm lặng

#### Bảo vệ PHI
- [ ] Patient data không ở trong `console.log`, `console.error` hoặc error messages
- [ ] PHI không ở trong URL parameters hoặc query strings
- [ ] PHI không ở trong browser localStorage/sessionStorage
- [ ] Không có `service_role` keys trong client code
- [ ] RLS enabled trên tất cả patient data tables
- [ ] Cross-facility data isolation đã được xác minh

#### Clinical Workflow
- [ ] Chart lock ngăn chặn edit (chỉ addendum)
- [ ] Mỗi clinical data CRUD có audit trail entry
- [ ] Critical alerts không thể bỏ qua (không phải toast notifications)
- [ ] Ghi lại lý do override khi clinician proceed với critical alert
- [ ] Red flag symptoms trigger visible alerts

#### Tính toàn vẹn dữ liệu
- [ ] Patient records không có CASCADE DELETE
- [ ] Concurrent edit detection (optimistic locking hoặc conflict resolution)
- [ ] Không có orphan records trong clinical tables
- [ ] Timestamps sử dụng timezone nhất quán

### Định dạng đầu ra

```
## Healthcare Review: [module/feature]

### Patient Safety Impact: [CRITICAL / HIGH / MEDIUM / LOW / NONE]

### Clinical Accuracy
- CDSS: [kiểm tra pass/fail]
- Drug DB: [xác minh/vấn đề]
- Scoring: [match spec/deviation]

### PHI Compliance
- Exposure vectors check: [danh sách]
- Issues found: [danh sách hoặc không]

### Issues
1. [PATIENT SAFETY / CLINICAL / PHI / TECHNICAL] Mô tả
   - Impact: [potential harm hoặc exposure]
   - Fix: [thay đổi cần thiết]

### Verdict: [SAFE TO DEPLOY / NEEDS FIXES / BLOCK — PATIENT SAFETY RISK]
```

### Quy tắc

- Khi có nghi vấn về độ chính xác lâm sàng, đánh dấu NEEDS REVIEW - không bao giờ approve clinical logic không chắc chắn
- Một tương tác thuốc bị bỏ qua tệ hơn một trăm false alerts
- PHI exposure luôn là CRITICAL severity, bất kể leak nhỏ đến đâu
- Không bao giờ approve code nuốt thầm CDSS errors

---

## opensource-sanitizer

### Tên và Mục đích
Xác minh open source fork đã được dọn dẹp hoàn toàn trước khi phát hành. Quét secrets rò rỉ, PII, internal references và file nguy hiểm. Sử dụng 20+ regex patterns. Tạo báo cáo PASS/FAIL/PASS-WITH-WARNINGS.

### Khả năng
- Secrets scanning - Quét API keys, passwords, tokens
- PII scanning - Quét email, IP addresses
- Internal references scanning - Quét absolute paths, internal domain names
- Dangerous files check - Kiểm tra .env, credentials.json, v.v.
- Config completeness validation - Xác minh .env.example
- Git history audit - Kiểm tra credentials rò rỉ

### Khi nào sử dụng
- Trước khi phát hành open source fork
- Trước khi audit code của bên thứ ba
- Kiểm tra bảo mật trước khi phát hành code

### Danh sách công cụ
- Read: Đọc tệp
- Grep: Tìm kiếm mẫu nhạy cảm
- Glob: Tìm tệp
- Bash: Chạy git log, v.v.

### Cách phối hợp với các Agent khác
- security-reviewer xử lý lỗ hổng bảo mật chung
- code-reviewer xử lý vấn đề chất lượng mã
- doc-updater cập nhật tài liệu

### Workflow

#### Bước 1: Secrets Scanning (CRITICAL)

Quét mỗi tệp văn bản (loại trừ `node_modules`, `.git`, `__pycache__`, `*.min.js`, binary files):

```
# API keys
pattern: [A-Za-z0-9_]*(api[_-]?key|apikey|api[_-]?secret)[A-Za-z0-9_]*\s*[=:]\s*['"]?[A-Za-z0-9+/=_-]{16,}

# AWS
pattern: AKIA[0-9A-Z]{16}
pattern: (?i)(aws_secret_access_key|aws_secret)\s*[=:]\s*['"]?[A-Za-z0-9+/=]{20,}

# Database URLs (with credentials)
pattern: (postgres|mysql|mongodb|redis)://[^:]+:[^@]+@[^\s'"]+

# JWT tokens (3-segment)
pattern: eyJ[A-Za-z0-9_-]{20,}\.eyJ[A-Za-z0-9_-]{20,}\.[A-Za-z0-9_-]+

# Private keys
pattern: -----BEGIN\s+(RSA\s+|EC\s+|DSA\s+|OPENSSH\s+)?PRIVATE KEY-----

# GitHub tokens
pattern: gh[pousr]_[A-Za-z0-9_]{36,}
pattern: github_pat_[A-Za-z0-9_]{22,}
```

#### Bước 2: PII Scanning (CRITICAL)

```
# Personal emails (không phải generic như noreply@, info@)
pattern: [a-zA-Z0-9._%+-]+@(gmail|yahoo|hotmail|outlook|protonmail|icloud)\.(com|net|org)

# Private IP addresses (indicates internal infrastructure)
pattern: (192\.168\.\d+\.\d+|10\.\d+\.\d+\.\d+|172\.(1[6-9]|2\d|3[01])\.\d+\.\d+)

# SSH connection strings
pattern: ssh\s+[a-z]+@[0-9.]+
```

#### Bước 3: Internal References Scanning (CRITICAL)

```
# Absolute paths to specific user home directories
pattern: /home/[a-z][a-z0-9_-]*/
pattern: /Users/[A-Za-z][A-Za-z0-9_-]*/
pattern: C:\\Users\\[A-Za-z]

# Internal secret file references
pattern: \.secrets/
pattern: source\s+~/\.secrets/
```

#### Bước 4: Dangerous Files Check (CRITICAL)

Xác minh những thứ này **không tồn tại**:
```
.env (any variant: .env.local, .env.production, .env.*.local)
*.pem, *.key, *.p12, *.pfx, *.jks
credentials.json, service-account*.json
.secrets/, secrets/
.claude/settings.json
sessions/
*.map (source maps expose raw source structure)
node_modules/, __pycache__/, .venv/, venv/
```

#### Bước 5: Git History Audit

```bash
# Nên là single initial commit
cd PROJECT_DIR
git log --oneline | wc -l
# Nếu > 1, history chưa được dọn — FAIL

# Tìm secrets có thể có
git log -p | grep -iE '(password|secret|api.?key|token)' | head -20
```

### Định dạng đầu ra

Tạo `SANITIZATION_REPORT.md` trong thư mục project:

```markdown
# Sanitization Report: {project-name}

**Date:** {date}
**Auditor:** opensource-sanitizer v1.0.0
**Verdict:** PASS | FAIL | PASS WITH WARNINGS

## Summary

| Category | Status | Findings |
|----------|--------|----------|
| Secrets | PASS/FAIL | {count} findings |
| PII | PASS/FAIL | {count} findings |
| Internal References | PASS/FAIL | {count} findings |
| Dangerous Files | PASS/FAIL | {count} findings |
| Config Completeness | PASS/WARN | {count} findings |
| Git History | PASS/FAIL | {count} findings |

## Critical Findings (Must Fix Before Release)

1. **[SECRETS]** `src/config.py:42` — Hardcoded database password: `DB_P...` (truncated)

## Warnings (Review Before Release)

1. **[CONFIG]** `src/app.py:8` — Port 8080 hardcoded, should be configurable

## .env.example Audit

- Variables in code but NOT in .env.example: {list}
- Variables in .env.example but NOT in code: {list}

## Recommendation

{If FAIL: "Fix the {N} critical findings and re-run sanitizer."}
{If PASS: "Project is clear for open-source release. Proceed to packager."}
{If WARNINGS: "Project passes critical checks. Review {N} warnings before release."}
```

### Quy tắc

- **Không bao giờ** hiển thị full secret value - truncate thành 4 ký tự đầu + "..."
- **Không bao giờ** sửa đổi source files - chỉ tạo báo cáo (SANITIZATION_REPORT.md)
- **Luôn** quét mỗi tệp văn bản, không chỉ extensions đã biết
- **Luôn** kiểm tra git history, kể cả repo mới
- **Giữ sự hoài nghi** - false positive có thể chấp nhận, miss là không thể
- Single CRITICAL finding trong bất kỳ category nào = overall FAIL
- Warnings đơn lẻ = PASS WITH WARNINGS (user quyết định)
[Quay lại Chỉ mục Agent](../README.md)