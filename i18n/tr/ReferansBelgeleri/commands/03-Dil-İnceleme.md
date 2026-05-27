# Dil İnceleme Komutları

Bu doküman, ECC'de kullanılan çeşitli programlama dilleri için kod inceleme komutlarını tanıtır.

---

## /python-review

**Kullanım Açıklaması**: Python kodu kapsamlı incelemesi. PEP 8 uyumluluğu, tür ipuçları, güvenlik ve Pythonic kullanımı kontrol eder.

**Kullanım Yöntemi**:
```
/python-review
```

**Kullanım Senaryosu**:
- Python kodu yazdıktan veya değiştirdikten sonra
- Python değişikliklerini işlemeden önce
- Python kodu içeren PR'leri incelerken
- Pythonic kullanım ve en iyi uygulamaları öğrenirken

**İnceleme Kategorileri**:

### CRITICAL (Zorunlu düzeltme)
- SQL/komut enjeksiyonu açıkları
- Güvenli olmayan eval/exec kullanımı
- Pickle güvenli olmayan deserializasyon
- Sabit kodlu kimlik bilgileri
- YAML güvenli olmayan yükleme
- Hataları gizleyen çıplak except yan tümceleri

### HIGH (Düzeltilmeli)
- Tür ipucu eksik genel fonksiyonlar
- Değişken varsayılan parametreler
- Sessiz yutulan istisnalar
- Kaynak yönetimi için bağlam yöneticisi kullanılmıyor
- List comprehension yerine C tarzı döngüler
- type() yerine isinstance() kullanımı

### MEDIUM (Düşünülmeli)
- PEP 8 biçimlendirme ihlalleri
- Belge dizisi eksik genel fonksiyonlar
- Print ifadeleri yerine günlük kaydı
- Verimsiz dize işlemleri
- Adlandırılmış sabitler kullanılmayan sihirli sayılar
- f-strings kullanılmıyor

**Otomatik Kontrol Komutları**:
```bash
mypy .                              # Tür kontrolü
ruff check .                        # Linting
black --check .                     # Biçimlendirme kontrolü
isort --check-only .                # İçe aktarma sıralaması
bandit -r .                         # Güvenlik taraması
pip-audit                           # Bağımlılık denetimi
pytest --cov=app --cov-report=term-missing  # Test kapsamı
```

---

## /go-review

**Kullanım Açıklaması**: Go kodu kapsamlı incelemesi. İdiomatik desenleri, eşzamanlılık güvenliğini, hata yönetimini ve güvenliği kontrol eder.

**Kullanım Yöntemi**:
```
/go-review
```

**Kullanım Senaryosu**:
- Go kodu yazdıktan veya değiştirdikten sonra
- Go değişikliklerini işlemeden önce
- Go kodu içeren PR'leri incelerken
- Go idiomatik desenlerini öğrenirken

**İnceleme Kategorileri**:

### CRITICAL (Zorunlu düzeltme)
- SQL/komut enjeksiyonu açıkları
- Eşzamanlılık yarış koşulları
- Goroutine sızıntıları
- Sabit kodlu kimlik bilgileri
- Güvenli olmayan işaretçi kullanımı
- Kritik yollarda hata yoksayma

### HIGH (Düzeltilmeli)
- Eksik hata bağlam sarmalayıcıları
- Panic ile hata dönüşü
- Bağlam yayılmıyor
- Deadlock'a neden olan tamponlanmamış kanallar
- Arayüz uygulama hataları
- Eksik mutex koruması

### MEDIUM (Düşünülmeli)
- İdiomatik olmayan kod desenleri
- Godoc yorumu eksik dışa aktarılan fonksiyonlar
- Verimsiz dize birleştirme
- Slice ön tahsisatı yok
- Tablo güdümlü testler kullanılmıyor

**Otomatik Kontrol Komutları**:
```bash
go vet ./...                         # Statik analiz
staticcheck ./...                    # İleri kontroller (yüklüyse)
golangci-lint run                   # Linting
go build -race ./...                # Yarış tespiti
govulncheck ./...                  # Güvenlik açıkları
```

---

## /kotlin-review

**Kullanım Açıklaması**: Kotlin kodu kapsamlı incelemesi. İdiomatik desenleri, null güvenliği, korutin güvenliği ve güvenliği kontrol eder.

**Kullanım Yöntemi**:
```
/kotlin-review
```

**Kullanım Senaryosu**:
- Kotlin kodu yazdıktan veya değiştirdikten sonra
- Kotlin değişikliklerini işlemeden önce
- Android/KMP projelerinin PR'lerini incelerken
- Kotlin idiomatik desenlerini öğrenirken

**İnceleme Kategorileri**:

### CRITICAL (Zorunlu düzeltme)
- SQL/komut enjeksiyonu açıkları
- Yeterli gerekçe olmadan zorla açma `!!`
- Platform türü null güvenliği ihlalleri
- GlobalScope kullanımı (yapılandırılmış eşzamanlılık ihlali)
- Sabit kodlu kimlik bilgileri
- Güvenli olmayan deserializasyon

### HIGH (Düzeltilmeli)
- Değişken durum (değişmez kullanılabilirken)
- Korutin bağlamında engelleyici çağrılar
- Uzun döngülerde iptal kontrolü eksik
- Sealed class `when` nonexhaustive
- Büyük fonksiyonlar (>50 satır)
- Derin iç içe geçme (>4 seviye)

### MEDIUM (Düşünülmeli)
- İdiomatik olmayan Kotlin (Java tarzı desenler)
- Sondaki virgül eksik
- Scope fonksiyonları yanlış kullanımı veya iç içe geçme
- Büyük koleksiyon zincirlerinde Sequence eksik
- Gereksiz açık tür

**Otomatik Kontrol Komutları**:
```bash
./gradlew build                      # Yapı kontrolü
./gradlew detekt                     # Statik analiz
./gradlew ktlintCheck                # Biçimlendirme kontrolü
./gradlew test                       # Testler
```

---

## /rust-review

**Kullanım Açıklaması**: Rust kodu kapsamlı incelemesi. Sahiplik, yaşam döngüsü, güvenli olmayan kullanım ve idiomatik desenleri kontrol eder.

**Kullanım Yöntemi**:
```
/rust-review
```

**Kullanım Senaryosu**:
- Rust kodu yazdıktan veya değiştirdikten sonra
- Rust değişikliklerini işlemeden önce
- Rust projelerinin PR'lerini incelerken
- Rust idiomatik desenlerini öğrenirken

**İnceleme Kategorileri**:

### CRITICAL (Zorunlu düzeltme)
- Üretim kodu yollarında kontrol edilmemiş `unwrap()`/`expect()`
- `// SAFETY:` yorumu olmadan `unsafe`
- Dize interpolasyonlu SQL enjeksiyonu
- Doğrulanmamış girdilerle komut enjeksiyonu
- Sabit kodlu kimlik bilgileri
- Serbest bırakıldıktan sonra kullanım

### HIGH (Düzeltilmeli)
- Ödünç denetleyiciyi tatmin etmek için gereksiz `.clone()`
- `String` parametreleri için `&str` veya `impl AsRef<str>` kullanılmalı
- Asenkron bağlamda engelleme (`std::thread::sleep`)
- Paylaşılan türlerde `Send`/`Sync` kısıtlamaları eksik
- Kritik enumlarda joker karakter `_ =>` eşleşmesi
- Büyük fonksiyonlar (>50 satır)

### MEDIUM (Düşünülmeli)
- Sıcak yollarda gereksiz tahsisatlar
- Bilinen boyut için `with_capacity` eksik
- Yeterli gerekçe olmadan bastırılmış clippy uyarıları
- Genel API'lerde `///` dokümantasyon eksik
- Olası değer yoksayma için `#[must_use]` dönüş türlerinde kullanım düşünülmeli

**Otomatik Kontrol Komutları**:
```bash
cargo check                           # Yapı kontrolü
cargo clippy -- -D warnings            # Lints
cargo fmt --check                      # Biçimlendirme kontrolü
cargo test                            # Testler
cargo audit                           # Güvenlik denetimi (yüklüyse)
```

---

## /cpp-review

**Kullanım Açıklaması**: C++ kodu kapsamlı incelemesi. Bellek güvenliği, modern C++ idiomatik kullanımı, eşzamanlılık ve güvenliği kontrol eder.

**Kullanım Yöntemi**:
```
/cpp-review
```

**Kullanım Senaryosu**:
- C++ kodu yazdıktan veya değiştirdikten sonra
- C++ değişikliklerini işlemeden önce
- C++ projelerinin PR'lerini incelerken
- Bellek güvenliği sorunlarını kontrol ederken

**İnceleme Kategorileri**:

### CRITICAL (Zorunlu düzeltme)
- RAII olmadan çıplak `new`/`delete`
- Arabellek taşması ve kullanım sonrası boşaltma
- Eşzamanlılık yarışı olan veriler
- `system()` ile komut enjeksiyonu
- Başlatılmamış değişken okuma
- Boş işaretçi başvurusu

### HIGH (Düzeltilmeli)
- Beş kural ihlalleri
- `std::lock_guard` / `std::scoped_lock` eksik
- Ömür yanlış yönetimi olan ayrılmış iş parçacıkları
- C tarzı dönüştürmeler yerine `static_cast`/`dynamic_cast`
- `const` doğruluğu eksik

### MEDIUM (Düşünülmeli)
- Gereksiz kopyalama (const& yerine değer geçirme)
- Bilinen boyut için `reserve()` eksik
- Header dosyalarında `using namespace std;`
- Önemli dönüş değerleri için `[[nodiscard]]` eksik
- Aşırı karmaşık şablon metaprogramlaması

**Otomatik Kontrol Komutları**:
```bash
clang-tidy --checks='*,-llvmlibc-*' src/*.cpp -- -std=c++17
cppcheck --enable=all --suppress=missingIncludeSystem src/
cmake --build build -- -Wall -Wextra -Wpedantic
```

---

## /flutter-review

**Kullanım Açıklaması**: Flutter/Dart kod incelemesi. İdiomatik desenleri, widget en iyi uygulamaları, durum yönetimi, performans, erişilebilirlik ve güvenliği kontrol eder.

**Kullanım Yöntemi**:
```
/flutter-review
```

**Kullanım Senaryosu**:
- Flutter/Dart değişikliklerini içeren PR'ları işlemeden önce (yapı ve testler geçtikten sonra)
- Yeni özellikler uyguladıktan sonra erken aşamada sorunları yakalarken
- Başkalarının Flutter kodunu incelerken
- Bileşenleri, durum yönetimini veya hizmet sınıflarını denetlerken
- Üretim yayınlamadan önce

**İnceleme Alanları**:

| Alan | Şiddet Seviyesi |
|------|----------|
| Sabit kodlu anahtarlar, düz metin HTTP | CRITICAL |
| Mimari ihlalleri, durum yönetimi anti-desenleri | CRITICAL |
| Widget yeniden oluşturma sorunları, kaynak sızıntıları | HIGH |
| `dispose()` eksik, await sonrası BuildContext | HIGH |
| Dart null güvenliği, hata/yükleme durumu eksik | HIGH |
| Const yayma, widget composisyonu | HIGH |
| `build()` içinde pahalı iş | HIGH |
| Erişilebilirlik, semantik etiketler | MEDIUM |
| Durum geçişleri için test eksik | HIGH |
| Sabit kodlu dizeler (l10n) | MEDIUM |

---

## /fastapi-review

**Kullanım Açıklaması**: FastAPI uygulaması incelemesi. Mimari, asenkron doğruluğu, bağımlılık enjeksiyonu, Pydantic şemaları, güvenlik, performans ve test edilebilirliği kontrol eder.

**Kullanım Yöntemi**:
```
/fastapi-review [dosya veya dizin]
```

**İnceleme Alanları**:
- App factory, rota sınırları, ara yazılım ve istisna işleyicileri
- Pydantic istek ve yanıt şemas ayrımı
- Veritabanı oturumları, kimlik doğrulama, sayfalandırma ve bağımlılık ayarları
- Asenkron veritabanı ve harici HTTP şemaları
- CORS, kimlik doğrulama, hız sınırlama, günlük kaydı ve anahtar işleme
- OpenAPI meta verileri ve dokümante edilmiş yanıt modelleri
- Test istemci kurulumu ve bağımlılık override'ları

---

## Dil İnceleme Komutları Karşılaştırma Tablosu

| Komut | Dil | Kritik Kontroller | Şiddet Seviyeleri |
|------|------|----------|----------|
| `/python-review` | Python | PEP 8, Tür İpuçları, SQL Enjeksiyonu | CRITICAL/HIGH/MEDIUM |
| `/go-review` | Go | Eşzamanlılık güvenliği, hata yönetimi, goroutine | CRITICAL/HIGH/MEDIUM |
| `/kotlin-review` | Kotlin | Null güvenliği, korutinler, GlobalScope | CRITICAL/HIGH/MEDIUM |
| `/rust-review` | Rust | Sahiplik, yaşam döngüsü, güvenli olmayan | CRITICAL/HIGH/MEDIUM |
| `/cpp-review` | C++ | Bellek güvenliği, RAII, eşzamanlılık | CRITICAL/HIGH/MEDIUM |
| `/flutter-review` | Flutter | Durum yönetimi, erişilebilirlik, performans | CRITICAL/HIGH/MEDIUM |
| `/fastapi-review` | Python | Pydantic, DI, asenkron, güvenlik | CRITICAL/HIGH/MEDIUM |
