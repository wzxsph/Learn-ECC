# Tác tử Kiểm thử

Tác tử Kiểm thử chịu trách nhiệm chính về phát triển hướng kiểm thử, kiểm thử đầu cuối, phân tích bao phủ kiểm thử và đảm bảo chất lượng.

## Danh sách Agent

| Tên Agent | Mục đích | Model sử dụng | Công cụ cốt lõi |
|------------|------|----------|----------|
| tdd-guide | Chuyên gia TDD phát triển hướng kiểm thử | sonnet | Read, Write, Edit, Bash, Grep |
| e2e-runner | Chuyên gia kiểm thử đầu cuối | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| mle-reviewer | Xem xét mã ML engineering (training/inference/monitoring) | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | Phân tích bao phủ kiểm thử PR | sonnet | Read, Grep, Glob, Bash |

---

## tdd-guide

### Tên và Mục đích
Chuyên gia phát triển hướng kiểm thử, thực thi phương pháp luận viết kiểm thử trước. Được sử dụng chủ động khi viết tính năng mới, sửa lỗi hoặc tái cấu trúc mã. Đảm bảo bao phủ kiểm thử 80%+.

### Khả năng
- Thực thi phương pháp luận ưu tiên kiểm thử
- Hướng dẫn chu kỳ Đỏ-Xanh-Làm sạch
- Đảm bảo bao phủ kiểm thử 80%+
- Viết bộ kiểm thử toàn diện (đơn vị, tích hợp, E2E)
- Nắm bắt trường hợp cạnh trước khi triển khai

### Khi nào sử dụng
- Khi viết tính năng mới
- Khi sửa lỗi
- Khi tái cấu trúc mã
- Khi cần quy trình TDD

### Danh sách công cụ
- Read: Đọc mã hiện có
- Write: Viết kiểm thử
- Edit: Sửa đổi kiểm thử và triển khai
- Bash: Chạy lệnh kiểm thử
- Grep: Tìm kiếm mẫu kiểm thử

### Cách phối hợp với các Agent khác
- code-reviewer xem xét mã sau chu kỳ TDD
- build-error-resolver sửa lỗi xây dựng kiểm thử
- e2e-runner thực hiện kiểm thử E2E cho luồng người dùng quan trọng

### Quy trình TDD

#### 1. Viết kiểm thử trước (ĐỎ)
Viết kiểm thử mô tả hành vi mong đợi thất bại.

#### 2. Chạy kiểm thử - Xác minh nó THẤT BẠI
```bash
npm test
```

#### 3. Viết triển khai tối thiểu (XANH)
Chỉ viết đủ mã để kiểm thử pass.

#### 4. Chạy kiểm thử - Xác minh nó ĐẠT

#### 5. Làm sạch (CẢI THIỆN)
Loại bỏ trùng lặp, cải thiện tên, tối ưu - kiểm thử phải giữ xanh.

#### 6. Xác minh bao phủ
```bash
npm run test:coverage
# Yêu cầu: 80%+ branch, function, line, statement coverage
```

### Các trường hợp cạnh phải kiểm thử

1. **Null/Undefined** đầu vào
2. **Rỗng** mảng/chuỗi
3. **Loại không hợp lệ** được truyền
4. **Giá trị biên** (tối thiểu/tối đa)
5. **Đường dẫn lỗi** (lỗi mạng, lỗi cơ sở dữ liệu)
6. **Điều kiện race** (thao tác đồng thời)
7. **Dữ liệu lớn** (hiệu suất 10k+ mục)
8. **Ký tự đặc biệt** (Unicode, emoji, ký tự SQL)

### Chống mẫu - Phải tránh

- Kiểm thử chi tiết triển khai (trạng thái nội bộ) thay vì hành vi
- Kiểm thử phụ thuộc lẫn nhau (trạng thái chia sẻ)
- Quá ít assertion (pass nhưng không xác minh gì)
- Phụ thuộc bên ngoài không mock (Supabase, Redis, OpenAI, v.v.)

### Danh sách kiểm tra chất lượng

- [ ] Tất cả hàm công cộng có kiểm thử đơn vị
- [ ] Tất cả endpoint API có kiểm thử tích hợp
- [ ] Luồng người dùng quan trọng có kiểm thử E2E
- [ ] Trường hợp cạnh đã được bao phủ (null, rỗng, không hợp lệ)
- [ ] Đường dẫn lỗi đã được kiểm thử (không chỉ happy path)
- [ ] Phụ thuộc bên ngoài đã được mock
- [ ] Kiểm thử độc lập (không trạng thái chia sẻ)
- [ ] Assertion cụ thể và có ý nghĩa
- [ ] Bao phủ 80%+

### Phụ lục TDD hướng Eval v1.8

Tích hợp phát triển hướng eval vào quy trình TDD:

1. Định nghĩa capability + regression evals trước khi triển khai
2. Chạy baseline và ghi lại đặc điểm thất bại
3. Triển khai thay đổi tối thiểu để pass
4. Chạy lại kiểm thử và evals; báo cáo pass@1 và pass@3
5. Đường dẫn quan trọng phát hành nên đạt pass^3 stability trước khi merge

---

## e2e-runner

### Tên và Mục đích
Chuyên gia kiểm thử đầu cuối, sử dụng Vercel Agent Browser (ưu tiên) và Playwright dự phòng. Được sử dụng chủ động để tạo, duy trì và chạy kiểm thử E2E.

### Khả năng
- Tạo hành trình kiểm thử - Viết kiểm thử cho luồng người dùng
- Duy trì kiểm thử - Giữ kiểm thử đồng bộ với thay đổi UI
- Quản lý kiểm thử không ổn định - Xác định và cô lập kiểm thử không ổn định
- Báo cáo kiểm thử - Tạo báo cáo HTML và JUnit XML
- Tích hợp CI/CD - Đảm bảo kiểm thử chạy đáng tin cậy trong pipeline
- Quản lý ảnh chụp/video/trace

### Khi nào sử dụng
- Khi luồng người dùng quan trọng cần xác minh
- Khi cần xác minh đầu cuối
- Khi chạy kiểm thử E2E trong pipeline CI/CD
- Khi phát hiện vấn đề tích hợp

### Danh sách công cụ
- Read: Đọc nội dung trang
- Write: Viết kiểm thử
- Edit: Sửa đổi kiểm thử
- Bash: Chạy lệnh Playwright/Agent Browser
- Grep: Tìm kiếm kiểm thử
- Glob: Tìm tệp kiểm thử

### Cách phối hợp với các Agent khác
- tdd-guide viết kiểm thử đơn vị/tích hợp
- code-reviewer xem xét chất lượng kiểm thử
- mle-reviewer xem xét kiểm thử liên quan đến ML

### Công cụ chính: Agent Browser

**Ưu tiên Agent Browser hơn Playwright thuần** - Bộ chọn ngữ nghĩa, tối ưu hóa AI, chờ đợi tự động, dựa trên Playwright.

```bash
# Thiết lập
npm install -g agent-browser && agent-browser install

# Luồng cốt lõi
agent-browser open https://example.com
agent-browser snapshot -i          # Lấy phần tử có refs [ref=e1]
agent-browser click @e1            # Click theo ref
agent-browser fill @e2 "text"     # Điền theo ref
agent-browser wait visible @e5     # Chờ phần tử nhìn thấy
agent-browser screenshot result.png
```

### Dự phòng: Playwright

Khi Agent Browser không khả dụng, sử dụng Playwright trực tiếp.

```bash
npx playwright test                        # Chạy tất cả kiểm thử E2E
npx playwright test tests/auth.spec.ts     # Chạy tệp cụ thể
npx playwright test --headed               # Trình duyệt nhìn thấy
npx playwright test --debug                # Gỡ lỗi với inspector
npx playwright test --trace on             # Chạy với trace
npx playwright show-report                 # Xem báo cáo HTML
```

### Quy trình

#### 1. Lập kế hoạch
- Xác định luồng người dùng quan trọng (auth, tính năng cốt lõi, thanh toán, CRUD)
- Định nghĩa kịch bản: happy path, trường hợp cạnh, kịch bản lỗi
- Ưu tiên theo rủi ro: CAO (tài chính, auth), TRUNG BÌNH (tìm kiếm, điều hướng), THẤP (tối ưu hóa UI)

#### 2. Tạo
- Sử dụng mẫu Page Object Model (POM)
- Ưu tiên bộ chọn `data-testid`
- Thêm assertion ở các bước quan trọng
- Chụp ảnh ở các thời điểm quan trọng
- Sử dụng chờ đợi phù hợp (không bao giờ `waitForTimeout`)

#### 3. Thực thi
- Chạy cục bộ 3-5 lần để kiểm tra không ổn định
- Sử dụng `test.fixme()` hoặc `test.skip()` để cô lập kiểm thử không ổn định
- Upload artifact vào CI

### Nguyên tắc chính

- **Sử dụng bộ chọn ngữ nghĩa**: `[data-testid="..."]` > CSS selectors > XPath
- **Chờ điều kiện không phải thời gian**: `waitForResponse()` > `waitForTimeout()`
- **Chờ đợi tự động tích hợp**: `page.locator().click()` chờ tự động; `page.click()` thuần không chờ
- **Cô lập kiểm thử**: Mỗi kiểm thử nên độc lập; không trạng thái chia sẻ
- **Fail nhanh**: Sử dụng `expect()` assertion ở mỗi bước quan trọng
- **Trace khi retry**: Cấu hình `trace: 'on-first-retry'` để gỡ lỗi thất bại

### Xử lý kiểm thử không ổn định

```typescript
// Cô lập
test('flaky: market search', async ({ page }) => {
  test.fixme(true, 'Flaky - Issue #123')
})

// Xác định không ổn định
// npx playwright test --repeat-each=10
```

Nguyên nhân phổ biến: Điều kiện race (sử dụng bộ chọn chờ tự động), timing mạng (chờ response), timing animation (chờ `networkidle`).

### Chỉ số thành công

- Tất cả luồng quan trọng đạt (100%)
- Tỷ lệ đạt tổng thể > 95%
- Tỷ lệ không ổn định < 5%
- Thời gian kiểm thử < 10 phút
- Artifact upload và accessible

---

## mle-reviewer

### Tên và Mục đích
Chuyên gia xem xét ML engineering sản xuất, tập trung vào data contract, feature pipeline, khả năng tái tạo training, đánh giá offline/online, model serving, monitoring và rollback.

### Khả năng
- Xem xét định nghĩa vấn đề và chất lượng quyết định
- Xem xét metrics, ngưỡng và phân tích lỗi
- Kiểm tra data contract và rò rỉ
- Xác minh khả năng tái tạo training
- Xem xét quy trình đánh giá và promotion
- Xem xét bảo mật serving và deployment
- Lập kế hoạch monitoring và ứng phó sự cố

### Khi nào sử dụng
- Khi thay đổi mã ML, MLOps, model training
- Khi thay đổi mã inference, feature store, đánh giá
- Khi đưa ra quyết định promotion model

### Danh sách công cụ
- Read: Đọc mã và cấu hình ML
- Grep: Tìm kiếm mẫu
- Glob: Tìm tệp
- Bash: Chạy pytest, ruff, mypy

### Cách phối hợp với các Agent khác
- python-reviewer xử lý phong cách Python, kiểu, xử lý lỗi
- pytorch-build-resolver xử lý tensor/CUDA/training failures
- database-reviewer xử lý feature tables, label storage
- security-reviewer xử lý secrets, PII, pickle security
- performance-optimizer xử lý latency, memory, GPU utilization
- build-error-resolver xử lý CI, phụ thuộc, native extension failures
- pr-test-analyzer phân tích bao phủ kiểm thử
- silent-failure-hunter phát hiện silent failures trong pipeline
- e2e-runner thực hiện kiểm thử E2E luồng sản phẩm
- a11y-architect xem xét khả năng truy cập dự đoán
- doc-updater cập nhật tài liệu

### Các lĩnh vực xem xét chính

#### Định nghĩa vấn đề và chất lượng quyết định
- Thay đổi có bắt đầu từ quyết định người dùng hoặc hệ thống, không phải ưu tiên kiến trúc model
- Stakeholder và chi phí thất bại đã được xác định rõ
- Lựa chọn metrics có tuân theo error budget không

#### Metrics, ngưỡng và phân tích lỗi
- Baseline và hành vi sản xuất hiện tại có được so sánh không
- Ngưỡng và cấu hình có được định nghĩa như quyết định sản phẩm không
- False positive và false negative có được kiểm tra trực tiếp không

#### Data contract và rò rỉ
- Entity granularity, primary keys, timestamps đã rõ ràng chưa
- Split có tuân thủ thời gian, user/entity grouping không
- Feature join có đúng thời điểm không
- Thuộc tính nhạy cảm có được loại trừ hoặc xử lý hợp lý không

#### Khả năng tái tạo training
- Training có thể chạy từ mã, cấu hình, dataset version, seeds không
- Hyperparameters, preprocessing, phiên bản phụ thuộc có được ghi lại không
- Randomness và GPU non-determinism có được xử lý có chủ đích không

#### Đánh giá và promotion
- Metrics có được so sánh với baseline và model sản xuất hiện tại không
- Promotion gate có được khai báo trước khi chọn không
- Slice metrics có bao phủ các cohort quan trọng không

#### Serving và deployment
- Training và serving transformation có được chia sẻ hoặc tương đương kiểm thử không
- Input schema có reject features lỗi thời, thiếu, không hợp lệ không
- Inference path có timeout, resource limits, fallback logic không

#### Monitoring và ứng phó sự cố
- Monitoring có bao phủ service health, feature drift, prediction drift không
- Logs có chứa đủ identifiers không
- Rollback có đặt tên artifact, cấu hình, data dependency trước đó không

### Các blockers phổ biến

- Split train/test ngẫu nhiên trên dữ liệu thời gian hoặc user-related
- Feature generation sử dụng fields không khả dụng tại thời điểm dự đoán
- Cải thiện metrics offline nhưng regression trên slice quan trọng
- Preprocessing được sao chép thủ công từ training sang serving code
- Model version không có trong prediction logs

### Tiêu chí phê duyệt

- **APPROVE**: Không có rủi ro MLE quan trọng/cao, các bài kiểm thử hoặc evaluation gate liên quan đạt
- **APPROVE WITH WARNINGS**: Chỉ có vấn đề trung bình, có hành động tiếp theo rõ ràng
- **BLOCK**: Bất kỳ rò rỉ có thể xảy ra, promotion không thể tái tạo, hành vi serving không an toàn, thiếu rollback deployment sản xuất, exposure dữ liệu nhạy cảm, hoặc gap đánh giá quan trọng

---

## pr-test-analyzer

### Tên và Mục đích
Xem xét chất lượng và tính đầy đủ của bao phủ kiểm thử Pull Request, nhấn mạnh bao phủ hành vi và ngăn ngừa lỗi thực.

### Khả năng
- Nhận diện mapping mã thay đổi
- Xác minh bao phủ hành vi
- Đánh giá chất lượng kiểm thử
- Phát hiện gap bao phủ

### Khi nào sử dụng
- Khi xem xét PR
- Khi đánh giá bao phủ kiểm thử
- Khi xác minh tính đầy đủ của kiểm thử

### Danh sách công cụ
- Read: Đọc mã thay đổi và kiểm thử
- Grep: Tìm kiếm kiểm thử
- Glob: Tìm tệp kiểm thử
- Bash: Chạy lệnh kiểm thử

### Cách phối hợp với các Agent khác
- tdd-guide cải thiện kiểm thử
- e2e-runner thực hiện bao phủ đầu cuối
- code-reviewer thực hiện xem xét chất lượng mã

### Quy trình phân tích

#### 1. Nhận diện mã thay đổi
- Map các hàm, class và module thay đổi
- Định vị kiểm thử tương ứng
- Nhận diện đường dẫn mã mới chưa được kiểm thử

#### 2. Bao phủ hành vi
- Kiểm tra mỗi chức năng có kiểm thử chưa
- Xác minh trường hợp cạnh và đường dẫn lỗi
- Đảm bảo tích hợp quan trọng được bao phủ

#### 3. Chất lượng kiểm thử
- Ưu tiên assertion có ý nghĩa hơn kiểm tra không throw
- Đánh dấu các mẫu không ổn định
- Kiểm tra tính cô lập và độ rõ ràng tên kiểm thử

#### 4. Gap bao phủ
Theo rating tác động:
- critical ( quan trọng)
- important ( quan trọng)
- nice-to-have (tốt để có)

### Định dạng đầu ra

1. Tóm tắt bao phủ
2. Gap quan trọng
3. Đề xuất cải thiện
4. Quan sát tích cực
[Quay lại Chỉ mục Agent](../README.md)