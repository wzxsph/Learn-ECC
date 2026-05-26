# 02 - Agent协作

Học cách thiết kế, implement và quản lý hệ thống multi-Agent协作.

## Tổng quan

ECC cung cấp **66 Agent chuyên nghiệp [](../../TaiLieuThamKhao/agents/1.%20代码审查类.md)**，分为 6 loại lớn. Các Agent này làm việc协同 qua workflow chuẩn hóa, hoàn thành các nhiệm vụ phức tạp từ code review đến thiết kế hệ thống.

---

## 1. Kiến trúc Agent

### 1.1 Đơn Agent vs Đa Agent

| Kiến trúc | Kịch bản phù hợp | Đặc điểm |
|------|----------|------|
| **Đơn Agent** | Nhiệm vụ đơn giản, thao tác độc lập | Gọi trực tiếp, phản hồi nhanh |
| **Đa Agent** | Nhiệm vụ phức tạp, cần协作 | Phân rã nhiệm vụ, tổng hợp kết quả |

### 1.2 Danh sách Agent khả dụng

| Agent | Mục đích | Thời điểm sử dụng |
|-------|------|----------|
| **planner** | Lập kế hoạch triển khai | Tính năng phức tạp, refactor |
| **architect** | Thiết kế hệ thống | Quyết định kiến trúc |
| **tdd-guide** | Phát triển TDD | Tính năng mới, sửa bug |
| **code-reviewer** | Review code | Sau khi viết code |
| **security-reviewer** | Phân tích bảo mật | Trước khi commit |
| **build-error-resolver** | Sửa lỗi build | Build thất bại |
| **e2e-runner** | Test đầu cuối | User flow quan trọng |
| **refactor-cleaner** | Dọn dead code | Bảo trì code |
| **doc-updater** | Cập nhật tài liệu | Cập nhật tài liệu |
| **rust-reviewer** | Review code Rust | Dự án Rust |
| **harmonyos-app-resolver** | Phát triển ứng dụng HarmonyOS | Dự án HarmonyOS/ArkTS |

### 1.3 Format file Agent

```yaml
---
name: agent-name
description: Mục đích Agent
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

---

## 2. Phân loại Agent chi tiết

### 2.1 Agent审查类（16个）

Chịu trách nhiệm review chất lượng code, bảo mật và khả năng bảo trì.

| Agent | Chuyên môn |
|-------|------|
| code-reviewer | Review code thông thường |
| security-reviewer | Phân tích lỗ hổng bảo mật |
| python-reviewer | Python PEP 8, type hint |
| go-reviewer | Go idioms, đồng thời |
| kotlin-reviewer | Kotlin null safety, coroutines |
| rust-reviewer | Rust ownership, vòng đời |
| cpp-reviewer | C++ bảo mật bộ nhớ |
| flutter-reviewer | Flutter/Dart patterns |
| fastapi-reviewer | FastAPI kiến trúc, async |
| typescript-reviewer | TypeScript type safety |
| java-reviewer | Java coding standards |
| csharp-reviewer | C# best practices |
| ruby-reviewer | Ruby idioms |
| php-reviewer | PHP bảo mật và performance |
| swift-reviewer | Swift đồng thời, quản lý bộ nhớ |
| go-security-reviewer | Go security review |

### 2.2 Agent构建修复类（10个）

Tập trung vào sửa lỗi build của các ngôn ngữ khác nhau.

| Agent | Chuyên môn |
|-------|------|
| build-error-resolver | Sửa lỗi build thông thường |
| go-build | Go build + go vet |
| kotlin-build | Kotlin/Gradle biên dịch |
| rust-build | Rust borrow checker |
| cpp-build | C++ CMake + linker |
| gradle-build | Android/KMP Gradle |
| flutter-build | Flutter build |
| maven-build | Maven build |
| cmake-build | CMake cấu hình |
| ninja-build | Ninja build system |

### 2.3 Agent规划类（5个）

Chịu trách nhiệm phân tích yêu cầu và lập kế hoạch hệ thống.

| Agent | Chuyên môn |
|-------|------|
| planner | Lập kế hoạch triển khai |
| architect | Thiết kế kiến trúc hệ thống |
| prp-planner | PRP workflow planning |
| multi-plan | Lập kế hoạch đa mô hình |
| product-manager | Phân tích yêu cầu sản phẩm |

### 2.4 Agent测试类（2个）

Tập trung vào phát triển driven bằng test và coverage.

| Agent | Chuyên môn |
|-------|------|
| tdd-guide | Hướng dẫn workflow TDD |
| e2e-runner | Playwright E2E test |

### 2.5 Agent bảo mật类（5个）

Chịu trách nhiệm review bảo mật và quét lỗ hổng.

| Agent | Chuyên môn |
|-------|------|
| security-reviewer | Review bảo mật thông thường |
| owasp-reviewer | OWASP Top 10 |
| pentest-reviewer | Penetration test |
| vulnerability-scanner | Quét lỗ hổng |
| secrets-detector | Phát hiện key và credentials |

### 2.6 Agent kiến trúc类（5个）

Chịu trách nhiệm thiết kế hệ thống và quyết định kỹ thuật.

| Agent | Chuyên môn |
|-------|------|
| architect | Thiết kế kiến trúc hệ thống |
| microservices-architect | Kiến trúc microservice |
| frontend-architect | Kiến trúc frontend |
| backend-architect | Kiến trúc backend |
| devops-architect | Kiến trúc DevOps |

---

## 3. Mô hình协作

### 3.1 Mô hình chủ - tớ

Một Agent chủ điều phối nhiều Agent tớ:

```
Yêu cầu người dùng → Agent chủ (planner)
           ├── Agent con 1 (code-reviewer)
           ├── Agent con 2 (tdd-guide)
           └── Agent con 3 (build-error-resolver)
```

### 3.2 Mô hình ngang hàng

Nhiều Agent làm việc song song, kết quả tổng hợp:

```
Yêu cầu người dùng → Agent 1 (phân tích bảo mật)
        → Agent 2 (phân tích performance)
        → Agent 3 (phân tích chất lượng code)
           ↓ Tổng hợp kết quả
        Báo cáo cuối cùng
```

### 3.3 Mô hình pipeline

Agent xử lý theo thứ tự, output của mỗi Agent là input của Agent tiếp theo:

```
Input → Agent 1 → Agent 2 → Agent 3 → Output
```

---

## 4. Chiến lược phân phối nhiệm vụ

### 4.1 Phân rã nhiệm vụ

Phân rã nhiệm vụ phức tạp thành các sub-task độc lập:

```markdown
# Ví dụ phân rã nhiệm vụ phức tạp
Nhiệm vụ gốc: Implement hệ thống xác thực người dùng

Sau khi phân rã:
1. Thiết kế database schema → architect
2. Implement API endpoint → backend-developer
3. Viết test → tdd-guide
4. Review bảo mật → security-reviewer
```

### 4.2 Thực thi song song

Các nhiệm vụ độc lập thực thi song song để nâng cao hiệu quả:

```markdown
# Tốt: Thực thi song song
Khởi động 3 Agent song song:
1. Agent 1: Phân tích bảo mật module xác thực
2. Agent 2: Review performance hệ thống cache
3. Agent 3: Kiểm tra type của utility

# Xấu: Thực thi tuần tự không cần thiết
Agent 1, rồi Agent 2, rồi Agent 3
```

### 4.3 Cân bằng tải

Phân phối nhiệm vụ theo tải Agent:

| Yếu tố | Mô tả |
|------|------|
| Độ phức tạp nhiệm vụ | Nhiệm vụ đơn giản → Agent nhẹ |
| Nhu cầu tài nguyên | Tính toán nặng → Agent chuyên dụng |
| Khả năng | Tránh Agent quá tải |

---

## 5. Phân tích đa góc nhìn

Với các vấn đề phức tạp, sử dụng sub-agents đa vai trò:

| Vai trò | Trách nhiệm |
|------|------|
| Người kiểm tra thực tế | Xác minh độ chính xác của thông tin và dữ liệu |
| Kỹ sư cao cấp | Đánh giá tính khả thi kỹ thuật và best practices |
| Chuyên gia bảo mật | Nhận diện rủi ro bảo mật và lỗ hổng |
| Người kiểm tra nhất quán | Đảm bảo tính nhất quán nội bộ của giải pháp |
| Người kiểm tra trùng lặp | Nhận diện sự trùng lặp và lãng phí |

---

## 6. Giao tiếp Agent

### 6.1 Gọi trực tiếp

```markdown
Sử dụng planner agent tạo kế hoạch triển khai
```

### 6.2 Ủy thác nhiệm vụ

```markdown
Ủy thác nhiệm vụ code review cho code-reviewer agent
```

### 6.3 Tổng hợp kết quả

Kết quả của nhiều Agent cần được tổng hợp:

```markdown
Tổng hợp từ:
- security-reviewer: Danh sách vấn đề bảo mật
- code-reviewer: Danh sách vấn đề chất lượng code
- tdd-guide: Báo cáo coverage test

→ Tạo báo cáo tổng hợp
```

---

## 7. Nguyên tắc sử dụng ngay

Không cần chờ yêu cầu từ người dùng:

| Kịch bản | Agent |
|------|-------|
| Yêu cầu tính năng phức tạp | **planner** |
| Code vừa viết/xong | **code-reviewer** |
| Sửa bug hoặc tính năng mới | **tdd-guide** |
| Quyết định kiến trúc | **architect** |
| Build thất bại | **build-error-resolver** |
| Liên quan bảo mật | **security-reviewer** |

---

## Bài tập

Hoàn thành [bài tập](./exercises/练习.md) trong phần này.

---

[Quay lại thư mục khả năng cốt lõi](../README.md)