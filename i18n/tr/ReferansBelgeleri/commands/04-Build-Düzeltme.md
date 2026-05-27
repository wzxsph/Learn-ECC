# Yapı Onarımı Komutları

Bu doküman, ECC'de kullanılan çeşitli dillerde yapı hatalarını onarmak için özel komutları tanıtır.

---

## /go-build

**Kullanım Açıklaması**: Go yapı hatalarını, go vet uyarılarını ve linter sorunlarını adım adım onarır.

**Kullanım Yöntemi**:
```
/go-build
```

**Kullanım Senaryosu**:
- `go build ./...` başarısız olduğunda
- `go vet ./...` sorun bildirdiğinde
- `golangci-lint run` uyarı gösterdiğinde
- Modül bağımlılıkları bozulduğunda
- Değişiklikleri çektikten sonra yapı başarısız olduğunda

**İş Akışı**:
1. **Tanılama çalıştır** - `go build`, `go vet`, `staticcheck` çalıştır
2. **Hataları ayrıştır** - Dosyaya göre gruplandır ve şiddete sırala
3. **Adım adım onar** - Bir seferde bir hata
4. **Onarımları doğrula** - Her değişiklikten sonra yapılandırmayı yeniden çalıştır
5. **Özet raporla** - Onarılan ve kalan sorunları göster

**Yaygın Hatalar ve Düzeltmeleri**:

| Hata | Tipik Düzeltme |
|------|----------|
| `tanımsız: X` | İçe aktarma ekle veya yazımı düzelt |
| `cannot use X as Y` | Tür dönüştürme veya atama düzelt |
| `missing return` | Return ifadesi ekle |
| `X does not implement Y` | Eksik metodu ekle |
| `import cycle` | Paket yapısını yeniden düzenle |
| `declared but not used` | Değişkeni kaldır veya kullan |
| `cannot find package` | `go get` veya `go mod tidy` |

**Tanılama Komutları**:
```bash
go build ./...                # Ana yapı kontrolü
go vet ./...                  # Statik analiz
staticcheck ./...             # İleri lint (yüklüyse)
golangci-lint run             # Linting
go mod verify                # Modül doğrulama
go mod tidy -v               # Bağımlılıkları düzelt
```

---

## /kotlin-build

**Kullanım Açıklaması**: Kotlin/Gradle yapı hatalarını, derleyici uyarılarını ve bağımlılık sorunlarını adım adım onarır.

**Kullanım Yöntemi**:
```
/kotlin-build
```

**Kullanım Senaryosu**:
- `./gradlew build` başarısız olduğunda
- Kotlin derleyicisi hata bildirdiğinde
- `./gradlew detekt` ihlal bildirdiğinde
- Gradle bağımlılık çözümlemesi başarısız olduğunda
- Değişiklikleri çektikten sonra yapı başarısız olduğunda

**Yaygın Hatalar ve Düzeltmeleri**:

| Hata | Tipik Düzeltme |
|------|----------|
| `Unresolved reference: X` | İçe aktarma veya bağımlılık ekle |
| `Type mismatch` | Tür dönüştürme veya atama düzelt |
| `'when' must be exhaustive` | Eksik sealed class dalı ekle |
| `Suspend function can only be called from coroutine` | `suspend` değiştirici ekle |
| `Smart cast impossible` | Yerel `val` veya `let` kullan |
| `Could not resolve dependency` | Sürümü düzelt veya repo ekle |

**Tanılama Komutları**:
```bash
./gradlew build 2>&1                      # Ana yapı kontrolü
./gradlew detekt 2>&1                      # Statik analiz (yapılandırılmışsa)
./gradlew ktlintCheck 2>&1                # Biçim kontrolü (yapılandırılmışsa)
./gradlew dependencies --configuration runtimeClasspath | head -100  # Bağımlılık sorunları
./gradlew build --refresh-dependencies     # Derin yenileme (önbellek veya bağımlılık meta verileri şüpheliyse)
```

---

## /rust-build

**Kullanım Açıklaması**: Rust yapı hatalarını, ödünç denetleyici sorunlarını ve bağımlılık sorunlarını adım adım onarır.

**Kullanım Yöntemi**:
```
/rust-build
```

**Kullanım Senaryosu**:
- `cargo build` veya `cargo check` başarısız olduğunda
- `cargo clippy` uyarı bildirdiğinde
- Ödünç denetleyici veya yaşam döngüsü hataları derlemeyi engellediğinde
- Cargo bağımlılık çözümlemesi başarısız olduğunda
- Değişiklikleri çektikten sonra yapı başarısız olduğunda

**Yaygın Hatalar ve Düzeltmeleri**:

| Hata | Tipik Düzeltme |
|------|----------|
| `cannot borrow as mutable` | Değişken erişimden önce immutable ödünçü bitirmek için yeniden düzenle |
| `does not live long enough` | Sahipli tür kullan veya yaşam döngüsü notasyonu ekle |
| `cannot move out of` | Sahiplik almak için yeniden düzenle; son çare olarak klonla |
| `mismatched types` | `.into()`, `as` veya açık dönüştürme ekle |
| `trait X not implemented` | `#[derive(Trait)]` ekle veya manuel uygula |
| `unresolved import` | Cargo.toml'a ekle veya `use` yolunu düzelt |
| `cannot find value` | İçe aktarma ekle veya yolu düzelt |

**Tanılama Komutları**:
```bash
cargo check 2>&1                               # Ana yapı kontrolü
cargo clippy -- -D warnings 2>&1               # Lintler
cargo fmt --check 2>&1                         # Biçim kontrolü
cargo tree --duplicates                        # Çoğaltılmış bağımlılıklar
cargo audit                                   # Güvenlik denetimi (yüklüyse)
```

---

## /cpp-build

**Kullanım Açıklaması**: C++ yapı hatalarını, CMake sorunlarını ve bağlayıcı hatalarını adım adım onarır.

**Kullanım Yöntemi**:
```
/cpp-build
```

**Kullanım Senaryosu**:
- `cmake --build build` başarısız olduğunda
- Bağlayıcı hataları (tanımsız referanslar, çoklu tanımlar)
- Şablon örnek oluşturma başarısızlıkları
- İnclude/bağımlılık sorunları
- Değişiklikleri çektikten sonra yapı başarısız olduğunda

**Yaygın Hatalar ve Düzeltmeleri**:

| Hata | Tipik Düzeltme |
|------|----------|
| `undeclared identifier` | `#include` ekle veya yazımı düzelt |
| `no matching function` | Parametre türlerini düzelt veya overload ekle |
| `tanımsız reference` | Kütüphane bağla veya uygulama ekle |
| `multiple definition` | `inline` kullan veya .cpp'ye taşı |
| `incomplete type` | Forward declare yerine `#include` kullan |
| `no member named X` | Üye adını düzelt veya include ekle |
| `cannot convert X to Y` | Uygun dönüştürme ekle |
| `CMake Error` | CMakeLists.txt yapılandırmasını düzelt |

**Tanılama Komutları**:
```bash
cmake -B build -S .                        # CMake yapılandırması
cmake --build build 2>&1 | head -100       # Yapı
clang-tidy src/*.cpp -- -std=c++17         # Statik analiz (yüklüyse)
cppcheck --enable=all src/                 # Ek analiz (yüklüyse)
```

---

## /gradle-build

**Kullanım Açıklaması**: Android ve Kotlin Multiplatform (KMP) projeleri için Gradle yapı hatalarını onarır.

**Kullanım Yöntemi**:
```
/gradle-build
```

**Kullanım Senaryosu**:
- Android proje yapısı başarısız olduğunda
- KMP projesi derleme hataları
- Gradle senkronizasyonu başarısız olduğunda
- Bağımlılık çatışmaları

**Proje Türü Algılama**:

| Gösterge | Yapı Komutu |
|--------|----------|
| `build.gradle.kts` + `composeApp/` (KMP) | `./gradlew composeApp:compileKotlinMetadata` |
| `build.gradle.kts` + `app/` (Android) | `./gradlew app:compileDebugKotlin` |
| `settings.gradle.kts` ile modüller | `./gradlew assemble` |
| detekt yapılandırılmış | `./gradlew detekt` |

**Yaygın Hatalar ve Düzeltmeleri**:

| Hata | Düzeltme |
|------|----------|
| `commonMain` içinde çözümlenmemiş referanslar | Bağımlılıkların `commonMain.dependencies {}` içinde olup olmadığını kontrol et |
| Expect deklarasyonu için actual yok | Her platform kaynak kümesine `actual` uygulama ekle |
| Compose derleyici sürümü uyumsuzluğu | `libs.versions.toml` içinde Kotlin ve Compose derleyici sürümlerini hizala |
| Yinelenen sınıflar | `./gradlew dependencies` ile çatışan bağımlılıkları kontrol et |
| KSP hataları | Yeniden oluşturmak için `./gradlew kspCommonMainKotlinMetadata` çalıştır |
| Yapılandırma önbellek sorunları | Seri hale getirilebilir olmayan görev girdilerini kontrol et |

---

## /flutter-build

**Kullanım Açıklaması**: Dart analiz hatalarını ve Flutter yapı başarısızlıklarını adım adım onarır.

**Kullanım Yöntemi**:
```
/flutter-build
```

**Kullanım Senaryosu**:
- `flutter analyze` hata bildirdiğinde
- `flutter build` herhangi bir platformda başarısız olduğunda
- `flutter pub get` sürüm çatışmaları
- `build_runner` kod üretme başarısız olduğunda
- Değişiklikleri çektikten sonra yapı başarısız olduğunda

**Yaygın Hatalar ve Düzeltmeleri**:

| Hata | Tipik Düzeltme |
|------|----------|
| `A value of type 'X?' can't be assigned to 'X'` | `?? default` veya null koruması ekle |
| `The name 'X' isn't defined` | İçe aktarma ekle veya yazımı düzelt |
| `Non-nullable instance field must be initialized` | Başlatıcı veya `late` ekle |
| `Version solving failed` | pubspec.yaml içindeki sürüm kısıtlamalarını ayarla |
| `Missing concrete implementation of 'X'` | Eksik arayüz metodlarını uygula |
| `build_runner: Part of X expected` | Güncel olmayan `.g.dart` dosyalarını sil ve yeniden oluştur |

**Tanılama Komutları**:
```bash
flutter analyze 2>&1                  # Analiz
flutter pub get 2>&1                  # Bağımlılıklar
dart run build_runner build --delete-conflicting-outputs 2>&1  # Kod üretme (build_runner kullanıyorsa)
flutter build apk 2>&1                # Platform yapısı
flutter build web 2>&1                # Web yapısı
```

---

## Yapı Onarımı Komutları Karşılaştırma Tablosu

| Komut | Dil/Platform | Ana Araçlar | Yaygın Sorunlar |
|------|----------|----------|----------|
| `/go-build` | Go | go build, go vet | Tür hataları, içe aktarma döngüleri |
| `/kotlin-build` | Kotlin | Gradle, detekt | Tür uyumsuzluğu, when nonexhaustive |
| `/rust-build` | Rust | cargo check, clippy | Ödünç hataları, yaşam döngüleri |
| `/cpp-build` | C++ | CMake, clang-tidy | Bağlayıcı hataları, şablon sorunları |
| `/gradle-build` | Android/KMP | Gradle | Bağımlılık çatışmaları, yapılandırma hataları |
| `/flutter-build` | Flutter/Dart | Flutter analyze | Null güvenliği, analiz hataları |

---

## Genel Onarım Stratejisi

Tüm yapı onarımı komutları aynı temel stratejiyi izler:

1. **Önce yapı hatalarını onar** - Kod derlenmelidir
2. **Sonra lint uyarılarını onar** - Şüpheli yapıları düzelt
3. **Sonra biçim uyarılarını onar** - Stil ve en iyi uygulamalar
4. **Bir seferde birini onar** - Her değişikliği doğrula
5. **Minimum değişiklik yap** - Yeniden düzenleme, sadece onarım yapma

**Durma Koşulları**:
Aşağıdakilerden herhangi biri gerçekleşirse, ajan durur ve raporlar:
- Aynı hata 3 denemeden sonra hala mevcut
- Düzeltme daha fazla hata getiriyor
- Gerekli mimari değişiklikler
- Eksik harici bağımlılıklar
