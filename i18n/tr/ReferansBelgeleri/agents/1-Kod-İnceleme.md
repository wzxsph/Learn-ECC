# Kod İnceleme Ajanı

Kod İnceleme Ajanı, kod kalitesi, güvenlik ve bakım kolaylığının incelenmesinden sorumludur.

## Ajan Listesi

| Ajan Adı | Kullanım | Kullanım Modeli | Temel Araçlar |
|------------|------|----------|----------|
| code-reviewer | Genel Kod İnceleme uzmanı | sonnet | Read, Grep, Glob, Bash |
| python-reviewer | Python Kod İnceleme (PEP 8, Tür İpuçları, Güvenlik) | sonnet | Read, Grep, Glob, Bash |
| go-reviewer | Go Kod İnceleme (Eşzamanlılık, Hata Yönetimi, Performans) | sonnet | Read, Grep, Glob, Bash |
| kotlin-reviewer | Kotlin/Android Kod İnceleme (Korutinler, Compose) | sonnet | Read, Grep, Glob, Bash |
| rust-reviewer | Rust Kod İnceleme (Sahiplik, Yaşam Döngüsü, Güvenlik) | sonnet | Read, Grep, Glob, Bash |
| cpp-reviewer | C++ Kod İnceleme (Bellek Güvenliği, Modern C++ İdiomları) | sonnet | Read, Grep, Glob, Bash |
| flutter-reviewer | Flutter/Dart Kod İnceleme (Durum Yönetimi, Performans) | sonnet | Read, Grep, Glob, Bash |
| typescript-reviewer | TypeScript/JavaScript Kod İnceleme (Tür Güvenliği) | sonnet | Read, Grep, Glob, Bash |
| swift-reviewer | Swift Kod İnceleme (Protokoller, Eşzamanlılık, ARC Bellek Yönetimi) | sonnet | Read, Grep, Glob, Bash |
| csharp-reviewer | C# Kod İnceleme (.NET Kuralları, Asenkron Desenler) | sonnet | Read, Grep, Glob, Bash |
| fsharp-reviewer | F# Kod İnceleme (Fonksiyonel İdiomlar, Desen Eşleştirme) | sonnet | Read, Grep, Glob, Bash |
| java-reviewer | Java Kod İnceleme (Spring Boot/Quarkus Çerçeveleri) | sonnet | Read, Grep, Glob, Bash |

---

## code-reviewer

### Ad ve Kullanım
Genel Kod İnceleme uzmanı, kodun kalitesini, güvenliğini ve bakım kolaylığını proaktif olarak inceler. Tüm kod değişikliklerinden sonra kullanılması zorunludur.

### Yetenekler
- Kod kalitesi kontrolü (fonksiyon boyutu, iç içe geçme derinliği, kod tekrarı)
- Güvenlik açıkları tespiti (SQL Enjeksiyonu, XSS, Sabit Kodlu Kimlik Bilgileri)
- React/Next.js desen kontrolü
- Node.js/backend desen kontrolü
- Performans sorunu tespiti
- En iyi uygulama önerileri

### Kullanım Senaryosu
- Kod yazıldıktan veya değiştirildikten sonra
- Pull Request incelemelerinde
- Paylaşılan dallara birleştirmeden önce

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: Dosyaları bul
- Bash: git diff ve lint komutlarını çalıştır

### Diğer Ajanlarla İşbirliği
- Güvenlik sorunları tespit edildiğinde security-reviewer'a yükselt
- Yapı hataları tespit edildiğinde build-error-resolver kullan
- Yeniden düzenleme gerekli görüldüğünde refactor-cleaner kullan

---

## python-reviewer

### Ad ve Kullanım
Python Kod İnceleme uzmanı, PEP 8 uyumluluğu, Pythonic idiomlar, tür ipuçları, güvenlik ve performans konularında uzmanlaşmıştır.

### Yetenekler
- PEP 8 stil kontrolü
- Pythonic idiomların tanıtımı (liste kavramaları, enumlar, with ifadeleri)
- Tür İpucu doğrulama
- SQL Enjeksiyonu tespiti
- Korutin ve asenkron desen kontrolü
- Çerçeveye özgü kontroller (Django, FastAPI, Flask)

### Kullanım Senaryosu
- Python projelerindeki tüm kod değişikliklerinde
- Django/FastAPI/Flask özgü projelerde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: Python dosyalarını bul
- Bash: mypy, ruff, black, bandit, pytest çalıştır

### Diğer Ajanlarla İşbirliği
- Güvenlik kontrol kuralları security-reviewer ile paylaşılır
- Test kapsamını sağlamak için tdd-guide kullanılır

---

## go-reviewer

### Ad ve Kullanım
Go Kod İnceleme uzmanı, idiomatic Go, eşzamanlılık desenleri, hata yönetimi ve performans konularında uzmanlaşmıştır.

### Yetenekler
- Go idiom kontrolü (erken dönüş, hata sarmalama)
- Eşzamanlılık güvenliği kontrolü (goroutine sızıntıları, kanal kilitlenmeleri)
- Hata yönetimi en iyi uygulamaları
- Performans deseni tanıma
- Güvenlik açığı tespiti

### Kullanım Senaryosu
- Go projelerindeki tüm kod değişikliklerinde
- Go kodlama standartlarına uyumun gerekli olduğu projelerde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: Go dosyalarını bul
- Bash: go vet, staticcheck, golangci-lint çalıştır

### Diğer Ajanlarla İşbirliği
- go-build-resolver yapı hatalarını işler
- security-reviewer güvenlikle ilgili sorunları işler

---

## kotlin-reviewer

### Ad ve Kullanım
Kotlin ve Android/KMP Kod İnceleme uzmanı, idiomlu desenler, korutin güvenliği, Compose en iyi uygulamaları ve Clean Architecture ihlalleri konularında uzmanlaşmıştır.

### Yetenekler
- Kotlin idiom kontrolleri
- Korutin ve Flow anti-desen tespiti
- Compose performans sorunu tespiti
- Clean Architecture modül sınırları zorunluluğu
- Android özgü sorun kontrolleri

### Kullanım Senaryosu
- Android native geliştirmede
- Kotlin Multiplatform (KMP) projelerinde
- Jetpack Compose projelerinde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: Kotlin/KTS dosyalarını bul
- Bash: Gradle kontrollerini çalıştır

### Diğer Ajanlarla İşbirliği
- kotlin-build-resolver yapı sorunlarını işler
- security-reviewer kritik güvenlik sorunlarını işler

---

## rust-reviewer

### Ad ve Kullanım
Rust Kod İnceleme uzmanı, sahiplik, yaşam döngüsü güvenliği, hata yönetimi, unsafe kullanımı ve idiomlu desenler konularında uzmanlaşmıştır.

### Yetenekler
- Sahiplik ve yaşam döngüsü kontrolleri
- Ödünç denetleyici hata tanılaması
- Unsafe kod güvenlik incelemesi
- Hata yönetimi deseni doğrulama
- Eşzamanlılık güvenliği kontrolleri

### Kullanım Senaryosu
- Rust projelerindeki tüm kod değişikliklerinde
- Bellek güvenliği garantilerinin gerekli olduğu projelerde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: Rust dosyalarını bul
- Bash: cargo check, clippy, fmt, test, audit çalıştır

### Diğer Ajanlarla İşbirliği
- rust-build-resolver yapı hatalarını işler
- security-reviewer ile güvenlik inceleme kuralları paylaşılır

---

## cpp-reviewer

### Ad ve Kullanım
C++ Kod İnceleme uzmanı, bellek güvenliği, modern C++ idiomları, eşzamanlılık ve performans konularında uzmanlaşmıştır.

### Yetenekler
- Bellek güvenliği kontrolleri (ham new/delete, arabellek taşması)
- Modern C++ desenlerinin tanıtımı (akıllı işaretçiler, RAII)
- Eşzamanlılık güvenliği kontrolleri
- Performans deseni tanıma
- Güvenlik açığı tespiti

### Kullanım Senaryosu
- C++ projelerindeki tüm kod değişikliklerinde
- Modern C++ en iyi uygulamalarının gerekli olduğu projelerde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: C++ dosyalarını bul
- Bash: clang-tidy, cppcheck, cmake çalıştır

### Diğer Ajanlarla İşbirliği
- cpp-build-resolver yapı sorunlarını işler
- security-reviewer kritik güvenlik sorunlarını işler

---

## flutter-reviewer

### Ad ve Kullanım
Flutter ve Dart Kod İnceleme uzmanı, Flutter kodunun widget en iyi uygulamaları, durum yönetimi desenleri, Dart idiomları, performans tuzakları, erişilebilirlik ve Clean Architecture ihlallerini inceler.

### Yetenekler
- Durum yönetimi anti-desen tespiti (setState çözümleri görmezden gelme)
- Widget oluşturma performans optimizasyonu
- Mimari sınır zorunluluğu
- Kaynak yaşam döngüsü yönetimi
- Erişilebilirlik kontrolleri

### Kullanım Senaryosu
- Flutter projelerindeki tüm kod değişikliklerinde
- Çapraz platform mobil uygulama geliştirmede

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: Dart dosyalarını bul
- Bash: flutter analyze çalıştır

### Diğer Ajanlarla İşbirliği
- dart-build-resolver yapı sorunlarını işler
- security-reviewer kritik güvenlik sorunlarını işler
- e2e-runner uçtan uca test gerçekleştirir

---

## typescript-reviewer

### Ad ve Kullanım
TypeScript/JavaScript Kod İnceleme uzmanı, tür güvenliği, asenkron doğruluğu, Node/web güvenliği ve idiomlu desenler konularında uzmanlaşmıştır.

### Yetenekler
- Tür güvenliği kontrolleri (any滥用, non-null iddialar)
- Asenkron doğrulama
- Güvenlik açığı tespiti
- React/Next.js özgü kontroller
- Node.js özgü kontroller

### Kullanım Senaryosu
- TypeScript/JavaScript projelerindeki tüm kod değişikliklerinde
- Next.js ve React projelerinde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: TS/TSX/JS/JSX dosyalarını bul
- Bash: tsc, eslint, prettier, npm audit çalıştır

### Diğer Ajanlarla İşbirliği
- build-error-resolver yapı hatalarını işler
- security-reviewer kritik güvenlik sorunlarını işler

---

## swift-reviewer

### Ad ve Kullanım
Swift Kod İnceleme uzmanı, protokol odaklı tasarım, değer semantiği, ARC bellek yönetimi, Swift Concurrency ve idiomlu desenler konularında uzmanlaşmıştır.

### Yetenekler
- Swift Concurrency güvenlik kontrolleri
- Bellek yönetimi kontrolleri (güçlü referans döngüleri, delegasyon referansları)
- Protokol odaklı tasarım doğrulama
- Hata yönetimi en iyi uygulamaları
- Performans deseni tanıma

### Kullanım Senaryosu
- Swift projelerindeki tüm kod değişikliklerinde
- iOS/macOS uygulama geliştirmede

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: Swift dosyalarını bul
- Bash: swift build, swiftlint, swift test çalıştır

### Diğer Ajanlarla İşbirliği
- swift-build-resolver yapı sorunlarını işler
- security-reviewer kritik güvenlik sorunlarını işler

---

## csharp-reviewer

### Ad ve Kullanım
C# Kod İnceleme uzmanı, .NET kuralları, asenkron desenler, güvenlik, nullable referans türleri ve performans konularında uzmanlaşmıştır.

### Yetenekler
- .NET idiom kontrolleri
- Asenkron desen doğrulama
- Nullable tür güvenliği
- Güvenlik açığı tespiti
- EF Core özgü kontroller

### Kullanım Senaryosu
- C# projelerindeki tüm kod değişikliklerinde
- .NET/.NET Core uygulama geliştirmede

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: C# dosyalarını bul
- Bash: dotnet build, dotnet format, dotnet test çalıştır

### Diğer Ajanlarla İşbirliği
- build-error-resolver .NET yapı sorunlarını işler
- security-reviewer kritik güvenlik sorunlarını işler

---

## java-reviewer

### Ad ve Kullanım
Java Kod İnceleme uzmanı, Spring Boot ve Quarkus projeleri için kullanılır. Çerçeveleri otomatik olarak algılar ve uygun inceleme kurallarını uygular.

### Yetenekler
- Çerçeve otomatik algılama (Spring Boot/Quarkus)
- Katmanlı mimari kontrolleri
- JPA/Panache/MongoDB kontrolleri
- Eşzamanlılık güvenliği kontrolleri
- Güvenlik açığı tespiti

### Kullanım Senaryosu
- Spring Boot projelerinde
- Quarkus projelerinde
- Java 11+ projelerinde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: Java dosyalarını bul
- Bash: Maven/Gradle kontrollerini çalıştır

### Diğer Ajanlarla İşbirliği
- java-build-resolver yapı sorunlarını işler
- security-reviewer kritik güvenlik sorunlarını işler

---

## fsharp-reviewer

### Ad ve Kullanım
F# Kod İnceleme uzmanı, fonksiyonel idiomlar, tür güvenliği, desen eşleştirme, hesaplama ifadeleri ve performans konularında uzmanlaşmıştır.

### Yetenekler
- Fonksiyonel idiom kontrolleri
- Desen eşleştirme bütünlüğü doğrulama
- Tür güvenliği kontrolleri
- Hesaplama ifadesi en iyi uygulamaları
- Performans deseni tanıma

### Kullanım Senaryosu
- F# projelerindeki tüm kod değişikliklerinde
- Fonksiyonel programlama projelerinde

### Kullanılan Araçlar Listesi
- Read: Dosya içeriğini oku
- Grep: Kod desenlerini ara
- Glob: F# dosyalarını bul
- Bash: dotnet build, fantomas, dotnet test çalıştır

### Diğer Ajanlarla İşbirliği
- security-reviewer ile güvenlik inceleme kuralları paylaşılır
- Test kapsamını sağlamak için tdd-guide kullanılır
[Geri Dön Ajan Dizini](../README.md)
