# Agent kiểm tra mã

Các Agent kiểm tra mã chịu trách nhiệm chuyên biệt cho việc kiểm tra chất lượng mã, bảo mật và khả năng bảo trì.

## Danh sách Agent

| Tên Agent | Mục đích | Mô hình sử dụng | Công cụ cốt lõi |
|------------|------|----------|----------|
| code-reviewer | Chuyên gia kiểm tra mã đa năng | sonnet | Read, Grep, Glob, Bash |
| python-reviewer | Kiểm tra mã Python (PEP 8, type hints, bảo mật) | sonnet | Read, Grep, Glob, Bash |
| go-reviewer | Kiểm tra mã Go (concurrency, xử lý lỗi, hiệu suất) | sonnet | Read, Grep, Glob, Bash |
| kotlin-reviewer | Kiểm tra mã Kotlin/Android (coroutines, Compose) | sonnet | Read, Grep, Glob, Bash |
| rust-reviewer | Kiểm tra mã Rust (ownership, an toàn lifecycle) | sonnet | Read, Grep, Glob, Bash |
| cpp-reviewer | Kiểm tra mã C++ (an toàn bộ nhớ, idiom C++ hiện đại) | sonnet | Read, Grep, Glob, Bash |
| flutter-reviewer | Kiểm tra mã Flutter/Dart (quản lý state, hiệu suất) | sonnet | Read, Grep, Glob, Bash |
| typescript-reviewer | Kiểm tra mã TypeScript/JavaScript (an toàn kiểu) | sonnet | Read, Grep, Glob, Bash |
| swift-reviewer | Kiểm tra mã Swift (protocol, concurrency, quản lý bộ nhớ ARC) | sonnet | Read, Grep, Glob, Bash |
| csharp-reviewer | Kiểm tra mã C# (.NET convention, async pattern) | sonnet | Read, Grep, Glob, Bash |
| fsharp-reviewer | Kiểm tra mã F# (idiom functional, pattern matching) | sonnet | Read, Grep, Glob, Bash |
| java-reviewer | Kiểm tra mã Java (Spring Boot/Quarkus framework) | sonnet | Read, Grep, Glob, Bash |

---

## code-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã đa năng, chủ động kiểm tra chất lượng, bảo mật và khả năng bảo trì của mã. Phải sử dụng sau mọi thay đổi mã.

### Khả năng
- Kiểm tra chất lượng mã (kích thước hàm, độ sâu lồng ghép, trùng lặp mã)
- Phát hiện lỗ hổng bảo mật (SQL injection, XSS, credentials hardcoded)
- Kiểm tra pattern React/Next.js
- Kiểm tra pattern Node.js/backend
- Nhận diện vấn đề hiệu suất
- Đề xuất thực hành tốt nhất

### Kịch bản sử dụng
- Sau khi viết hoặc sửa mã
- Kiểm tra Pull Request
- Trước khi merge vào nhánh chia sẻ

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file
- Bash: Chạy git diff và lint command

### Cách phối hợp với Agent khác
- Khi phát hiện vấn đề bảo mật, Escalation đến security-reviewer
- Khi phát hiện lỗi build, sử dụng build-error-resolver
- Khi cần refactor, sử dụng refactor-cleaner

---

## python-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã Python, tập trung vào tuân thủ PEP 8, idiom Pythonic, type hints, bảo mật và hiệu suất.

### Khả năng
- Kiểm tra style PEP 8
- Phổ biến idiom Pythonic (list comprehension, enumerate, with statement)
- Xác thực type hints
- Phát hiện SQL injection
- Kiểm tra pattern async/coroutines
- Kiểm tra framework-specific (Django, FastAPI, Flask)

### Kịch bản sử dụng
- Mọi thay đổi mã trong dự án Python
- Dự án Django/FastAPI/Flask cụ thể

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file Python
- Bash: Chạy mypy, ruff, black, bandit, pytest

### Cách phối hợp với Agent khác
- Chia sẻ quy tắc kiểm tra bảo mật với security-reviewer
- Sử dụng tdd-guide để đảm bảo coverage kiểm thử

---

## go-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã Go, tập trung vào idiom Go, pattern concurrency, xử lý lỗi và hiệu suất.

### Khả năng
- Kiểm tra idiom Go (early return, error wrapping)
- Kiểm tra an toàn concurrency (goroutine leak, channel deadlock)
- Thực hành tốt nhất xử lý lỗi
- Nhận diện pattern hiệu suất
- Phát hiện lỗ hổng bảo mật

### Kịch bản sử dụng
- Mọi thay đổi mã trong dự án Go
- Dự án cần tuân thủ quy tắc mã Go

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file Go
- Bash: Chạy go vet, staticcheck, golangci-lint

### Cách phối hợp với Agent khác
- go-build-resolver xử lý lỗi build
- security-reviewer xử lý vấn đề bảo mật

---

## kotlin-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã Kotlin và Android/KMP, tập trung vào idiom pattern, an toàn coroutines, thực hành tốt nhất Compose và vi phạm Clean Architecture.

### Khả năng
- Kiểm tra idiom Kotlin
- Phát hiện anti-pattern coroutines và Flow
- Nhận diện vấn đề hiệu suất Compose
-执行 Clean Architecture module boundary
- Kiểm tra vấn đề Android-specific

### Kịch bản sử dụng
- Phát triển Android native
- Dự án Kotlin Multiplatform (KMP)
- Dự án Jetpack Compose

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file Kotlin/KTS
- Bash: Chạy Gradle check

### Cách phối hợp với Agent khác
- kotlin-build-resolver xử lý vấn đề build
- security-reviewer xử lý vấn đề bảo mật quan trọng

---

## rust-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã Rust, tập trung vào ownership, an toàn lifecycle, xử lý lỗi, sử dụng unsafe và idiom pattern.

### Khả năng
- Kiểm tra ownership và lifecycle
- Chẩn đoán lỗi borrow checker
- Kiểm tra bảo mật code unsafe
- Xác thực pattern xử lý lỗi
- Kiểm tra an toàn concurrency

### Kịch bản sử dụng
- Mọi thay đổi mã trong dự án Rust
- Dự án cần đảm bảo an toàn bộ nhớ

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file Rust
- Bash: Chạy cargo check, clippy, fmt, test, audit

### Cách phối hợp với Agent khác
- rust-build-resolver xử lý lỗi build
- Chia sẻ quy tắc kiểm tra bảo mật với security-reviewer

---

## cpp-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã C++, tập trung vào an toàn bộ nhớ, idiom C++ hiện đại, concurrency và hiệu suất.

### Khả năng
- Kiểm tra an toàn bộ nhớ (raw new/delete, buffer overflow)
- Phổ biến pattern C++ hiện đại (smart pointer, RAII)
- Kiểm tra an toàn concurrency
- Nhận diện pattern hiệu suất
- Phát hiện lỗ hổng bảo mật

### Kịch bản sử dụng
- Mọi thay đổi mã trong dự án C++
- Dự án cần thực hành tốt nhất C++ hiện đại

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file C++
- Bash: Chạy clang-tidy, cppcheck, cmake

### Cách phối hợp với Agent khác
- cpp-build-resolver xử lý vấn đề build
- security-reviewer xử lý vấn đề bảo mật quan trọng

---

## flutter-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã Flutter và Dart, kiểm tra thực hành tốt nhất widget, pattern quản lý state, idiom Dart, bẫy hiệu suất, khả năng tiếp cận và vi phạm Clean Architecture.

### Khả năng
- Phát hiện anti-pattern quản lý state (setState solution ignoring)
- Tối ưu hóa hiệu suất widget build
-执行 kiến trúc boundary
- Quản lý resource lifecycle
- Kiểm tra khả năng tiếp cận

### Kịch bản sử dụng
- Mọi thay đổi mã trong dự án Flutter
- Phát triển ứng dụng di động cross-platform

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file Dart
- Bash: Chạy flutter analyze

### Cách phối hợp với Agent khác
- dart-build-resolver xử lý vấn đề build
- security-reviewer xử lý vấn đề bảo mật quan trọng
- e2e-runner tiến hành kiểm thử E2E

---

## typescript-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã TypeScript/JavaScript, tập trung vào an toàn kiểu, async correctness, bảo mật Node/web và idiom pattern.

### Khả năng
- Kiểm tra an toàn kiểu (lạm dụng any, non-null assertion)
- Xác thực async correctness
- Phát hiện lỗ hổng bảo mật
- Kiểm tra React/Next.js-specific
- Kiểm tra Node.js-specific

### Kịch bản sử dụng
- Mọi thay đổi mã trong dự án TypeScript/JavaScript
- Dự án Next.js và React

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file TS/TSX/JS/JSX
- Bash: Chạy tsc, eslint, prettier, npm audit

### Cách phối hợp với Agent khác
- build-error-resolver xử lý lỗi build
- security-reviewer xử lý vấn đề bảo mật quan trọng

---

## swift-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã Swift, tập trung vào thiết kế hướng protocol, value semantics, quản lý bộ nhớ ARC, Swift Concurrency và idiom pattern.

### Khả năng
- Kiểm tra an toàn Swift Concurrency
- Kiểm tra quản lý bộ nhớ (strong reference cycles, delegate reference)
- Xác thực thiết kế hướng protocol
- Thực hành tốt nhất xử lý lỗi
- Nhận diện pattern hiệu suất

### Kịch bản sử dụng
- Mọi thay đổi mã trong dự án Swift
- Phát triển ứng dụng iOS/macOS

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file Swift
- Bash: Chạy swift build, swiftlint, swift test

### Cách phối hợp với Agent khác
- swift-build-resolver xử lý vấn đề build
- security-reviewer xử lý vấn đề bảo mật quan trọng

---

## csharp-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã C#, tập trung vào .NET convention, async pattern, bảo mật, nullable reference types và hiệu suất.

### Khả năng
- Kiểm tra idiom .NET
- Xác thực async pattern
- An toàn nullable type
- Phát hiện lỗ hổng bảo mật
- Kiểm tra EF Core-specific

### Kịch bản sử dụng
- Mọi thay đổi mã trong dự án C#
- Phát triển ứng dụng .NET/.NET Core

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file C#
- Bash: Chạy dotnet build, dotnet format, dotnet test

### Cách phối hợp với Agent khác
- build-error-resolver xử lý vấn đề build .NET
- security-reviewer xử lý vấn đề bảo mật quan trọng

---

## java-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã Java cho dự án Spring Boot và Quarkus. Tự động phát hiện framework và áp dụng quy tắc kiểm tra phù hợp.

### Khả năng
- Tự động phát hiện framework (Spring Boot/Quarkus)
- Kiểm tra kiến trúc phân lớp
- Kiểm tra JPA/Panache/MongoDB
- Kiểm tra an toàn concurrency
- Phát hiện lỗ hổng bảo mật

### Kịch bản sử dụng
- Dự án Spring Boot
- Dự án Quarkus
- Dự án Java 11+

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file Java
- Bash: Chạy Maven/Gradle check

### Cách phối hợp với Agent khác
- java-build-resolver xử lý vấn đề build
- security-reviewer xử lý vấn đề bảo mật quan trọng

---

## fsharp-reviewer

### Tên và mục đích
Chuyên gia kiểm tra mã F#, tập trung vào idiom functional, an toàn kiểu, pattern matching, computational expressions và hiệu suất.

### Khả năng
- Kiểm tra idiom functional
- Xác thực tính đầy đủ của pattern matching
- Kiểm tra an toàn kiểu
- Thực hành tốt nhất computational expressions
- Nhận diện pattern hiệu suất

### Kịch bản sử dụng
- Mọi thay đổi mã trong dự án F#
- Dự án lập trình functional

### Công cụ sử dụng
- Read: Đọc nội dung file
- Grep: Tìm kiếm pattern mã
- Glob: Tìm file F#
- Bash: Chạy dotnet build, fantomas, dotnet test

### Cách phối hợp với Agent khác
- Chia sẻ quy tắc kiểm tra bảo mật với security-reviewer
- Sử dụng tdd-guide để đảm bảo coverage kiểm thử
[Quay lại Agent index](../README.md)