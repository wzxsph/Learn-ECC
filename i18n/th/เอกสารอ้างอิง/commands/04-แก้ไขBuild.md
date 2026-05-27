# คำสั่งแก้ไขการสร้าง

เอกสารนี้แนะนำคำสั่งเฉพาะทางสำหรับแก้ไขข้อผิดพลาดในการสร้าง (build) สำหรับแต่ละภาษาใน ECC

---

## /go-build

**วัตถุประสงค์**: แก้ไขข้อผิดพลาดในการสร้าง Go ทีละขั้นตอน คำเตือน go vet และปัญหา linter

**วิธีใช้**:
```
/go-build
```

**สถานการณ์การใช้งาน**:
- `go build ./...` ล้มเหลว
- `go vet ./...` รายงานปัญหา
- `golangci-lint run` แสดงคำเตือน
- การพึ่งพาโมดูลเสียหาย
- การสร้างล้มเหลวหลังดึงการเปลี่ยนแปลง

**ขั้นตอนการทำงาน**:
1. **รันการวินิจฉัย** - รัน `go build`, `go vet`, `staticcheck`
2. **แยกวิเคราะห์ข้อผิดพลาด** - จัดกลุ่มตามไฟล์และเรียงตามระดับความรุนแรง
3. **แก้ไขทีละขั้นตอน** - ทีละข้อผิดพลาด
4. **ตรวจสอบการแก้ไข** - รันการสร้างใหม่หลังการเปลี่ยนแปลงแต่ละครั้ง
5. **สรุปรายงาน** - แสดงปัญหาที่แก้ไขและที่ยังคงมี

**การแก้ไขข้อผิดพลาดทั่วไป**:

| ข้อผิดพลาด | การแก้ไขทั่วไป |
|------|----------|
| `undefined: X` | เพิ่ม import หรือแก้ไขการสะกด |
| `cannot use X as Y` | แปลงประเภทหรือแก้ไขการกำหนดค่า |
| `missing return` | เพิ่มคำสั่ง return |
| `X does not implement Y` | เพิ่ม method ที่ขาดหาย |
| `import cycle` | ปรับโครงสร้างแพ็กเกจ |
| `declared but not used` | ลบหรือใช้ตัวแปร |
| `cannot find package` | `go get` หรือ `go mod tidy` |

**คำสั่งการวินิจฉัย**:
```bash
go build ./...                # การตรวจสอบการสร้างหลัก
go vet ./...                  # การวิเคราะห์แบบคงที่
staticcheck ./...             # lint ขั้นสูง (ถ้ามี)
golangci-lint run             # Linting
go mod verify                # การยืนยันโมดูล
go mod tidy -v               # จัดเรียงการพึ่งพา
```

---

## /kotlin-build

**วัตถุประสงค์**: แก้ไขข้อผิดพลาดในการสร้าง Kotlin/Gradle ทีละขั้นตอน คำเตือนคอมไพลเลอร์ และปัญหาการพึ่งพา

**วิธีใช้**:
```
/kotlin-build
```

**สถานการณ์การใช้งาน**:
- `./gradlew build` ล้มเหลว
- Kotlin compiler รายงานข้อผิดพลาด
- `./gradlew detekt` รายงานการละเมิด
- Gradle dependency resolution ล้มเหลว
- การสร้างล้มเหลวหลังดึงการเปลี่ยนแปลง

**การแก้ไขข้อผิดพลาดทั่วไป**:

| ข้อผิดพลาด | การแก้ไขทั่วไป |
|------|----------|
| `Unresolved reference: X` | เพิ่ม import หรือการพึ่งพา |
| `Type mismatch` | แก้ไขการแปลงประเภทหรือการกำหนดค่า |
| `'when' must be exhaustive` | เพิ่ม branch ของ sealed class ที่ขาดหาย |
| `Suspend function can only be called from coroutine` | เพิ่ม `suspend` modifier |
| `Smart cast impossible` | ใช้ `val` ในพื้นที่หรือ `let` |
| `Could not resolve dependency` | แก้ไขเวอร์ชันหรือเพิ่มที่เก็บ |

**คำสั่งการวินิจฉัย**:
```bash
./gradlew build 2>&1                      # การตรวจสอบการสร้างหลัก
./gradlew detekt 2>&1                      # การวิเคราะห์แบบคงที่ (ถ้าตั้งค่า)
./gradlew ktlintCheck 2>&1                # การตรวจสอบรูปแบบ (ถ้าตั้งค่า)
./gradlew dependencies --configuration runtimeClasspath | head -100  # ปัญหาการพึ่งพา
./gradlew build --refresh-dependencies     # รีเฟรชเชิงลึก (เมื่อสงสัย cache หรือ metadata การพึ่งพา)
```

---

## /rust-build

**วัตถุประสงค์**: แก้ไขข้อผิดพลาดในการสร้าง Rust ทีละขั้นตอน ปัญหา borrow checker และ lifetime และปัญหาการพึ่งพา

**วิธีใช้**:
```
/rust-build
```

**สถานการณ์การใช้งาน**:
- `cargo build` หรือ `cargo check` ล้มเหลว
- `cargo clippy` รายงานคำเตือน
- borrow checker หรือ lifetime errors ที่ป้องกันการคอมไพล์
- Cargo dependency resolution ล้มเหลว
- การสร้างล้มเหลวหลังดึงการเปลี่ยนแปลง

**การแก้ไขข้อผิดพลาดทั่วไป**:

| ข้อผิดพลาด | การแก้ไขทั่วไป |
|------|----------|
| `cannot borrow as mutable` | ปรับโครงสร้างเพื่อให้ immutable borrow จบก่อนที่จะเข้าถึงแบบ mutable |
| `does not live long enough` | ใช้ owning type หรือเพิ่ม lifetime annotation |
| `cannot move out of` | ปรับโครงสร้างเพื่อรับ ownership; clone เฉพาะเมื่อจำเป็นที่สุด |
| `mismatched types` | เพิ่ม `.into()`, `as` หรือการแปลงประเภทที่ชัดเจน |
| `trait X not implemented` | เพิ่ม `#[derive(Trait)]` หรือ implement ด้วยตนเอง |
| `unresolved import` | เพิ่มใน Cargo.toml หรือแก้ไข path ของ `use` |
| `cannot find value` | เพิ่ม import หรือแก้ไข path |

**คำสั่งการวินิจฉัย**:
```bash
cargo check 2>&1                               # การตรวจสอบการสร้างหลัก
cargo clippy -- -D warnings 2>&1               # Lints
cargo fmt --check 2>&1                         # การตรวจสอบรูปแบบ
cargo tree --duplicates                        # การพึ่งพาที่ซ้ำกัน
cargo audit                                   # การตรวจสอบความปลอดภัย (ถ้ามี)
```

---

## /cpp-build

**วัตถุประสงค์**: แก้ไขข้อผิดพลาดในการสร้าง C++ ทีละขั้นตอน ปัญหา CMake และปัญหา linker

**วิธีใช้**:
```
/cpp-build
```

**สถานการณ์การใช้งาน**:
- `cmake --build build` ล้มเหลว
- linker errors (undefined references, multiple definitions)
- template instantiation failures
- include/dependency issues
- การสร้างล้มเหลวหลังดึงการเปลี่ยนแปลง

**การแก้ไขข้อผิดพลาดทั่วไป**:

| ข้อผิดพลาด | การแก้ไขทั่วไป |
|------|----------|
| `undeclared identifier` | เพิ่ม `#include` หรือแก้ไขการสะกด |
| `no matching function` | แก้ไขประเภทพารามิเตอร์หรือเพิ่ม overload |
| `undefined reference` | Link library หรือเพิ่ม implementation |
| `multiple definition` | ใช้ `inline` หรือย้ายไปยัง .cpp |
| `incomplete type` | ใช้ `#include` แทน forward declaration |
| `no member named X` | แก้ไขชื่อ member หรือเพิ่ม include |
| `cannot convert X to Y` | เพิ่มการแปลงที่เหมาะสม |
| `CMake Error` | แก้ไขการตั้งค่า CMakeLists.txt |

**คำสั่งการวินิจฉัย**:
```bash
cmake -B build -S .                        # CMake configuration
cmake --build build 2>&1 | head -100       # การสร้าง
clang-tidy src/*.cpp -- -std=c++17         # การวิเคราะห์แบบคงที่ (ถ้ามี)
cppcheck --enable=all src/                 # การวิเคราะห์เพิ่มเติม (ถ้ามี)
```

---

## /gradle-build

**วัตถุประสงค์**: แก้ไขข้อผิดพลาดในการสร้าง Gradle สำหรับโปรเจกต์ Android และ Kotlin Multiplatform (KMP)

**วิธีใช้**:
```
/gradle-build
```

**สถานการณ์การใช้งาน**:
- โปรเจกต์ Android build ล้มเหลว
- KMP โปรเจกต์ compilation errors
- Gradle sync ล้มเหลว
- dependency conflicts

**การตรวจจับประเภทโปรเจกต์**:

| ตัวบ่งชี้ | คำสั่งการสร้าง |
|--------|----------|
| `build.gradle.kts` + `composeApp/` (KMP) | `./gradlew composeApp:compileKotlinMetadata` |
| `build.gradle.kts` + `app/` (Android) | `./gradlew app:compileDebugKotlin` |
| `settings.gradle.kts` พร้อมโมดูล | `./gradlew assemble` |
| ตั้งค่า detekt | `./gradlew detekt` |

**การแก้ไขข้อผิดพลาดทั่วไป**:

| ข้อผิดพลาด | การแก้ไข |
|------|------|
| Unresolved reference ใน `commonMain` | ตรวจสอบการพึ่งพาใน `commonMain.dependencies {}` |
| Expect declaration ไม่มี actual | เพิ่ม `actual` implementation ในแต่ละ platform source set |
| Compose compiler version mismatch | จัดตำแหน่ง Kotlin และ Compose compiler versions ใน `libs.versions.toml` |
| คลาสซ้ำกัน | ตรวจสอบ dependency conflicts ด้วย `./gradlew dependencies` |
| KSP errors | รัน `./gradlew kspCommonMainKotlinMetadata` เพื่อสร้างใหม่ |
| Configuration cache issues | ตรวจสอบ task inputs ที่ไม่สามารถ serialize ได้ |

---

## /flutter-build

**วัตถุประสงค์**: แก้ไขข้อผิดพลาด Dart analyzer และ Flutter build failures ทีละขั้นตอน

**วิธีใช้**:
```
/flutter-build
```

**สถานการณ์การใช้งาน**:
- `flutter analyze` รายงานข้อผิดพลาด
- `flutter build` ล้มเหลวในทุกแพลตฟอร์ม
- `flutter pub get` version conflicts
- `build_runner` code generation ล้มเหลว
- การสร้างล้มเหลวหลังดึงการเปลี่ยนแปลง

**การแก้ไขข้อผิดพลาดทั่วไป**:

| ข้อผิดพลาด | การแก้ไขทั่วไป |
|------|----------|
| `A value of type 'X?' can't be assigned to 'X'` | เพิ่ม `?? default` หรือ null protection |
| `The name 'X' isn't defined` | เพิ่ม import หรือแก้ไขการสะกด |
| `Non-nullable instance field must be initialized` | เพิ่ม initializer หรือ `late` |
| `Version solving failed` | ปรับ version constraints ใน pubspec.yaml |
| `Missing concrete implementation of 'X'` | implement interface method ที่ขาดหาย |
| `build_runner: Part of X expected` | ลบ `.g.dart` ที่ล้าสมัยแล้วสร้างใหม่ |

**คำสั่งการวินิจฉัย**:
```bash
flutter analyze 2>&1                  # การวิเคราะห์
flutter pub get 2>&1                  # การพึ่งพา
dart run build_runner build --delete-conflicting-outputs 2>&1  # การสร้างโค้ด (ถ้าใช้ build_runner)
flutter build apk 2>&1                # การสร้างแพลตฟอร์ม
flutter build web 2>&1                # การสร้าง Web
```

---

## ตารางเปรียบเทียบคำสั่งแก้ไขการสร้าง

| คำสั่ง | ภาษา/แพลตฟอร์ม | เครื่องมือหลัก | ปัญหาทั่วไป |
|------|----------|----------|----------|
| `/go-build` | Go | go build, go vet | ข้อผิดพลาดประเภท, import cycle |
| `/kotlin-build` | Kotlin | Gradle, detekt | ประเภทไม่ตรงกัน, when ไม่ครบ |
| `/rust-build` | Rust | cargo check, clippy | borrow errors, lifetimes |
| `/cpp-build` | C++ | CMake, clang-tidy | linker errors, template issues |
| `/gradle-build` | Android/KMP | Gradle | dependency conflicts, ข้อผิดพลาดการตั้งค่า |
| `/flutter-build` | Flutter/Dart | Flutter analyze | null safety, ข้อผิดพลาดการวิเคราะห์ |

---

## กลยุทธ์การแก้ไขทั่วไป

คำสั่งแก้ไขการสร้างทั้งหมดใช้กลยุทธ์พื้นฐานเดียวกัน:

1. **แก้ไขข้อผิดพลาด build ก่อน** - โค้ดต้องคอมไพล์ได้
2. **แก้ไขคำเตือน lint ต่อไป** - แก้ไขโครงสร้างที่น่าสงสัย
3. **จากนั้นแก้ไขคำเตือนรูปแบบ** - รูปแบบและแนวปฏิบัติที่ดีที่สุด
4. **แก้ไขทีละอย่าง** - ตรวจสอบการเปลี่ยนแปลงแต่ละครั้ง
5. **เปลี่ยนน้อยที่สุด** - อย่าปรับโครงสร้าง แค่แก้ไข

**เงื่อนไขการหยุด**:
ถ้าเกิดสิ่งต่อไปนี้ agent จะหยุดและรายงาน:
- ข้อผิดพลาดเดียวกันยังคงอยู่หลังจากพยายาม 3 ครั้ง
- การแก้ไขทำให้เกิดข้อผิดพลาดเพิ่มเติม
- ต้องมีการเปลี่ยนแปลงสถาปัตยกรรม
- การพึ่งพาภายนอกที่ขาดหาย