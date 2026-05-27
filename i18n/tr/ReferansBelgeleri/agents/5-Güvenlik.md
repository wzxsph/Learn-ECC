# Güvenlik Ajanı

Güvenlik Ajanı, güvenlik açıkları tespiti, uyumluluk incelemesi ve hassas bilgi korumasından sorumludur.

## Ajan Listesi

| Ajan Adı | Kullanım | Kullanım Modeli | Temel Araçlar |
|------------|------|----------|----------|
| security-reviewer | Güvenlik açıkları tespit ve düzeltme uzmanı | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| silent-failure-hunter | Sessiz başarısızlık tespit uzmanı | sonnet | Read, Grep, Glob, Bash |
| healthcare-reviewer | Tıbbi uygulama kod inceleme (PHI/klinik güvenlik) | opus | Read, Grep, Glob |
| opensource-sanitizer | Açık kaynak proje sızıntı kontrol uzmanı | sonnet | Read, Grep, Glob, Bash |

---

## security-reviewer

### Ad ve Kullanım
Güvenlik açıkları tespit ve düzeltme uzmanı. Kullanıcı girişi, kimlik doğrulama, API uç noktaları veya hassas verileri işleyen kod yazıldıktan sonra proaktif olarak kullanılır. Secrets, SSRF, enjeksiyon, güvenli olmayan şifreleme ve OWASP Top 10 açıklarını işaretler.

### Yetenekler
- Açık tespiti - OWASP Top 10 ve yaygın güvenlik sorunlarını tanımlama
- Secrets tespiti - Sabit kodlu API anahtarları, şifreler, tokenlar bulma
- Giriş doğrulama - Tüm kullanıcı girdilerinin düzgün temizlendiğinden emin olma
- Kimlik doğrulama/Yetkilendirme - Uygun erişim kontrolünün doğrulanması
- Bağımlılık güvenliği - Güvenlik açığı olan npm paketlerini kontrol etme
- Güvenlik en iyi uygulamaları - Güvenlik kodlama desenlerini zorunlu kılma

### Kullanım Senaryosu
- Yeni API uç noktaları
- Kimlik doğrulama kodu değişiklikleri
- Kullanıcı girişi işleme
- Veritabanı sorgusu değişiklikleri
- Dosya yükleme
- Ödeme kodu
- Harici API entegrasyonu
- Bağımlılık güncellemeleri

### Kullanılan Araçlar Listesi
- Read: Kodu oku
- Write: Güvenlik düzeltmeleri yaz
- Edit: Güvenlik düzeltmelerini düzenle
- Bash: npm audit, eslint çalıştır
- Grep: Hassas desenleri ara
- Glob: Dosyaları bul

### Diğer Ajanlarla İşbirliği
- code-reviewer kod kalitesi kontrollerini paylaşır
- healthcare-reviewer tıbbi özgü güvenlik sorunlarını işler
- opensource-sanitizer açık kaynak yayın öncesi kontrollerini işler

### İlk Tarama

```bash
npm audit --audit-level=high
npx eslint . --plugin security
```

Sabit kodlu secrets ara, yüksek riskli alanları incele: auth, API uç noktaları, DB sorguları, dosya yükleme, ödeme, webhooks.

### OWASP Top 10 Kontrolleri

1. **Enjeksiyon** - Sorgular parametreli mi? Kullanıcı girişi temizleniyor mu? ORM güvenli kullanılıyor mu?
2. **Kimlik doğrulama bozulması** - Şifreler hash'leniyor mu (bcrypt/argon2)? JWT doğrulanıyor mu? Oturumlar güvenli mi?
3. **Hassas veriler** - HTTPS zorunlu mu? Secrets ortam değişkenlerinde mi? PII şifreli mi? Günlükler temizleniyor mu?
4. **XXE** - XML ayrıştırıcıları güvenli yapılandırılmış mı? Harici varlıklar devre dışı mı?
5. **Erişim kontrolü bozulması** - Her rota auth kontrolü yapıyor mu? CORS doğru yapılandırılmış mı?
6. **Güvenlik yapılandırma hataları** - Varsayılan kimlik bilgileri değiştirildi mi? Üretim ortamı debug modu kapalı mı? Güvenlik başlıkları ayarlanmış mı?
7. **XSS** - Çıktı kaçınıyor mu? CSP ayarlanmış mı? Çerçeveler otomatik kaçıyor mu?
8. **Güvenli olmayan deserialization** - Kullanıcı girişi güvenli deserializasyon yapılıyor mu?
9. **Bilinen açıklar** - Bağımlılıklar güncel mi? npm audit geçiyor mu?
10. **Yetersiz günlük ve izleme** - Güvenlik olayları günlüğe kaydediliyor mu? Uyarılar yapılandırılmış mı?

### Kod Deseni İncelemesi

Bu desenleri derhal işaretle:

| Desen | Şiddet | Düzeltme |
|------|--------|------|
| Sabit kodlu secrets | KRİTİK | `process.env` kullan |
| Kullanıcı girişli shell komutları | KRİTİK | Güvenli API veya execFile kullan |
| String birleştirmeli SQL | KRİTİK | Parametreli sorgular kullan |
| `innerHTML = userInput` | YÜKSEK | `textContent` veya DOMPurify kullan |
| `fetch(userProvidedUrl)` | YÜKSEK | İzin verilen domainleri beyaz listele |
| Düz metin şifre karşılaştırması | KRİTİK | `bcrypt.compare()` kullan |
| Auth kontrolü olmayan rota | KRİTİK | Kimlik doğrulama ara yazılımı ekle |
| Kilit olmadan bakiye kontrolü | KRİTİK | İşlemde `FOR UPDATE` kullan |
| Hız sınırı yok | YÜKSEK | `express-rate-limit` ekle |
| Şifre/secrets günlüğe kaydetme | ORTA | Günlük çıktısını temizle |

### Temel İlkeler

1. **Derinlemesine savunma** - Çoklu güvenlik katmanları
2. **Minimum yetki** - Gerekli minimum yetki
3. **Güvenlik başarısızlığı** - Hatalar veri ifşa etmemeli
4. **Girişe güvenme** - Her şeyi doğrula ve temizle
5. **Düzenli güncelleme** - Bağımlılıkları güncel tut

### Yaygın Yanlış Pozitifler

- `.env.example`'daki ortam değişkenleri (gerçek secrets değil)
- Test dosyalarındaki test kimlik bilgileri (açıkça işaretlenmişse)
- Gerçekten herkese açık olan genel API anahtarları
- Sağlama toplamları için kullanılan SHA256/MD5 (şifre değil)

**İşaretlemeden önce her zaman bağlamı doğrula.**

### Acil Müdahale

KRİTİK açık bulunursa:
1. Ayrıntılı rapor kaydet
2. Derhal proje liderini bildir
3. Güvenli kod örneği sağla
4. Düzeltmenin etkili olduğunu doğrula
5. Kimlik bilgileri ifşa olduysa secrets'ı döndür

---

## silent-failure-hunter

### Ad ve Kullanım
Koddaki sessiz başarısızlıkları, yutulan hataları, yanlış geri dönüşleri ve eksik hata yaymayı inceler.

### Yetenekler
- Boş catch blokları tespiti
- Yetersiz günlük tespiti
- Tehlikeli geri dönüş tespiti
- Hata yayma sorunları tespiti
- Eksik hata yönetimi tespiti

### Kullanım Senaryosu
- Kod incelenirken
- Tuhaf hatalar bulunduğunda
- Pipeline yeşil görünürken veri atlandığında
- İstisna işleme incelenirken

### Kullanılan Araçlar Listesi
- Read: Kodu oku
- Grep: Hata yönetimi desenlerini ara
- Glob: Kaynak dosyaları bul
- Bash: Testleri çalıştır

### Diğer Ajanlarla İşbirliği
- code-reviewer kod kalitesi kontrollerini paylaşır
- security-reviewer güvenlikle ilgili sorunları işler
- mle-reviewer ML pipeline sorunlarını işler

### Av Hedefleri

#### 1. Boş Catch Blokları
- `catch {}` veya yoksayılan istisnalar
- Bağlam olmadan `null` / boş diziye dönüştürme

#### 2. Yetersiz Günlük
- Yetersiz günlük bağlamı
- Yanlış hata şiddeti
- log-and-forget işleme

#### 3. Tehlikeli Geri Dönüşler
- Gerçek başarısızlıkları gizleyen varsayılan değerler
- `.catch(() => [])`
- Aşağı akış hatalarını tanılamayı zorlaştıran zarif yollar

#### 4. Hata Yayma Sorunları
- Kayıp yığın izleri
- Jenerik rethrows
- Eksik async işleme

#### 5. Eksik Hata Yönetimi
- Zaman aşımı veya hata yönetimi olmayan ağ/dosya/veritabanı yolları
- Rollback olmadan işlem işleri

### Çıktı Formatı

Her bulgu için:
- Konum
- Şiddet
- Sorun
- Etki
- Düzeltme önerisi

---

## healthcare-reviewer

### Ad ve Kullanım
Tıbbi uygulama kodunun klinik güvenliğini, CDSS doğruluğunu, PHI uyumluluğunu ve tıbbi veri bütünlüğünü inceler. EMR/EHR, klinik karar destek sistemleri ve sağlık bilgi sistemleri için özel olarak tasarlanmıştır.

### Yetenekler
- CDSS doğruluğu - İlaç etkileşim mantığını, doz doğrulama kurallarını ve klinik skorları doğrulama
- PHI/PII koruma - Günlüklerde, hatalarda, yanıtlarda, URL'lerde ve istemci depolamada hasta verisi ifşasını tarama
- Klinik veri bütünlüğü - Denetim izleri, kilitli kayıtlar ve basamaklı korumalar sağlama
- Tıbbi veri doğruluğu - ICD-10/SNOMED eşlemelerini, laboratuvar referans aralıklarını ve ilaç veritabanı girişlerini doğrulama
- Entegrasyon uyumluluğu - HL7/FHIR mesaj işleme ve hata kurtarmayı doğrulama

### Kullanım Senaryosu
- EMR/EHR sistem geliştirme
- Klinik karar destek sistemleri
- Sağlık bilgi sistemleri
- Tıbbi veri işleme uygulamaları

### Kullanılan Araçlar Listesi
- Read: Tıbbi kodu oku
- Grep: PHI desenlerini ara
- Glob: Tıbbi ilgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- security-reviewer genel güvenlik açıklarını işler
- code-reviewer kod kalitesi sorunlarını işler
- silent-failure-hunter sessiz başarısızlık sorunlarını işler

### Kritik Kontroller

#### CDSS Motoru
- [ ] Tüm ilaç etkileşim çiftleri doğru uyarı üretiyor (çift yönlü)
- [ ] Doz doğrulama kuralları aralık dışı değerlerde tetikleniyor
- [ ] Klinik skorlar yayınlanan spesifikasyonla eşleşiyor
- [ ] Yanlış negatif yok (kaçırılan etkileşimler = hasta güvenlik olayı)
- [ ] Hatalı biçimlendirilmiş girdiler sessizce geçmek yerine hata üretiyor

#### PHI Koruma
- [ ] Hasta verileri `console.log`, `console.error` veya hata mesajlarında yok
- [ ] PHI URL parametrelerinde veya sorgu dizelerinde değil
- [ ] PHI tarayıcı localStorage/sessionStorage'ta değil
- [ ] İstemci kodunda `service_role` anahtarı yok
- [ ] Tüm hasta veri tablolarında RLS etkin
- [ ] Tesisler arası veri izolasyonu doğrulanmış

#### Klinik İş Akışları
- [ ] Tanı kilidi düzenlemeyi önlüyor (yalnızca ek)
- [ ] Her klinik veri CRUD için denetim izi girişi var
- [ ] Kritik uyarılar göz ardı edilemez (toast bildirimi değil)
- [ ] Klinik uyarılarla devam eden klinisyenlerin önyargı nedenini kaydediyor
- [ ] Kırmızı bayrak belirtileri görünür uyarı tetikliyor

#### Veri Bütünlüğü
- [ ] Hasta kayıtlarında CASCADE DELETE yok
- [ ] Eşzamanlı düzenleme tespiti (iyimser kilit veya çatışma çözümü)
- [ ] Klinik tablolarda yalnız kayıt yok
- [ ] Zaman damgaları tutarlı saat dilimi kullanıyor

### Çıktı Formatı

```
## Healthcare Review: [modül/özellik]

### Hasta Güvenliği Etkisi: [KRİTİK / YÜKSEK / ORTA / DÜŞÜK / HİÇBİRİ]

### Klinik Doğruluk
- CDSS: [kontrol geçti/başarısız]
- İlaç DB: [doğrulandı/sorun]
- Skorlama: [spesifikasyona uygun/sapma]

### PHI Uyumluluğu
- Maruz kalma vektör kontrolü: [liste]
- Bulunan sorunlar: [liste veya yok]

### Sorunlar
1. [HASTA GÜVENLİĞİ / KLINIK / PHI / TEKNİK] Açıklama
   - Etki: [Potansiyel zarar veya ifşa]
   - Düzeltme: [Gerekli değişiklik]

### Karar: [YAYINLAMAK GÜVENLİ / DÜZELTİLMELİ / ENGELLE — HASTA GÜVENLİĞİ RİSKİ]
```

### Kurallar

- Klinik doğruluk konusunda şüphe varsa, inceleme gerekiyor olarak işaretle - asla belirsiz klinik mantığı onaylama
- Bir kaçırılan ilaç etkileşimi yüz tane yanlış alarmdan daha kötüdür
- PHI ifşası her zaman kritik şiddettedir, ne kadar küçük olursa olsun
- CDSS hatalarını sessizce yakalayan kodu asla onaylama

---

## opensource-sanitizer

### Ad ve Kullanım
Yayın öncesi açık kaynak fork'ün tamamen temizlendiğini doğrular. Sızdırılan secrets, PII, dahili referanslar ve tehlikeli dosyaları tarar. 20+ regex desen kullanır. PASS/FAIL/PASS-WITH-WARNINGS raporu oluşturur.

### Yetenekler
- Secrets tarama - API anahtarları, şifreler, token tarama
- PII tarama - E-posta, IP adresleri tarama
- Dahili referans tarama - Mutlak yollar, dahili domainler tarama
- Tehlikeli dosya kontrolü - .env, credentials.json vb. kontrol etme
- Yapılandırma bütünlüğü doğrulama - .env.example doğrulama
- Git geçmişi denetimi - Sızdırılan kimlik bilgilerini kontrol etme

### Kullanım Senaryosu
- Açık kaynak fork yayın öncesi
- Üçüncü parti kod denetimi öncesi
- Kod yayın öncesi güvenlik kontrolü

### Kullanılan Araçlar Listesi
- Read: Dosyaları oku
- Grep: Hassas desenleri ara
- Glob: Dosyaları bul
- Bash: git log vb. çalıştır

### Diğer Ajanlarla İşbirliği
- security-reviewer genel güvenlik açıklarını işler
- code-reviewer kod kalitesi sorunlarını işler
- doc-updater dokümantasyonu günceller

### İş Akışı

#### Adım 1: Secrets Taraması (KRİTİK)

Her metin dosyasını tarayın (şunları hariç tutarak: `node_modules`, `.git`, `__pycache__`, `*.min.js`, ikili dosyalar):

```
# API anahtarları
pattern: [A-Za-z0-9_]*(api[_-]?key|apikey|api[_-]?secret)[A-Za-z0-9_]*\s*[=:]\s*['"]?[A-Za-z0-9+/=_-]{16,}

# AWS
pattern: AKIA[0-9A-Z]{16}
pattern: (?i)(aws_secret_access_key|aws_secret)\s*[=:]\s*['"]?[A-Za-z0-9+/=]{20,}

# Veritabanı URL'leri (kimlik bilgileriyle)
pattern: (postgres|mysql|mongodb|redis)://[^:]+:[^@]+@[^\s'"]+

# JWT tokenleri (3 segmentli)
pattern: eyJ[A-Za-z0-9_-]{20,}\.eyJ[A-Za-z0-9_-]{20,}\.[A-Za-z0-9_-]+

# Özel anahtarlar
pattern: -----BEGIN\s+(RSA\s+|EC\s+|DSA\s+|OPENSSH\s+)?PRIVATE KEY-----

# GitHub tokenleri
pattern: gh[pousr]_[A-Za-z0-9_]{36,}
pattern: github_pat_[A-Za-z0-9_]{22,}
```

#### Adım 2: PII Taraması (KRİTİK)

```
# Kişisel e-postalar (noreply@, info@ gibi genel e-postalar değil)
pattern: [a-zA-Z0-9._%+-]+@(gmail|yahoo|hotmail|outlook|protonmail|icloud)\.(com|net|org)

# Özel IP adresleri (dahili altyapıyı temsil eden)
pattern: (192\.168\.\d+\.\d+|10\.\d+\.\d+\.\d+|172\.(1[6-9]|2\d|3[01])\.\d+\.\d+)

# SSH bağlantı dizeleri
pattern: ssh\s+[a-z]+@[0-9.]+
```

#### Adım 3: Dahili Referans Taraması (KRİTİK)

```
# Belirli kullanıcı ana dizinine mutlak yollar
pattern: /home/[a-z][a-z0-9_-]*/
pattern: /Users/[A-Za-z][A-Za-z0-9_-]*/
pattern: C:\\Users\\[A-Za-z]

# Dahili secret dosya referansları
pattern: \.secrets/
pattern: source\s+~/\.secrets/
```

#### Adım 4: Tehlikeli Dosya Kontrolü (KRİTİK)

Bunların **yok** olduğunu doğrula:
```
.env (her varyant: .env.local, .env.production, .env.*.local)
*.pem, *.key, *.p12, *.pfx, *.jks
credentials.json, service-account*.json
.secrets/, secrets/
.claude/settings.json
sessions/
*.map (source map'ler orijinal kaynak yapısını ifşa eder)
node_modules/, __pycache__/, .venv/, venv/
```

#### Adım 5: Git Geçmişi Denetimi

```bash
# Tek bir başlangıç commit olmalı
cd PROJECT_DIR
git log --oneline | wc -l
# > 1 ise, geçmiş temizlenmemiş — FAIL

# Olası secrets ara
git log -p | grep -iE '(password|secret|api.?key|token)' | head -20
```

### Çıktı Formatı

Proje dizininde `SANITIZATION_REPORT.md` oluştur:

```markdown
# Sanitization Report: {project-name}

**Date:** {date}
**Auditor:** opensource-sanitizer v1.0.0
**Verdict:** PASS | FAIL | PASS WITH WARNINGS

## Summary

| Category | Status | Findings |
|----------|--------|----------|
| Secrets | PASS/FAIL | {count} findings |
| PII | PASS/FAIL | {count} findings |
| Internal References | PASS/FAIL | {count} findings |
| Dangerous Files | PASS/FAIL | {count} findings |
| Config Completeness | PASS/WARN | {count} findings |
| Git History | PASS/FAIL | {count} findings |

## Critical Findings (Must Fix Before Release)

1. **[SECRETS]** `src/config.py:42` — Hardcoded database password: `DB_P...` (truncated)

## Warnings (Review Before Release)

1. **[CONFIG]** `src/app.py:8` — Port 8080 hardcoded, should be configurable

## .env.example Audit

- Variables in code but NOT in .env.example: {list}
- Variables in .env.example but NOT in code: {list}

## Recommendation

{If FAIL: "Fix the {N} critical findings and re-run sanitizer."}
{If PASS: "Project is clear for open-source release. Proceed to packager."}
{If WARNINGS: "Project passes critical checks. Review {N} warnings before release."}
```

### Kurallar

- **Asla** tam secret değeri gösterme - ilk 4 karakter + "..." ile kısalt
- **Asla** kaynak dosyaları değiştirme - sadece rapor oluştur (SANITIZATION_REPORT.md)
- **Her zaman** her metin dosyasını tara, sadece bilinen uzantıları değil
- **Her zaman** git geçmişini kontrol et, yeni depo olsa bile
- **Paranoyak kal** - Yanlış pozitif kabul edilebilir, yanlış negatif kabul edilemez
- Herhangi bir kategoride tek KRİTİK bulgu = Genel FAIL
- Sadece uyarılar = PASS WITH WARNINGS (kullanıcı karar verir)
[Geri Dön Ajan Dizini](../README.md)
