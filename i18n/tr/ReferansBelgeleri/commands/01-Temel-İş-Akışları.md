# Temel İş Akışları Komutları

Bu doküman, ECC'de kullanılan temel geliştirme iş akışlarının komutlarını tanıtır.

---

## /plan

**Kullanım Açıklaması**: Gereksinimleri yeniden ifade eder, riskleri değerlendirir ve adım adım uygulama planı oluşturur. Herhangi bir koda dokunmadan **önce kullanıcı onayını bekler**.

**Kullanım Yöntemi**:
```
/plan [özellik tanımı | PRD dosya yolu]
```

**Kullanım Senaryosu**:
- Yeni özellik geliştirmeye başlarken
- Büyük mimari değişiklikler yaparken
- Karmaşık yeniden düzenleme çalışmalarında
- Gereksinimler belirsiz veya muğlak olduğunda

**Detaylı Açıklama**:
Bu komut şunları yapar:
1. **Gereksinimleri yeniden ifade et** - Ne inşa edilmesi gerektiğini net bir şekilde ifade et
2. **Riskleri tanımla** - Potansiyel sorunları ve engelleri ortaya çıkar
3. **Adım adım plan oluştur** - Uygulamayı aşamalara ayrıştır
4. **Onay bekler** - Devam etmeden önce kullanıcının açık onayını zorunlu kıl

Birden fazla giriş modunu destekler:
| Giriş | Mod | Davranış |
|------|------|------|
| `path/to/name.prd.md` | PRD modu | PRD'yi okur, işlenecek bir sonraki teslimat dönüm noktasını seçer |
| Diğer markdown yolları | Referans modu | Dosyayı bağlam olarak okur ve satır içi plan oluşturur |
| Serbest metin | Sohbet modu | Satır içi plan oluşturur |
| Boş giriş | Netleştirme modu | Neyin planlanacağını sorar |

---

## /code-review

**Kullanım Açıklaması**: Yerel olarak işlenmemiş değişikliklerin veya GitHub PR'nin kapsamlı kod incelemesi.

**Kullanım Yöntemi**:
```
/code-review                 # Yerel inceleme modu
/code-review [PR numarası | PR bağlantısı]  # PR inceleme modu
```

**Kullanım Senaryosu**:
- Kodu işlemeden önce kapsamlı inceleme yaparken
- Potansiyel sorunları keşfetmek için PR incelemesi
- Kod tabanındaki en iyi uygulamaları öğrenirken

**Yerel İnceleme Modu**:
1. Değişen dosyaları topla (`git diff --name-only HEAD`)
2. Güvenlik sorunlarını kontrol et (Sabit Kodlu Kimlik Bilgileri, SQL enjeksiyonu, XSS vb.)
3. Kod Kalitesi sorunlarını kontrol et (fonksiyon >50 satır, dosya >800 satır vb.)
4. Şiddet seviyesiyle işaretlenmiş rapor oluştur

**PR İnceleme Modu**:
1. PR meta verilerini ve farkını al
2. İlgili proje spesifikasyonlarını ve planlama dokümanlarını kontrol et
3. Değişen tüm dosyaları oku
4. 7 inceleme kategorisi uygula (doğruluk, tür güvenliği, desen uyumluluğu, güvenlik, performans, bütünlük, bakım kolaylığı)
5. Doğrulama komutlarını çalıştır (tür kontrolü, lint, test, yapı)
6. İnceleme yorumlarını GitHub'da yayınla

---

## /build-fix

**Kullanım Açıklaması**: Proje yapı sistemini algılar ve adım adım yapı/tür hatalarını onarır.

**Kullanım Yöntemi**:
```
/build-fix
```

**Kullanım Senaryosu**:
- Yapı başarısız olduğunda
- TypeScript/JavaScript tür hataları
- Derleme hataları
- Bağımlılık sorunları

**İş Akışı**:
1. **Yapı sistemini algıla** - Dosya türüne göre uygun yapı komutunu seç
2. **Hataları ayrıştır** - Dosya ve bağımlılık sırasına göre gruplandır
3. **Adım adım onar** - Bir seferde bir hata onar
4. **Doğrula** - Her onardan sonra yapılandırmayı yeniden çalıştır

Desteklenen yapı sistemleri:

| Gösterge | Yapı Komutu |
|--------|----------|
| `package.json` + `build` betiği | `npm run build` |
| `tsconfig.json` (TypeScript) | `npx tsc --noEmit` |
| `Cargo.toml` | `cargo build` |
| `pom.xml` | `mvn compile` |
| `build.gradle` | `./gradlew compileJava` |
| `go.mod` | `go build ./...` |
| `pyproject.toml` | `python -m compileall` veya `mypy` |

---

## /verify

**Kullanım Açıklaması**: Kod değişikliklerinin beklendiği gibi çalıştığını doğrular. PR'leri doğrulamak, düzeltmeleri onaylamak veya değişiklikleri test etmek için kullanılır.

**Kullanım Yöntemi**:
```
/verify [quick]           # Hızlı doğrulama (önerilen)
/verify full             # Tam doğrulama
```

**Kullanım Senaryosu**:
- PR değişikliklerini doğrulama
- Düzeltmelerin etkili olduğunu onaylama
- Değişiklikleri manuel test etme
- İtmmeden önce yerel doğrulama

**Doğrulama Adımları**:
1. İlgili testleri çalıştır
2. Tür güvenliğini kontrol et
3. Yapının başarılı olduğunu doğrula
4. Linting'i kontrol et

---

## /quality-gate

**Kullanım Açıklaması**: Bir dosya veya proje kapsamı için ECC kalite pipeline'ını çalıştırır ve onarım adımlarını raporlar.

**Kullanım Yöntemi**:
```
/quality-gate [yol|.] [--fix] [--strict]
```

**Kullanım Senaryosu**:
- İsteğe bağlı kalite kontrollerini çalıştırırken
- CI pipeline'ında entegrasyon
- Kodun hedef standartlara uygun olduğundan emin olma

**Pipeline Adımları**:
1. Hedef dil/araçları algıla
2. Formatlayıcı kontrollerini çalıştır
3. Lint/Tür kontrollerini çalıştır
4. Kısa bir onarım listesi oluştur

---

## /tdd

**Kullanım Açıklaması**: Test Güdümlü Geliştirme iş akışı. Kırmızı-yeşil-yeniden düzenleme döngüsünü takip eder.

**Kullanım Yöntemi**:
```
/tdd                      # TDD iş akışını başlat
```

**Kullanım Senaryosu**:
- Yeni özellik uygularken
- Hata düzeltirken (önce başarısız test yazarak)
- Test kapsamı eklerken
- TDD metodolojisini öğrenirken

**TDD Döngüsü**:
```
KIRMIZI → Başarısız bir test yaz
YEŞİL  → Testi geçirmek için minimum kodu yaz
REFACTOR → Kodu iyileştir, testlerin yeşil kalmasını sağla
TEKRARLA → Sonraki test durumu için
```

**En İyi Uygulamalar**:
- **Önce test yaz** - Herhangi bir uygulama yapmadan önce
- **Her değişiklikten sonra testleri çalıştır** - İlerlemeyi doğrula
- **Uygulama yerine davranışı test et** - Arayüze odaklan, detaylara değil
- **Sınır durumlarını dahil et** - Boş değerler, null, maksimum değerler vb.

---

## Komut Entegrasyon İlişkisi

```
Gereksinimler belirsiz → /plan-prd (PRD oluştur)
     ↓
Gereksinimler belirgin → /plan (uygulama planı oluştur)
     ↓
/tdd (veya /feature-dev) uygula
     ↓
/build-fix Yapı Hatalarını Onar
     ↓
/code-review Kod İnceleme
     ↓
/quality-gate Kalite kapısı
     ↓
/verify Doğrulama
     ↓
/pr PR Oluştur
```
