# Pull Request İş Akışı Komutları

## Genel Bakış

PR iş akışı komutları, GitHub Pull Request oluşturmak, yönetmek ve incelemek için kullanılır.

## Komut Listesi

### /pr

**Kullanım**: Mevcut daldan gönderilmemiş işlemlerle GitHub PR oluştur

**Tanım**: Şablonu keşfeder, değişiklikleri analiz eder, dalı iter ve PR oluşturur. Tam işlem geçmişini analiz eder, tüm değişiklikleri görmek için `git diff [base-branch]...HEAD` kullanır.

**Giriş**: `$ARGUMENTS` - İsteğe bağlı, temel dal adı ve/veya bayrak (örn. `--draft`) içerebilir

**Ayrıştırma Kuralları**:
- Tanınan bayrakları çıkar (örn. `--draft`)
- Kalan bayrak olmayan metni temel dal adı olarak kullan
- Belirtilmemişse varsayılan olarak `main` kullan

**İş Akışı**:

| Aşama | Açıklama |
|---|---|
| **Aşama 1: DOĞRULA** | Ön koşulları kontrol et (base dalda değil, işlenmemiş değişiklik yok, ahead işlem var, mevcut PR yok) |
| **Aşama 2: KEŞFET** | PR şablonu ara, işlemleri analiz et, dosya değişikliklerini incele, ilgili planlama artifactlarını kontrol et |
| **Aşama 3: İT** | Dalı it (gerekirse önce rebase yap) |
| **Aşama 4: OLUŞTUR** | Şablon veya varsayılan formatla PR oluştur |
| **Aşama 5: DOĞRULA** | PR başarıyla oluşturulduğunu doğrula |
| **Aşama 6: ÇIKTI** | PR URL'sini ve sonraki adımı raporla |

**PR Oluşturma Doğrulama Kontrolleri**:

| Kontrol | Koşul | Başarısızlıkta Eylem |
|---|---|---|
| Base dalda değil | Mevcut dal ≠ base | Durdur: "Önce bir özellik dalına geç" |
| Çalışma alanı temiz | İşlenmemiş değişiklik yok | Uyarı: "Önce işle veya stash yap" |
| Ahead işlemler var | `git log origin/<base>..HEAD` boş değil | Durdur: "Ahead işlem yok" |
| Mevcut PR yok | `gh pr list` boş | Durdur: "PR zaten var" |

**Şablon Arama Sırası**:
1. `.github/PULL_REQUEST_TEMPLATE/` dizini (varsa)
2. `.github/PULL_REQUEST_TEMPLATE.md`
3. `.github/pull_request_template.md`
4. `docs/pull_request_template.md`

**Varsayılan PR Formatı** (şablon yoksa):
```markdown
## Summary
<1-2 sentence description>

## Changes
<bulleted list grouped by area>

## Files Changed
<table of changed files>

## Testing
<how changes were tested, or "Needs testing">

## Related Issues
<linked issues or "None">
```

**Kenar Durum İşleme**:
- **`gh` CLI yok**: Durdur ve kurulum ipucu ver
- **Kimlik doğrulama yok**: `gh auth login` çalıştırmasını söyle
- **Dal ayrışması**: `git fetch && git rebase` kullan sonra it
- **Büyük PR (>20 dosya)**: Uyarı ve bölmeyi öner

**Diğer Komutlarla Entegrasyon**:
- PR'yi incelemek için `/code-review <number>` kullan
- Hazır PR'leri birleştirmek için `gh pr merge <number>` kullan
- İlgili planlama artifactları oluşturmak için `/plan-prd` veya `/plan` kullan

---

### /review-pr

**Kullanım**: GitHub PR veya yerel işlenmemiş değişiklikleri incele

**Tanım**: GitHub'daki Pull Request veya yerel işlenmemiş değişiklikleri inceler. PR inceleme modu PRPs-agentic-eng'den uyarlanmıştır.

**Giriş**: `$ARGUMENTS` - PR numarası, PR URL veya boş (yerel inceleme) olabilir

---

#### Kalıp Seçimi

| Giriş | Eylem |
|---|---|
| PR numarası (örn. `42`) | PR numarası olarak kullan |
| URL (örn. `github.com/.../pull/42`) | PR numarasını çıkar |
| Dal adı | `gh pr list --head <branch>` ile PR'yi bul |
| Boş | Yerel inceleme modunu kullan |

---

#### Yerel İnceleme Modu

**İş Akışı**:
1. **TOPLA** - Değişiklikleri topla: `git diff --name-only HEAD`
2. **İNCELE** - Her dosyayı incele (güvenlik sorunları, kalite sorunları, en iyi uygulamalar)
3. **RAPORLA** - Şiddet seviyesi ve düzeltme önerileriyle rapor oluştur

**İnceleme Kontrol Listesi**:

| Kategori | Kontroller |
|---|---|
| **Güvenlik sorunları (CRITICAL)** | Sabit kodlu kimlik bilgileri, SQL enjeksiyonu, XSS, giriş doğrulama eksikliği, güvenli olmayan bağımlılıklar, yol geçişi |
| **Kod kalitesi (HIGH)** | Fonksiyon >50 satır, dosya >800 satır, iç içe geçme >4 seviye, hata yönetimi eksik, console.log, TODO/FIXME |
| **En iyi uygulamalar (MEDIUM)** | Değişiklik kalıpları, emoji kullanımı, yeni kodda test eksik, erişilebilirlik sorunları |

**Karar**:

| Koşul | Karar |
|---|---|
| CRITICAL veya HIGH sorun var | **BLOKLA** - İşlemeyi engelle |
| Yalnızca MEDIUM/LOW sorun var | Geçer ama yorumlarla |
| Sorun yok ve doğrulama geçti | **ONAYLA** |

---

#### PR İnceleme Modu

**İş Akışı**:
1. **GETİR** - PR meta verilerini ve farkını al
2. **BAĞLAM** - İnceleme bağlamı oluştur (proje kuralları, planlama artifactları, değişen dosyalar)
3. **İNCELE** - Tam dosyaları oku, 7 kategorili inceleme kontrol listesini uygula
4. **DOĞRULA** - Proje türü doğrulama komutlarını çalıştır
5. **KARAR VER** - Bulgulara göre öneri oluştur
6. **RAPORLA** - İnceleme artifactı oluştur
7. **YAYINLA** - İncelemeyi GitHub'da yayınla
8. **ÇIKTI** - Kullanıcıya raporla

**7 Kategorili İnceleme Kontrol Listesi**:

| Kategori | Kontroller |
|---|---|
| **Doğruluk** | Mantık hataları, kenar durumlar, boş işleme, yarış koşulları |
| **Tür Güvenliği** | Tür uyumsuzlukları, güvenli olmayan tür dönüşümleri, `any` kullanımı, eksik jenerikler |
| **Desen Uyumluluğu** | Proje kurallarına uyum (adlandırma, dosya yapısı, hata yönetimi, içe aktarmalar) |
| **Güvenlik** | Enjeksiyon, kimlik doğrulama boşlukları, secret ifşası, SSRF, yol geçişi, XSS |
| **Performans** | N+1 sorguları, eksik indeksler, sınırsız döngüler, bellek sızıntıları, büyük payload |
| **Bütünlük** | Test eksikliği, hata yönetimi eksikliği, eksik göçler, dokümantasyon eksikliği |
| **Bakım Kolaylığı** | Ölü kod, sihirli sayılar, derin iç içe geçme, belirsiz adlandırma, tür eksikliği |

**Doğrulama Komutu Algılama**:

| Proje Türü | Algılama Dosyası | Doğrulama Komutları |
|---|---|---|
| Node.js/TypeScript | `package.json` | `npm run typecheck`, `npm run lint`, `npm test`, `npm run build` |
| Rust | `Cargo.toml` | `cargo clippy`, `cargo test`, `cargo build` |
| Go | `go.mod` | `go vet ./...`, `go test ./...`, `go build ./...` |
| Python | `pyproject.toml` | `pytest` |

**Karar Seviyeleri**:

| Seviye | Anlamı | Eylem |
|---|---|---|
| CRITICAL | Güvenlik açıkları veya veri kaybı riski | Birleştirmeden önce zorunlu düzeltme |
| HIGH | Sorunlara yol açabilecek hatalar veya mantık hataları | Birleştirmeden önce düzeltilmeli |
| MEDIUM | Kod kalitesi veya en iyi uygulama eksiklikleri | Düzeltme önerilir |
| LOW | Stil sorunları veya küçük öneriler | İsteğe bağlı |

**GitHub'da İnceleme Yayınlama**:
```bash
# ONAYLA
gh pr review <NUMBER> --approve --body "<summary>"

# DEĞİŞİKLİK İSTE
gh pr review <NUMBER> --request-changes --body "<summary with required fixes>"

# YALNIZCA YORUM (taslak PR veya bilgilendirici)
gh pr review <NUMBER> --comment --body "<summary>"
```

---

### /multi-workflow

**Kullanım**: Çoklu model işbirliği geliştirme

**Tanım**: İşbirliği içinde geliştirmek için birden fazla AI modelini koordine eder.

---

### /multi-backend

**Kullanım**: Backend çoklu model geliştirme

**Tanım**: Backend geliştirme odaklı çoklu model işbirliği.

---

### /multi-frontend

**Kullanım**: Frontend çoklu model geliştirme

**Tanım**: Frontend geliştirme odaklı çoklu model işbirliği.

---

### /multi-execute

**Kullanım**: Çoklu model işbirliği yürütme

**Tanım**: Görevleri paralel yürütmek için birden fazla modeli koordine eder.

---

## İlgili Komutlar

- `/pr` - PR oluştur
- `/review-pr` - PR incele
- `/multi-workflow` - Çoklu model işbirliği
