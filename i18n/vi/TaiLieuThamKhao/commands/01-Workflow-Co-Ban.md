# Lệnh Sửa Lỗi Build

Tài liệu này giới thiệu các lệnh chuyên dụng trong ECC để sửa lỗi build cho các ngôn ngữ khác nhau.

---

## /go-build

**Mục đích**: Từng bước sửa lỗi build Go, cảnh báo go vet và vấn đề linter.

**Cách sử dụng**:
```
/go-build
```

**Khi nào sử dụng**:
- `go build ./...` thất bại
- `go vet ./...` báo vấn đề
- `golangci-lint run` hiển thị cảnh báo
- Phụ thuộc module bị hỏng
- Build thất bại sau khi kéo thay đổi

**Quy trình làm việc**:
1. **Chạy chẩn đoán** - Thực thi `go build`, `go vet`, `staticcheck`
2. **Phân tích lỗi** - Nhóm theo file và sắp xếp theo mức độ nghiêm trọng
3. **Sửa từng bước** - Mỗi lần một lỗi
4. **Xác minh sửa** - Chạy lại build sau mỗi thay đổi
5. **Báo cáo tổng kết** - Hiển thị đã sửa và vấn đề còn lại

**Sửa lỗi thường gặp**:

| Lỗi | Sửa lỗi điển hình |
|------|----------|
| `undefined: X` | Thêm import hoặc sửa lỗi chính tả |
| `cannot use X as Y` | Ép kiểu hoặc sửa gán |
| `missing return` | Thêm câu lệnh return |
| `X does not implement Y` | Thêm method còn thiếu |
| `import cycle` | Tái cấu trúc cấu trúc package |
| `declared but not used` | Xóa hoặc sử dụng biến |
| `cannot find package` | `go get` hoặc `go mod tidy` |

**Lệnh chẩn đoán**:
```bash
go build ./...                # Build chính
go vet ./...                  # Phân tích tĩnh
staticcheck ./...             # Lint nâng cao (nếu có)
golangci-lint run             # Linting
go mod verify                # Xác minh module
go mod tidy -v               # Dọn dẹp phụ thuộc
```

---

## /kotlin-build

**Mục đích**: Từng bước sửa lỗi build Kotlin/Gradle, cảnh báo trình biên dịch và vấn đề phụ thuộc.

**Cách sử dụng**:
```
/kotlin-build
```

**Khi nào sử dụng**:
- `./gradlew build` thất bại
- Trình biên dịch Kotlin báo lỗi
- `./gradlew detekt` báo vi phạm
- Gradle dependency resolution thất bại
- Build thất bại sau khi kéo thay đổi

**Sửa lỗi thường gặp**:

| Lỗi | Sửa lỗi điển hình |
|------|----------|
| `Unresolved reference: X` | Thêm import hoặc dependency |
| `Type mismatch` | Sửa ép kiểu hoặc gán |
| `'when' must be exhaustive` | Thêm branch sealed class còn thiếu |
| `Suspend function can only be called from coroutine` | Thêm modifier `suspend` |
| `Smart cast impossible` | Dùng `val` cục bộ hoặc `let` |
| `Could not resolve dependency` | Sửa version hoặc thêm repository |

**Lệnh chẩn đoán**:
```bash
./gradlew build 2>&1                      # Build chính
./gradlew detekt 2>&1                      # Phân tích tĩnh (nếu cấu hình)
./gradlew ktlintCheck 2>&1                # Kiểm tra format (nếu cấu hình)
./gradlew dependencies --configuration runtimeClasspath | head -100  # Vấn đề phụ thuộc
./gradlew build --refresh-dependencies     # Refresh sâu (khi cache hoặc metadata đáng ngờ)
```

---

## /rust-build

**Mục đích**: Từng bước sửa lỗi build Rust, vấn đề borrow checker và lifecycle.

**Cách sử dụng**:
```
/rust-build
```

**Khi nào sử dụng**:
- `cargo build` hoặc `cargo check` thất bại
- `cargo clippy` báo cảnh báo
- Borrow checker hoặc lỗi lifecycle ngăn compile
- Cargo dependency resolution thất bại
- Build thất bại sau khi kéo thay đổi

**Sửa lỗi thường gặp**:

| Lỗi | Sửa lỗi điển hình |
|------|----------|
| `cannot borrow as mutable` | Tái cấu trúc để immutable borrow kết thúc trước mutable access |
| `does not live long enough` | Dùng owned type hoặc thêm annotation lifecycle |
| `cannot move out of` | Tái cấu trúc để lấy ownership; clone chỉ là phương án cuối cùng |
| `mismatched types` | Thêm `.into()`, `as` hoặc conversion rõ ràng |
| `trait X not implemented` | Thêm `#[derive(Trait)]` hoặc implement thủ công |
| `unresolved import` | Thêm vào Cargo.toml hoặc sửa đường dẫn `use` |
| `cannot find value` | Thêm import hoặc sửa đường dẫn |

**Lệnh chẩn đoán**:
```bash
cargo check 2>&1                               # Build chính
cargo clippy -- -D warnings 2>&1               # Lints
cargo fmt --check 2>&1                         # Kiểm tra format
cargo tree --duplicates                        # Phụ thuộc trùng lặp
cargo audit                                   # Audit bảo mật (nếu có)
```

---

## /cpp-build

**Mục đích**: Từng bước sửa lỗi build C++, vấn đề CMake và linker.

**Cách sử dụng**:
```
/cpp-build
```

**Khi nào sử dụng**:
- `cmake --build build` thất bại
- Lỗi linker (undefined reference, multiple definition)
- Template instantiation thất bại
- Vấn đề include/phụ thuộc
- Build thất bại sau khi kéo thay đổi

**Sửa lỗi thường gặp**:

| Lỗi | Sửa lỗi điển hình |
|------|----------|
| `undeclared identifier` | Thêm `#include` hoặc sửa lỗi chính tả |
| `no matching function` | Sửa kiểu tham số hoặc thêm overload |
| `undefined reference` | Link library hoặc thêm implementation |
| `multiple definition` | Dùng `inline` hoặc chuyển vào .cpp |
| `incomplete type` | Thay forward declaration bằng `#include` |
| `no member named X` | Sửa tên member hoặc thêm include |
| `cannot convert X to Y` | Thêm conversion phù hợp |
| `CMake Error` | Sửa cấu hình CMakeLists.txt |

**Lệnh chẩn đoán**:
```bash
cmake -B build -S .                        # Cấu hình CMake
cmake --build build 2>&1 | head -100       # Build
clang-tidy src/*.cpp -- -std=c++17         # Phân tích tĩnh (nếu có)
cppcheck --enable=all src/                 # Phân tích thêm (nếu có)
```

---

## /gradle-build

**Mục đích**: Sửa lỗi build Gradle cho Android và Kotlin Multiplatform (KMP).

**Cách sử dụng**:
```
/gradle-build
```

**Khi nào sử dụng**:
- Build Android thất bại
- Lỗi compile KMP
- Gradle sync thất bại
- Xung đột phụ thuộc

**Phát hiện loại project**:

| Indicator | Lệnh build |
|--------|----------|
| `build.gradle.kts` + `composeApp/` (KMP) | `./gradlew composeApp:compileKotlinMetadata` |
| `build.gradle.kts` + `app/` (Android) | `./gradlew app:compileDebugKotlin` |
| `settings.gradle.kts` với modules | `./gradlew assemble` |
| Có detekt cấu hình | `./gradlew detekt` |

**Sửa lỗi thường gặp**:

| Lỗi | Sửa |
|------|------|
| Unresolved reference trong `commonMain` | Kiểm tra dependency có trong `commonMain.dependencies {}` |
| Expect declaration không có actual | Thêm `actual` implementation trong mỗi platform source set |
| Compose compiler version không khớp | Căn chỉnh Kotlin và Compose compiler version trong `libs.versions.toml` |
| Duplicate class | Kiểm tra dependency xung đột với `./gradlew dependencies` |
| Lỗi KSP | Chạy `./gradlew kspCommonMainKotlinMetadata` để regenerate |
| Vấn đề configuration cache | Kiểm tra task input không serializable |

---

## /flutter-build

**Mục đích**: Từng bước sửa lỗi Dart analyzer và Flutter build.

**Cách sử dụng**:
```
/flutter-build
```

**Khi nào sử dụng**:
- `flutter analyze` báo lỗi
- `flutter build` thất bại trên bất kỳ platform nào
- `flutter pub get` xung đột version
- `build_runner` tạo code thất bại
- Build thất bại sau khi kéo thay đổi

**Sửa lỗi thường gặp**:

| Lỗi | Sửa lỗi điển hình |
|------|----------|
| `A value of type 'X?' can't be assigned to 'X'` | Thêm `?? default` hoặc null guard |
| `The name 'X' isn't defined` | Thêm import hoặc sửa lỗi chính tả |
| `Non-nullable instance field must be initialized` | Thêm initializer hoặc `late` |
| `Version solving failed` | Điều chỉnh version constraint trong pubspec.yaml |
| `Missing concrete implementation of 'X'` | Implement interface method còn thiếu |
| `build_runner: Part of X expected` | Xóa `.g.dart` cũ và build lại |

**Lệnh chẩn đoán**:
```bash
flutter analyze 2>&1                  # Phân tích
flutter pub get 2>&1                  # Phụ thuộc
dart run build_runner build --delete-conflicting-outputs 2>&1  # Tạo code (nếu dùng build_runner)
flutter build apk 2>&1                # Build platform
flutter build web 2>&1                # Build web
```

---

## Bảng so sánh lệnh sửa build

| Lệnh | Ngôn ngữ/Platform | Tool chính | Vấn đề thường gặp |
|------|----------|----------|----------|
| `/go-build` | Go | go build, go vet | Lỗi kiểu, import cycle |
| `/kotlin-build` | Kotlin | Gradle, detekt | Kiểu không khớp, when không đầy đủ |
| `/rust-build` | Rust | cargo check, clippy | Lỗi borrow, lifecycle |
| `/cpp-build` | C++ | CMake, clang-tidy | Lỗi linker, vấn đề template |
| `/gradle-build` | Android/KMP | Gradle | Xung đột phụ thuộc, lỗi cấu hình |
| `/flutter-build` | Flutter/Dart | Flutter analyze | Null safety, lỗi phân tích |

---

## Chiến lược sửa chung

Tất cả các lệnh sửa build đều tuân theo cùng một chiến lược cơ bản:

1. **Sửa lỗi build trước** - Code phải compile được
2. **Sau đó sửa cảnh báo lint** - Sửa các cấu trúc đáng ngờ
3. **Rồi sửa cảnh báo format** - Style và best practice
4. **Mỗi lần sửa một lỗi** - Xác minh mỗi thay đổi
5. **Thay đổi tối thiểu** - Đừng refactor, chỉ sửa

**Điều kiện dừng**:
Nếu xảy ra những điều sau, agent sẽ dừng lại và báo cáo:
- Cùng một lỗi vẫn tồn tại sau 3 lần thử
- Sửa đã tạo thêm lỗi
- Cần thay đổi kiến trúc
- Thiếu phụ thuộc bên ngoài