# Agent sửa lỗi build

Các Agent sửa lỗi build chịu trách nhiệm chuyên biệt cho việc chẩn đoán và sửa lỗi build, lỗi biên dịch và vấn đề dependencies cho nhiều ngôn ngữ lập trình.

## Danh sách Agent

| Tên Agent | Mục đích | Mô hình sử dụng | Công cụ cốt lõi |
|------------|------|----------|----------|
| build-error-resolver | Sửa lỗi build TypeScript/generic | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| go-build-resolver | Sửa lỗi build/compile Go | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| kotlin-build-resolver | Sửa lỗi build Kotlin/Gradle | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| rust-build-resolver | Sửa lỗi build/borrow checker Rust | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| cpp-build-resolver | Sửa lỗi build C++/CMake | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| java-build-resolver | Sửa lỗi build Java/Maven/Gradle | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| swift-build-resolver | Sửa lỗi build Swift/Xcode | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| dart-build-resolver | Sửa lỗi build Dart/Flutter | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| django-build-resolver | Sửa vấn đề build Django/Python | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| pytorch-build-resolver | Sửa vấn đề build PyTorch/CUDA | sonnet | Read, Write, Edit, Bash, Grep, Glob |

---

## build-error-resolver

### Tên và mục đích
Chuyên gia giải quyết lỗi build và type TypeScript. Sửa lỗi build thất bại hoặc lỗi type, tập trung vào việc làm cho build xanh, không thực hiện thay đổi kiến trúc.

### Khả năng
- Giải quyết lỗi TypeScript
- Sửa lỗi build
- Sửa vấn đề dependencies
- Giải quyết lỗi cấu hình
- Sửa tối thiểu diff

### Kịch bản sử dụng
- Khi build thất bại
- Khi có lỗi type TypeScript
- Khi có vấn đề module resolution

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy tsc, npm build, eslint
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- Không làm refactor → Sử dụng refactor-cleaner
- Không thay đổi kiến trúc → Sử dụng architect
- Không thêm tính năng mới → Sử dụng planner
- Không sửa test → Sử dụng tdd-guide
- Không xử lý vấn đề bảo mật → Sử dụng security-reviewer

### Nguyên tắc cốt lõi
- Chỉ sửa lỗi, không refactor
- Thay đổi tối thiểu
- Xác minh build pass sau mỗi sửa chữa

---

## go-build-resolver

### Tên và mục đích
Chuyên gia giải quyết lỗi build, vet và compile Go. Sửa lỗi build Go, vấn đề go vet và cảnh báo linter.

### Khả năng
- Chẩn đoán lỗi compile Go
- Sửa cảnh báo go vet
- Sửa vấn đề staticcheck/golangci-lint
- Giải quyết vấn đề module dependencies
- Xử lý lỗi type và interface mismatch

### Kịch bản sử dụng
- Khi build Go thất bại
- Khi go vet báo lỗi
- Khi có xung đột module dependencies

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy go build, go vet, staticcheck, golangci-lint
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- go-reviewer kiểm tra chất lượng mã
- security-reviewer xử lý vấn đề bảo mật

### Pattern sửa lỗi phổ biến

| Loại lỗi | Nguyên nhân | Cách sửa |
|----------|------|----------|
| undefined: X | Thiếu import, lỗi chính tả | Thêm import hoặc sửa case |
| cannot use X as type Y | Type không khớp | Type conversion hoặc dereference |
| X does not implement Y | Thiếu method | Implement method với đúng receiver |
| import cycle not allowed | Cycle dependency | Extract shared type sang package mới |
| cannot find package | Thiếu dependency | go get pkg@version hoặc go mod tidy |

---

## kotlin-build-resolver

### Tên và mục đích
Chuyên gia giải quyết lỗi build, compile và dependency Kotlin/Gradle. Sửa lỗi build Kotlin, lỗi compiler Kotlin và vấn đề Gradle.

### Khả năng
- Chẩn đoán lỗi compile Kotlin
- Sửa vấn đề cấu hình Gradle build
- Giải quyết xung đột dependency và version mismatch
- Xử lý lỗi Kotlin compiler
- Sửa vi phạm detekt và ktlint

### Kịch bản sử dụng
- Khi build Kotlin thất bại
- Khi có xung đột Gradle dependency
- Khi Kotlin compiler báo lỗi

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy ./gradlew build, detekt, ktlintCheck
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- kotlin-reviewer kiểm tra chất lượng mã
- security-reviewer xử lý vấn đề bảo mật

### Pattern sửa lỗi phổ biến

| Loại lỗi | Nguyên nhân | Cách sửa |
|----------|------|----------|
| Unresolved reference: X | Thiếu import, lỗi chính tả | Thêm import hoặc dependency |
| Type mismatch | Lỗi type | Thêm conversion hoặc sửa type |
| Smart cast impossible | Thuộc tính mutable hoặc concurrent access | Sử dụng bản sao val cục bộ |
| when expression must be exhaustive | sealed class when thiếu branch | Thêm branch còn thiếu hoặc else |
| Suspend function can only be called from coroutine | Thiếu suspend hoặc coroutine scope | Thêm suspend modifier |

---

## rust-build-resolver

### Tên và mục đích
Chuyên gia giải quyết lỗi build, compile và dependency Rust. Sửa lỗi cargo build, vấn đề borrow checker và Cargo.toml.

### Khả năng
- Chẩn đoán lỗi cargo build/check
- Sửa lỗi borrow checker và lifecycle
- Giải quyết trait implementation mismatch
- Xử lý Cargo dependency và vấn đề feature
- Sửa cảnh báo cargo clippy

### Kịch bản sử dụng
- Khi build Rust thất bại
- Khi borrow checker báo lỗi
- Khi có xung đột Cargo dependency

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy cargo check, clippy, fmt, tree
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- rust-reviewer kiểm tra chất lượng mã
- security-reviewer xử lý vấn đề bảo mật

### Pattern sửa lỗi phổ biến

| Loại lỗi | Nguyên nhân | Cách sửa |
|----------|------|----------|
| cannot borrow as mutable | Immutable borrow activated | Refactor để immutable borrow kết thúc trước |
| does not live long enough | Value dropped while still borrowed | Mở rộng scope của lifecycle |
| cannot move out of | Move sau reference | Sử dụng .clone() hoặc refactor |
| mismatched types | Lỗi type | Thêm .into(), as hoặc conversion rõ ràng |
| trait X is not implemented for Y | Thiếu impl hoặc derive | Thêm #[derive(Trait)] |

---

## cpp-build-resolver

### Tên và mục đích
Chuyên gia giải quyết lỗi build C++, CMake và compile. Sửa lỗi build, vấn đề linker và lỗi template.

### Khả năng
- Chẩn đoán lỗi compile C++
- Sửa vấn đề cấu hình CMake
- Giải quyết lỗi linker (undefined reference, duplicate definition)
- Xử lý lỗi template instantiation
- Sửa vấn đề include và dependency

### Kịch bản sử dụng
- Khi build C++ thất bại
- Khi cấu hình CMake lỗi
- Khi linker báo lỗi

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy cmake --build, clang-tidy, cppcheck
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- cpp-reviewer kiểm tra chất lượng mã
- security-reviewer xử lý vấn đề bảo mật

### Pattern sửa lỗi phổ biến

| Loại lỗi | Nguyên nhân | Cách sửa |
|----------|------|----------|
| undefined reference to X | Thiếu implementation hoặc library | Thêm source file hoặc link library |
| no matching function for call | Lỗi tham số type | Sửa type hoặc thêm overload |
| multiple definition of | Duplicate symbol | Sử dụng inline hoặc chuyển vào .cpp |
| cannot convert X to Y | Type không khớp | Thêm conversion hoặc sửa type |
| template argument deduction failed | Lỗi tham số template | Sửa template argument |

---

## java-build-resolver

### Tên và mục đích
Chuyên gia giải quyết lỗi build, compile và dependency Java/Maven/Gradle. Tự động phát hiện Spring Boot hoặc Quarkus và áp dụng framework-specific fixes.

### Khả năng
- Chẩn đoán lỗi compile Java
- Sửa vấn đề cấu hình Maven và Gradle build
- Giải quyết xung đột dependency và version mismatch
- Xử lý lỗi annotation processor (Lombok, MapStruct, Spring, Quarkus)
- Sửa vi phạm Checkstyle và SpotBugs

### Kịch bản sử dụng
- Khi build Java thất bại
- Khi có xung đột Maven/Gradle dependency
- Khi có vấn đề build framework-specific

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy ./mvnw compile, ./gradlew build, dependency:tree
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- java-reviewer kiểm tra chất lượng mã
- security-reviewer xử lý vấn đề bảo mật

### Phát hiện framework
Tự động phát hiện project framework:
- Nếu chứa `quarkus` → Áp dụng quy tắc [QUARKUS]
- Nếu chứa `spring-boot` → Áp dụng quy tắc [SPRING]
- Cả hai đều có → Áp dụng cả hai bộ quy tắc

### Pattern sửa lỗi phổ biến

| Loại lỗi | Nguyên nhân | Cách sửa |
|----------|------|----------|
| cannot find symbol | Thiếu import, lỗi chính tả | Thêm import hoặc dependency |
| incompatible types | Type không khớp | Thêm explicit conversion |
| package X does not exist | Thiếu dependency | Thêm vào pom.xml/build.gradle |
| No qualifying bean of type X | Thiếu @Component hoặc component scan | Thêm annotation hoặc sửa scan package |

---

## swift-build-resolver

### Tên và mục đích
Chuyên gia giải quyết lỗi build, compile và dependency Swift/Xcode. Sửa lỗi swift build, Xcode build thất bại, vấn đề SPM dependency và code signing.

### Khả năng
- Chẩn đoán lỗi swift build/xcodebuild
- Sửa lỗi type checker và protocol conformance
- Giải quyết Swift Concurrency và Sendable issues
- Xử lý SPM dependency và version resolution failure
- Sửa Xcode project configuration và code signing issues

### Kịch bản sử dụng
- Khi build Swift thất bại
- Khi Xcode build thất bại
- Khi SPM dependency conflict

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy swift build, xcodebuild, swiftlint
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- swift-reviewer kiểm tra chất lượng mã
- security-reviewer xử lý vấn đề bảo mật

### Pattern sửa lỗi phổ biến

| Loại lỗi | Nguyên nhân | Cách sửa |
|----------|------|----------|
| cannot find type 'X' in scope | Thiếu import hoặc lỗi chính tả | Thêm import hoặc sửa tên |
| type 'X' does not conform to protocol 'Y' | Thiếu required member | Implement protocol requirement còn thiếu |
| non-sendable type passed | Sendable violation | Thêm Sendable conformance hoặc refactor |
| @MainActor function cannot be called | Main actor isolation | Thêm await hoặc sử dụng MainActor.run |

---

## dart-build-resolver

### Tên và mục đích
Chuyên gia giải quyết lỗi build, analyze và dependency Dart/Flutter. Sửa lỗi dart analyze, Flutter compile fail, pub dependency conflict và build_runner issues.

### Khả năng
- Chẩn đoán lỗi dart analyze/flutter analyze
- Sửa Dart type errors, null safety violation và missing imports
- Giải quyết pubspec.yaml dependency conflict và version constraint
- Sửa build_runner code generation failure
- Xử lý Flutter-specific build errors (Android Gradle, iOS CocoaPods, web)

### Kịch bản sử dụng
- Khi Dart/Flutter build thất bại
- Khi pub dependency conflict
- Khi build_runner generation fail

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy flutter analyze, dart analyze, flutter pub get, build_runner
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- flutter-reviewer kiểm tra chất lượng mã
- security-reviewer xử lý vấn đề bảo mật

### Pattern sửa lỗi phổ biến

| Loại lỗi | Nguyên nhân | Cách sửa |
|----------|------|----------|
| The name 'X' isn't defined | Thiếu import hoặc lỗi chính tả | Thêm import đúng |
| A value of type 'X?' can't be assigned to type 'X' | Null safety - nullable type chưa xử lý | Thêm !, ?? default hoặc null check |
| Because X depends on Y >=A and Z depends on Y <B | Pub version conflict | Điều chỉnh version constraint hoặc thêm dependency_overrides |
| build_runner: No actions were run | Code generation input không thay đổi | Sử dụng --delete-conflicting-outputs để force rebuild |

---

## django-build-resolver

### Tên và mục đích
Chuyên gia sửa vấn đề build và dependency Django/Python. Xử lý Django-specific build issues và Python dependency conflicts.

### Khả năng
- Sửa Django project configuration issues
- Giải quyết Python dependency conflicts
- Xử lý Django migration issues
- Requirements.txt/pipfile dependency management

### Kịch bản sử dụng
- Khi Django project build thất bại
- Khi Python dependency conflict
- Khi có vấn đề Django migration

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy pip, pipenv, poetry, django-admin
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- python-reviewer kiểm tra chất lượng mã
- security-reviewer xử lý vấn đề bảo mật

---

## pytorch-build-resolver

### Tên và mục đích
Chuyên gia sửa vấn đề build PyTorch/CUDA. Xử lý PyTorch-specific build issues, CUDA compatibility issues và tensor operation errors.

### Khả năng
- Chẩn đoán PyTorch build errors
- Sửa CUDA compatibility issues
- Xử lý tensor shape và device placement errors
- Sửa DataLoader và AMP failures
- PyTorch environment configuration issues

### Kịch bản sử dụng
- Khi PyTorch build thất bại
- Khi có CUDA compatibility issues
- Khi training/inference fail

### Công cụ sử dụng
- Read: Đọc nội dung file
- Write: Viết sửa chữa
- Edit: Chỉnh sửa file
- Bash: Chạy python, pip, nvcc, nvidia-smi
- Grep: Tìm kiếm pattern lỗi
- Glob: Tìm file liên quan

### Cách phối hợp với Agent khác
- mle-reviewer kiểm tra ML code
- security-reviewer xử lý vấn đề bảo mật
[Quay lại Agent index](../README.md)