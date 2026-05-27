# Yapı Onarımı Ajanı

Yapı Onarımı Ajanı, çeşitli Programlama Dillerinde yapı hataları, derleme hataları ve bağımlılık sorunlarını tanılamak ve onarmaktan sorumludur.

## Ajan Listesi

| Ajan Adı | Kullanım | Kullanım Modeli | Temel Araçlar |
|------------|------|----------|----------|
| build-error-resolver | TypeScript/Genel yapı hatası düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| go-build-resolver | Go yapı/derleme hatası düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| kotlin-build-resolver | Kotlin/Gradle yapı hatası düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| rust-build-resolver | Rust yapı/ödünç denetleyici hatası düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| cpp-build-resolver | C++/CMake yapı hatası düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| java-build-resolver | Java/Maven/Gradle yapı hatası düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| swift-build-resolver | Swift/Xcode yapı hatası düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| dart-build-resolver | Dart/Flutter yapı hatası düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| django-build-resolver | Django/Python yapı sorunu düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| pytorch-build-resolver | PyTorch/CUDA yapı sorunu düzeltme | sonnet | Read, Write, Edit, Bash, Grep, Glob |

---

## build-error-resolver

### Ad ve Kullanım
TypeScript yapı ve tür hatası çözümleme uzmanı. Yapı hatalarını veya tür hatalarını onarır, yapıyı yeşile getirmeye odaklanır, mimari değişiklik yapmaz.

### Yetenekler
- TypeScript hata çözümleme
- Yapı hatası düzeltme
- Bağımlılık sorunu düzeltme
- Yapılandırma hatası çözümleme
- Minimal diff düzeltme

### Kullanım Senaryosu
- Yapı başarısız olduğunda
- TypeScript tür hatası oluştuğunda
- Modül çözümleme sorunlarında

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: tsc, npm build, eslint çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- Yeniden düzenleme yapma → refactor-cleaner kullan
- Mimari değişiklik yapma → architect kullan
- Yeni özellik eklememe → planner kullan
- Test onarmama → tdd-guide kullan
- Güvenlik sorunları işlememe → security-reviewer kullan

### Temel İlkeler
- Yalnızca hataları düzelt, yeniden düzenleme yapma
- Minimal değişiklik yap
- Her düzeltmeden sonra yapının geçtiğini doğrula

---

## go-build-resolver

### Ad ve Kullanım
Go yapı, vet ve derleme hatası çözümleme uzmanı. Go yapı hatalarını, go vet sorunlarını ve linter uyarılarını onarır.

### Yetenekler
- Go derleme hatası tanılama
- go vet uyarı düzeltme
- staticcheck/golangci-lint sorun düzeltme
- Modül bağımlılık sorunu çözümleme
- Tür hataları ve arayüz uyuşmazlıkları işleme

### Kullanım Senaryosu
- Go yapısı başarısız olduğunda
- go vet hata verdiğinde
- Modül bağımlılık çatışmalarında

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: go build, go vet, staticcheck, golangci-lint çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- go-reviewer kod kalitesini inceler
- security-reviewer güvenlikle ilgili sorunları işler

### Yaygın Hata Düzeltme Desenleri

| Hata Türü | Neden | Düzeltme Yöntemi |
|----------|------|----------|
| tanımsız: X | Eksik import, yazım hatası | Import ekle veya büyük/küçük harf düzelt |
| cannot use X as type Y | Tür uyuşmazlığı | Tür dönüştürme veya dereference et |
| X does not implement Y | Eksik metod | Doğru receiver ile metodu uygula |
| import cycle not allowed | Döngüsel bağımlılık | Paylaşılan türü yeni pakete çıkar |
| cannot find package | Eksik bağımlılık | go get pkg@version veya go mod tidy |

---

## kotlin-build-resolver

### Ad ve Kullanım
Kotlin/Gradle yapı, derleme ve bağımlılık hatası çözümleme uzmanı. Kotlin yapı hatalarını, Kotlin derleyici hatalarını ve Gradle sorunlarını onarır.

### Yetenekler
- Kotlin derleme hatası tanılama
- Gradle yapılandırma sorunu düzeltme
- Bağımlılık çatışmaları ve sürüm uyuşmazlıkları çözümleme
- Kotlin derleyici hata yönetimi
- detekt ve ktlint ihlal düzeltmeleri

### Kullanım Senaryosu
- Kotlin yapısı başarısız olduğunda
- Gradle bağımlılık çatışmalarında
- Kotlin derleyici hata verdiğinde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: ./gradlew build, detekt, ktlintCheck çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- kotlin-reviewer kod kalitesini inceler
- security-reviewer güvenlikle ilgili sorunları işler

### Yaygın Hata Düzeltme Desenleri

| Hata Türü | Neden | Düzeltme Yöntemi |
|----------|------|----------|
| Unresolved reference: X | Eksik import, yazım hatası | Import veya bağımlılık ekle |
| Type mismatch | Tür hatası | Dönüştürme ekle veya türü düzelt |
| Smart cast impossible | Değişken özellik veya eşzamanlı erişim | Yerel val kopyası kullan |
| when expression must be exhaustive | sealed class when dalı eksik | Eksik dal veya else ekle |
| Suspend function can only be called from coroutine | suspend veya korutin scope eksik | suspend değiştiricisi ekle |

---

## rust-build-resolver

### Ad ve Kullanım
Rust yapı, derleme ve bağımlılık hatası çözümleme uzmanı. Cargo yapı hatalarını, ödünç denetleyici sorunlarını ve Cargo.toml sorunlarını onarır.

### Yetenekler
- cargo build/check hata tanılama
- Ödünç denetleyici ve yaşam döngüsü hatası düzeltme
- Trait uygulama uyuşmazlıkları çözümleme
- Cargo bağımlılık ve trait sorunları işleme
- cargo clippy uyarı düzeltme

### Kullanım Senaryosu
- Rust yapısı başarısız olduğunda
- Ödünç denetleyici hata verdiğinde
- Cargo bağımlılık çatışmalarında

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: cargo check, clippy, fmt, tree çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- rust-reviewer kod kalitesini inceler
- security-reviewer güvenlikle ilgili sorunları işler

### Yaygın Hata Düzeltme Desenleri

| Hata Türü | Neden | Düzeltme Yöntemi |
|----------|------|----------|
| cannot borrow as mutable | İmmutabl ödünç etkin | Önce immutabl ödünçü bitirmek için yeniden düzenle |
| does not live long enough | Değer hala ödünç alınmışken düşüyor | Yaşam döngüsü kapsamını genişlet |
| cannot move out of | Referanstan sonra taşıma | .clone() kullan veya yeniden düzenle |
| mismatched types | Tür hatası | .into(), as veya açık dönüştürme ekle |
| trait X is not implemented for Y | Eksik impl veya derive | #[derive(Trait)] ekle |

---

## cpp-build-resolver

### Ad ve Kullanım
C++ yapı, CMake ve derleme hatası çözümleme uzmanı. Yapı hatalarını, bağlayıcı sorunlarını ve şablon hatalarını onarır.

### Yetenekler
- C++ derleme hatası tanılama
- CMake yapılandırma sorunu düzeltme
- Bağlayıcı hatası çözümleme (tanımsız referanslar, çoğaltılmış tanımlar)
- Şablon örnek oluşturma hata yönetimi
- İnclude ve bağımlılık sorunları düzeltme

### Kullanım Senaryosu
- C++ yapısı başarısız olduğunda
- CMake yapılandırma hatası olduğunda
- Bağlayıcı hata verdiğinde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: cmake --build, clang-tidy, cppcheck çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- cpp-reviewer kod kalitesini inceler
- security-reviewer güvenlikle ilgili sorunları işler

### Yaygın Hata Düzeltme Desenleri

| Hata Türü | Neden | Düzeltme Yöntemi |
|----------|------|----------|
| tanımsız referans to X | Eksik uygulama veya kütüphane | Kaynak dosya ekle veya kütüphane bağla |
| no matching function for call | Parametre türü hatası | Türü düzelt veya overload ekle |
| multiple definition of | Çoğaltılmış sembol | inline kullan veya .cpp'ye taşı |
| cannot convert X to Y | Tür uyuşmazlığı | Dönüştürme ekle veya türü düzelt |
| template argument deduction failed | Şablon parametre hatası | Şablon parametresini düzelt |

---

## java-build-resolver

### Ad ve Kullanım
Java/Maven/Gradle yapı, derleme ve bağımlılık hatası çözümleme uzmanı. Spring Boot veya Quarkus'u otomatik olarak algılar ve çerçeve özgü düzeltmeleri uygular.

### Yetenekler
- Java derleme hatası tanılama
- Maven ve Gradle yapılandırma sorunu düzeltme
- Bağımlılık çatışmaları ve sürüm uyuşmazlıkları çözümleme
- Annotasyon işlemcisi hata yönetimi (Lombok, MapStruct, Spring, Quarkus)
- Checkstyle ve SpotBugs ihlal düzeltmeleri

### Kullanım Senaryosu
- Java yapısı başarısız olduğunda
- Maven/Gradle bağımlılık çatışmalarında
- Çerçeve özgü yapı sorunlarında

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: ./mvnw compile, ./gradlew build, dependency:tree çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- java-reviewer kod kalitesini inceler
- security-reviewer güvenlikle ilgili sorunları işler

### Çerçeve Algılama
Proje çerçevelerini otomatik algıla:
- `quarkus` içeriyorsa → [QUARKUS] kuralları uygula
- `spring-boot` içeriyorsa → [SPRING] kuralları uygula
- Her ikisi de varsa → Her iki kural setini uygula

### Yaygın Hata Düzeltme Desenleri

| Hata Türü | Neden | Düzeltme Yöntemi |
|----------|------|----------|
| cannot find symbol | Eksik import, yazım hatası | Import veya bağımlılık ekle |
| incompatible types | Tür uyuşmazlığı | Açık dönüştürme ekle |
| package X does not exist | Eksik bağımlılık | pom.xml/build.gradle'a ekle |
| No qualifying bean of type X | Eksik @Component veya bileşen tarama | Annotasyon ekle veya tarama paketini düzelt |

---

## swift-build-resolver

### Ad ve Kullanım
Swift/Xcode yapı, derleme ve bağımlılık hatası çözümleme uzmanı. Swift yapı hatalarını, Xcode yapı başarısızlıklarını, SPM bağımlılık sorunlarını ve kod imzalama sorunlarını onarır.

### Yetenekler
- swift build/xcodebuild hata tanılama
- Tür denetleyici ve protokol uygunluğu hatası düzeltme
- Swift Concurrency ve Sendable sorunları çözümleme
- SPM bağımlılık ve sürüm çözümleme başarısızlıkları işleme
- Xcode proje yapılandırma ve kod imzalama sorunları düzeltme

### Kullanım Senaryosu
- Swift yapısı başarısız olduğunda
- Xcode yapısı başarısız olduğunda
- SPM bağımlılık çatışmalarında

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: swift build, xcodebuild, swiftlint çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- swift-reviewer kod kalitesini inceler
- security-reviewer güvenlikle ilgili sorunları işler

### Yaygın Hata Düzeltme Desenleri

| Hata Türü | Neden | Düzeltme Yöntemi |
|----------|------|----------|
| cannot find type 'X' in scope | Eksik import veya yazım hatası | Import ekle veya adı düzelt |
| type 'X' does not conform to protocol 'Y' | Eksik gerekli üyeler | Eksik protokol gereksinimlerini uygula |
| non-sendable type passed | Sendable ihlali | Sendable uygunluğu ekle veya yeniden düzenle |
| @MainActor function cannot be called | Main actor izolasyonu | await ekle veya MainActor.run kullan |

---

## dart-build-resolver

### Ad ve Kullanım
Dart/Flutter yapı, analiz ve bağımlılık hatası çözümleme uzmanı. Dart analiz hatalarını, Flutter derleme başarısızlıklarını, pub bağımlılık çatışmalarını ve build_runner sorunlarını onarır.

### Yetenekler
- dart analyze/flutter analyze hata tanılama
- Dart tür hataları, null güvenliği ihlalleri ve eksik import düzeltme
- pubspec.yaml bağımlılık çatışmaları ve sürüm kısıtlamaları çözümleme
- build_runner kod üretme başarısızlık düzeltme
- Flutter özgü yapı hata yönetimi (Android Gradle, iOS CocoaPods, web)

### Kullanım Senaryosu
- Dart/Flutter yapısı başarısız olduğunda
- Pub bağımlılık çatışmalarında
- build_runner üretme başarısız olduğunda

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: flutter analyze, dart analyze, flutter pub get, build_runner çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- flutter-reviewer kod kalitesini inceler
- security-reviewer güvenlikle ilgili sorunları işler

### Yaygın Hata Düzeltme Desenleri

| Hata Türü | Neden | Düzeltme Yöntemi |
|----------|------|----------|
| The name 'X' isn't defined | Eksik import veya yazım hatası | Doğru import ekle |
| A value of type 'X?' can't be assigned to type 'X' | Null güvenliği - nullable tür işlenmemiş | !, ?? default veya null kontrolü ekle |
| Because X depends on Y >=A and Z depends on Y <B | Pub sürüm çatışması | Sürüm kısıtlamalarını ayarla veya dependency_overrides ekle |
| build_runner: No actions were run | Kod üretme girişi değişmedi | --delete-conflicting-outputs ile yeniden oluşturmaya zorla |

---

## django-build-resolver

### Ad ve Kullanım
Django/Python yapı ve bağımlılık sorunu düzeltme uzmanı. Django özgü yapı sorunlarını ve Python bağımlılık çatışmalarını işler.

### Yetenekler
- Django proje yapılandırma sorunu düzeltme
- Python bağımlılık çatışması çözümü
- Django migrasyon sorunu işleme
- requirements.txt/pipfile bağımlılık yönetimi

### Kullanım Senaryosu
- Django proje yapısı başarısız olduğunda
- Python bağımlılık çatışmalarında
- Django migrasyon sorunlarında

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: pip, pipenv, poetry, django-admin çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- python-reviewer kod kalitesini inceler
- security-reviewer güvenlikle ilgili sorunları işler

---

## pytorch-build-resolver

### Ad ve Kullanım
PyTorch/CUDA yapı sorunu düzeltme uzmanı. PyTorch özgü yapı sorunlarını, CUDA uyumluluk sorunlarını ve tensör işlem hatalarını işler.

### Yetenekler
- PyTorch yapı hatası tanılama
- CUDA uyumluluk sorunu düzeltme
- Tensör şekli ve cihaz yerleştirme hata yönetimi
- DataLoader ve AMP başarısızlık düzeltme
- PyTorch ortam yapılandırma sorunları

### Kullanım Senaryosu
- PyTorch yapısı başarısız olduğunda
- CUDA uyumluluk sorunu olduğunda
- Eğitim/çıkarım başarısız olduğunda

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Write: Düzeltme yaz
- Edit: Dosyayı düzenle
- Bash: python, pip, nvcc, nvidia-smi çalıştır
- Grep: Hata desenlerini ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- mle-reviewer ML kodunu inceler
- security-reviewer güvenlikle ilgili sorunları işler
[Geri Dön Ajan Dizini](../README.md)
