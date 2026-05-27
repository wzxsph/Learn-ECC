# Test Ajanı

Test Ajanı, Test Güdümlü Geliştirme, Uçtan Uca Test, test kapsam analizi ve kalite güvencesinden sorumludur.

## Ajan Listesi

| Ajan Adı | Kullanım | Kullanım Modeli | Temel Araçlar |
|------------|------|----------|----------|
| tdd-guide | TDD Test Güdümlü Geliştirme uzmanı | sonnet | Read, Write, Edit, Bash, Grep |
| e2e-runner | Uçtan Uca Test uzmanı | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| mle-reviewer | ML mühendisliği kod inceleme (eğitim/çıkarım/izleme) | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | PR test kapsam analizi | sonnet | Read, Grep, Glob, Bash |

---

## tdd-guide

### Ad ve Kullanım
Test Güdümlü Geliştirme uzmanı, test öncelikli metodolojiyi zorunlu kılar. Yeni özellikler yazarken, hataları düzeltirken veya kodu yeniden düzenlerken proaktif olarak kullanılır. %80+ test kapsamını garanti eder.

### Yetenekler
- Test öncelikli metodolojiyi zorunlu kılma
- Kırmızı-Yeşil-Yeniden Düzenle döngüsünü yönetme
- %80+ test kapsamını sağlama
- Kapsamlı test süiti yazma (birim, entegrasyon, E2E)
- Uygulamadan önce kenar durumlarını yakalama

### Kullanım Senaryosu
- Yeni özellik yazarken
- Hata düzeltirken
- kodu yeniden düzenlerken
- TDD iş akışı gerekli olduğunda

### Kullanılan Araçlar Listesi
- Read: Mevcut kodu oku
- Write: Test yaz
- Edit: Testleri ve uygulamayı değiştir
- Bash: Test komutlarını çalıştır
- Grep: Test desenlerini ara

### Diğer Ajanlarla İşbirliği
- TDD döngüsünden sonra code-reviewer kodu inceler
- Test yapı hatalarını düzeltmek için build-error-resolver kullan
- Kritik kullanıcı akışları için E2E testi yürütmek için e2e-runner kullan

### TDD İş Akışı

#### 1. Önce Test Yaz (KIRMIZI)
Beklenen davranışın başarısız testini yaz.

#### 2. Testi Çalıştır - Başarısız Olduğunu Doğrula
```bash
npm test
```

#### 3. Minimum Uygulama Yaz (YEŞİL)
Testin geçmesi için yeterli kodu yaz.

#### 4. Testi Çalıştır - Geçtiğini Doğrula

#### 5. Yeniden Düzenle (İYİLEŞTİR)
Tekrarları kaldır, adları iyileştir, optimize et - Test zorunlu yeşil kalmalı.

#### 6. Kapsamı Doğrula
```bash
npm run test:coverage
# Gereksinim: %80+ dal, fonksiyon, satır, ifade kapsamı
```

### Zorunlu Test Kenar Durumları

1. **Null/Undefined** giriş
2. **Boş** dizi/string
3. **Geçersiz tür** geçirme
4. **Sınır değerleri** (minimum/maksimum)
5. **Hata yolları** (ağ hataları, veritabanı hataları)
6. **Yarış koşulları** (eşzamanlı işlemler)
7. **Büyük veri** (10k+ öğe performansı)
8. **Özel karakterler** (Unicode, emojiler, SQL karakterleri)

### Anti-Desenler - Zorunlu Kaçınılmalı

- Testin detayları (iç durum) yerine davranışını test etme
- Testlerin birbirine bağımlı olması (paylaşılan durum)
- Çok az assert (geçiyor ama hiçbir şeyi doğrulamıyor)
- Harici bağımlılıklar mocklanmamış (Supabase, Redis, OpenAI vb.)

### Kalite Kontrol Listesi

- [ ] Tüm genel fonksiyonların birim testi var
- [ ] Tüm API uç noktalarının entegrasyon testi var
- [ ] Kritik kullanıcı akışlarının E2E testi var
- [ ] Kenar durumları ele alınmış (null, boş, geçersiz)
- [ ] Hata yolları test edilmiş (sadece mutlu yol değil)
- [ ] Harici bağımlılıklar mocklanmış
- [ ] Testler birbirinden bağımsız (paylaşılan durum yok)
- [ ] Assertler spesifik ve anlamlı
- [ ] Kapsam %80+

### v1.8 Eval-Driven TDD Eki

Eval-driven geliştirmeyi TDD sürecine entegre et:

1. Uygulamadan önce yetenek + regresyon evallerini tanımla
2. Baseline çalıştır ve başarısızlık karakteristiklerini yakala
3. Minimum değişikliği uygula
4. Testleri ve evalleri yeniden çalıştır; pass@1 ve pass@3 raporla
5. Yayın kritik yollar birleştirmeden önce pass^3 stabiliteye ulaşmalı

---

## e2e-runner

### Ad ve Kullanım
Vercel Ajan Browser (tercih edilen) ve Playwright yedek kullanarak Uçtan Uca Test uzmanı. E2E testleri proaktif olarak oluşturur, bakımını yapar ve çalıştırır.

### Yetenekler
- Test yolculuğu oluşturma - kullanıcı akışları için test yazma
- Test bakımı - testleri UI değişiklikleriyle senkronize tutma
- Kararsız test yönetimi - kararsız testleri tanımlama ve izolasyon
- Test raporlama - HTML raporları ve JUnit XML oluşturma
- CI/CD entegrasyonu - testlerin pipeline'da güvenilir çalışmasını sağlama
- Screenshot/video/trace yönetimi

### Kullanım Senaryosu
- Kritik kullanıcı yolculuklarının doğrulanması gerektiğinde
- Uçtan uca doğrulama gerekli olduğunda
- CI/CD pipeline'ında E2E testleri çalıştırırken
- Entegrasyon sorunları tespit edildiğinde

### Kullanılan Araçlar Listesi
- Read: Sayfa içeriğini oku
- Write: Test yaz
- Edit: Testleri değiştir
- Bash: Playwright/Ajan Browser komutlarını çalıştır
- Grep: Testleri ara
- Glob: Test dosyalarını bul

### Diğer Ajanlarla İşbirliği
- tdd-guide birim/entegrasyon testleri yazar
- code-reviewer test kalitesini inceler
- mle-reviewer ML ile ilgili testleri inceler

### Ana Araç: Ajan Browser

**Ajan Browser'ı ham Playwright yerine tercih et** - Anlamsal seçiciler, AI optimizasyonu, otomatik bekleme, Playwright tabanlı.

```bash
# Kurulum
npm install -g agent-browser && agent-browser install

# Temel İş Akışı
agent-browser open https://example.com
agent-browser snapshot -i          # Refs ile öğeleri al [ref=e1]
agent-browser click @e1            # Ref ile tıkla
agent-browser fill @e2 "text"     # Ref ile girişi doldur
agent-browser wait visible @e5     # Öğe görünür olana kadar bekle
agent-browser screenshot result.png
```

### Yedek: Playwright

Ajan Browser kullanılamadığında, Playwright'ı doğrudan kullan.

```bash
npx playwright test                        # Tüm E2E testlerini çalıştır
npx playwright test tests/auth.spec.ts     # Belirli dosyayı çalıştır
npx playwright test --headed               # Görünür tarayıcı
npx playwright test --debug                # Denetçi ile debug
npx playwright test --trace on             # Trace ile çalıştır
npx playwright show-report                 # HTML raporunu görüntüle
```

### İş Akışı

#### 1. Planlama
- Kritik kullanıcı yolculuklarını belirle (kimlik doğrulama, çekirdek özellikler, ödeme, CRUD)
- Senaryoları tanımla: mutlu yol, kenar durumlar, hata senaryoları
- Risk'e göre sırala: YÜKSEK (maliyet, kimlik), ORTA (arama, navigasyon), DÜŞÜK (UI optimizasyonu)

#### 2. Oluşturma
- Sayfa nesne modeli (POM) deseni kullan
- `data-testid` seçicileri tercih et
- Kritik adımlara assert ekle
- Kritik noktalarda screenshot al
- Uygun bekleme kullan (asla `waitForTimeout` kullanma)

#### 3. Yürütme
- Kararsızlığı kontrol etmek için yerel olarak 3-5 kez çalıştır
- Kararsız testleri izole etmek için `test.fixme()` veya `test.skip()` kullan
- Artifact'leri CI'ya yükle

### Temel İlkeler

- **Anlamsal seçiciler kullan**: `[data-testid="..."]` > CSS seçiciler > XPath
- **Zaman yerine koşul bekle**: `waitForResponse()` > `waitForTimeout()`
- **Otomatik bekleme dahili**: `page.locator().click()` otomatik bekler; ham `page.click()` beklemez
- **Testleri izole et**: Her test bağımsız olmalı; paylaşılan durum yok
- **Hızlı başarısız ol**: Her kritik adımda `expect()` assert kullan
- **Yeniden denemede trace**: Başarısızlıkları debug etmek için `trace: 'on-first-retry'` yapılandır

### Kararsız Test İşleme

```typescript
// İzolasyon
test('kararsız: pazar araması', async ({ page }) => {
  test.fixme(true, 'Kararsız - Issue #123')
})

// Kararsızlığı tanımlama
// npx playwright test --repeat-each=10
```

 yaygın nedenler: Yarış koşulları (otomatik bekleme seçicileri kullan), ağ zamanlaması (yanıtı bekle), animasyon zamanlaması (`networkidle` bekle).

### Başarı Metrikleri

- Tüm kritik yolculuklar geçiyor (%100)
- Genel geçme oranı > %95
- Kararsızlık oranı < %5
- Test süresi < 10 dakika
- Artifactler yüklü ve erişilebilir

---

## mle-reviewer

### Ad ve Kullanım
Üretim makine öğrenmesi mühendisliği inceleme uzmanı, veri sözleşmeleri, özellik boru hatları, eğitim tekrarlanabilirliği, çevrimdışı/çevrimiçi değerlendirme, model servis, izleme ve geri alma konularında uzmanlaşmıştır.

### Yetenekler
- Problem tanımı ve karar kalitesi incelemesi
- Metrik, eşik ve hata analizi incelemesi
- Veri sözleşmesi ve sızıntı kontrolü
- Eğitim tekrarlanabilirliği doğrulaması
- Değerlendirme ve yayın süreci incelemesi
- Servis ve dağıtım güvenlik incelemesi
- İzleme ve olay müdahale planlaması

### Kullanım Senaryosu
- ML, MLOps, model eğitim kodu değiştiğinde
- Çıkarım, özellik deposu, değerlendirme kodu değiştiğinde
- Model yayın kararlarında

### Kullanılan Araçlar Listesi
- Read: ML kodunu ve yapılandırmasını oku
- Grep: Desenleri ara
- Glob: Dosyaları bul
- Bash: pytest, ruff, mypy çalıştır

### Diğer Ajanlarla İşbirliği
- python-reviewer Python stilini, türleri, hata yönetimini işler
- pytorch-build-resolver tensor/CUDA/eğitim başarısızlıklarını işler
- database-reviewer özellik tablolarını, etiket depolama işler
- security-reviewer secrets, PII, pickle güvenliğini işler
- performance-optimizer gecikme, bellek, GPU kullanımını işler
- build-error-resolver CI, bağımlılık, native extension başarısızlıklarını işler
- pr-test-analyzer test kapsamını analiz eder
- silent-failure-hunter pipeline sessiz başarısızlıklarını bulur
- e2e-runner ürün akışı E2E testi yürütür
- a11y-architect tahmin edilebilirliği inceler
- doc-updater dokümantasyonu günceller

### Kritik İnceleme Alanları

#### Problem tanımı ve karar kalitesi
- Değişiklik kullanıcı veya sistem kararından mı başlıyor, model mimari tercihinden mi
- Paydaşlar ve başarısızlık maliyetleri açık mı
- Metrik seçimi hata bütçesini takip ediyor mu

#### Metrikler, eşikler ve hata analizi
- Baseline ve mevcut üretim davranışı karşılaştırılıyor mu
- Eşikler ve yapılandırma ürün kararı olarak mı ele alınıyor
- Yanlış pozitifler ve yanlış negatifler doğrudan kontrol ediliyor mu

#### Veri sözleşmeleri ve sızıntı
- Varlık granülerliği, birincil anahtarlar, zaman damgaları açık mı
- Bölme zaman, kullanıcı/varlık gruplamalarına uyuyor mu
- Özellik birleştirmeleri zaman noktası doğru mu
- Hassas özellikler hariç tutulmuş veya meşru şekilde işlenmiş mi

#### Eğitim tekrarlanabilirliği
- Eğitim koddan, yapılandırmadan, veri kümesi sürümlerinden, seed'lerden çalışabilir mi
- Hiperparametreler, ön işleme, bağımlılık sürümleri kaydedilmiş mi
- Randomness ve GPU belirsizliği kasıtlı olarak mı işleniyor

#### Değerlendirme ve yayın
- Metrikler baseline ve mevcut üretim modeliyle karşılaştırılıyor mu
- Yayın kapıları seçimden önce mi beyan ediliyor
- Dilim metrikleri önemli kuyrukları kapsıyor mu

#### Servis ve dağıtım
- Eğitim ve servis dönüşümleri paylaşılan veya eşdeğer testlere sahip mi
- Giriş şeması eski, eksik, geçersiz özellikleri reddediyor mu
- Çıkarım yolu zaman aşımı, kaynak sınırları, geri dönüş mantığına sahip mi

#### İzleme ve olay müdahalesi
- İzleme servis sağlığını, özellik kaymasını, tahmin kaymasını kapsıyor mu
- Günlükler yeterli tanımlayıcılar içeriyor mu
- Geri alma önceki artifact, yapılandırma, veri bağımlılıklarını adlandırıyor mu

### Yaygın Engelleyiciler

- Zamanla veya kullanıcıyla ilgili verilerde rastgele train/test bölme
- Özellik oluşturma tahmin sırasında kullanılamayan alanları kullanma
- Kritik dilimler regresyon yaparken çevrimdışı metrikler iyileşiyor
- Eğitim ön işleme manuel olarak servis koduna kopyalanıyor
- Model sürümleri tahmin günlüklerinde yok

### Onay Kriterleri

- **ONAYLA**: Kritik/yüksek MLE riski yok, ilgili test veya değerlendirme kapıları geçiyor
- **UYARILARLA ONAYLA**: Yalnızca orta düzey sorunlar, net sonraki eylemler var
- **BLOKLA**: Herhangi bir olası sızıntı, tekrarlanamayan yayın, güvensiz servis davranışı, eksik üretim dağıtım geri alma, hassas veri ifşası veya kritik değerlendirme boşluğu

---

## pr-test-analyzer

### Ad ve Kullanım
Pull Request test kapsam kalitesini ve bütünlüğünü, davranış kapsamı ve gerçek hata önleme vurgusuyla inceler.

### Yetenekler
- Değişiklik kodu haritalama
- Davranış kapsamını doğrulama
- Test kalitesini değerlendirme
- Kapsam boşluklarını keşfetme

### Kullanım Senaryosu
- PR incelenirken
- Test kapsamı değerlendirilirken
- Test yeterliliği doğrulanırken

### Kullanılan Araçlar Listesi
- Read: Değişiklik kodunu ve testleri oku
- Grep: Testleri ara
- Glob: Test dosyalarını bul
- Bash: Test komutlarını çalıştır

### Diğer Ajanlarla İşbirliği
- tdd-guide testleri iyileştirir
- e2e-runner uçtan uca kapsam yürütür
- code-reviewer kod kalitesi incelemesi yapar

### Analiz Süreci

#### 1. Değişiklik Kodunu Tanımla
- Değişen fonksiyonları, sınıfları ve modülleri haritala
- İlgili testleri bul
- Yeni test edilmemiş kod yollarını tanımla

#### 2. Davranış Kapsamı
- Her özelliğin testi olup olmadığını kontrol et
- Kenar durumlarını ve hata yollarını doğrula
- Önemli entegrasyonların kapsandığından emin ol

#### 3. Test Kalitesi
- Anlamsız assert yerine fırlatmayan kontroller yerine anlamlı assertleri tercih et
- Kararsız desenleri işaretle
- İzolasyonu ve test adı netliğini kontrol et

#### 4. Kapsam Boşlukları
Etki derecesine göre:
- kritik (kritik)
- önemli (önemli)
- güzel-olsun (eklentsi)

### Çıktı Formatı

1. Kapsam özeti
2. Kritik boşluklar
3. İyileştirme önerileri
4. Pozitif gözlemler
[Geri Dön Ajan Dizini](../README.md)
