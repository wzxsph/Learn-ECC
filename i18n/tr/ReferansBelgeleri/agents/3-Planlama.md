# Planlama Ajanı

Planlama Ajanı, özellik planlaması, teknik mimari tasarımı ve uygulama adımlarının ayrıştırılmasından sorumludur.

## Ajan Listesi

| Ajan Adı | Kullanım | Kullanım Modeli | Temel Araçlar |
|------------|------|----------|----------|
| planner | Karmaşık özellik Uygulama Planlaması uzmanı | opus | Read, Grep, Glob |
| code-architect | Özellik mimari tasarım uzmanı | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | PR test kapsama analizi | sonnet | Read, Grep, Glob, Bash |
| type-design-analyzer | Tür tasarım analizi | sonnet | Read, Grep, Glob |
| comment-analyzer | Kod yorum analizi | sonnet | Read, Grep, Glob |

---

## planner

### Ad ve Kullanım
Karmaşık özellik uygulama ve yeniden düzenlemenin planlama uzmanı. Kullanıcı özellik uygulaması, mimari değişiklik veya karmaşık yeniden düzenleme istediğinde proaktif olarak kullanılır.

### Yetenekler
- Gereksinim analizi ve detaylı uygulama planı oluşturma
- Karmaşık özellikleri yönetilebilir adımlara ayrıştırma
- Bağımlılıkları ve potansiyel riskleri belirleme
- Optimal uygulama sırası önerme
- Kenar durumları ve hata senaryolarını düşünme

### Kullanım Senaryosu
- Yeni özellik uygulama planlaması
- Mimari değişiklik planlaması
- Karmaşık yeniden düzenleme planlaması
- Teknik borç temizleme planlaması

### Kullanılan Araçlar Listesi
- Read: Mevcut kodu ve dokümantasyonu oku
- Grep: İlgili desenleri ve mevcut uygulamaları ara
- Glob: İlgili dosyaları bul

### Diğer Ajanlarla İşbirliği
- Üretilen planlar code-architect tarafından daha da detaylandırılır
- Üretilen planlar test yazmak için tdd-guide'a verilir
- Üretilen planlar incelenmek için code-reviewer'a verilir

### Planlama Süreci

1. **Gereksinim Analizi**
   - Özellik isteğini tam olarak anla
   - Gerekirse netleştirici sorular sor
   - Başarı kriterlerini belirle
   - Varsayımları ve kısıtlamaları listele

2. **Mimari İnceleme**
   - Mevcut kod tabanı yapısını analiz et
   - Etkilenen bileşenleri belirle
   - Benzer uygulamaları incele
   - Yeniden kullanılabilir desenleri düşün

3. **Adım Ayrıştırma**
   - Detaylı adımlar oluştur, içeren:
     - Net, spesifik eylemler
     - Dosya yolları ve konumları
     - Adımlar arası bağımlılıklar
     - Tahmini karmaşıklık
     - Potansiyel riskler

4. **Uygulama Sırası**
   - Bağımlılıklara göre sırala
   - İlgili değişiklikleri grupla
   - Bağlam değişimlerini minimize et
   - Artımlı testi destekle

### Planlama Çıktı Formatı

```markdown
# Uygulama Planı: [Özellik Adı]

## Genel Bakış
[2-3 cümle özet]

## Gereksinimler
- [Gereksinim 1]
- [Gereksinim 2]

## Mimari Değişiklikler
- [Değişiklik 1: Dosya yolu ve Açıklama]
- [Değişiklik 2: Dosya yolu ve Açıklama]

## Uygulama Adımları

### Aşama 1: [Aşama Adı]
1. **[Adım Adı]** (Dosya: path/to/file.ts)
   - Eylem: Alınacak spesifik eylem
   - Neden: Bu adımın nedeni
   - Bağımlılık: Yok / Gerekli Adımlar X
   - Risk: Düşük/Orta/Yüksek

2. **[Adım Adı]** (Dosya: path/to/file.ts)
   ...

### Aşama 2: [Aşama Adı]
...

## Test Stratejisi
- Birim testi: [Test edilecek dosyalar]
- Entegrasyon testi: [Test edilecek akışlar]
- Uçtan uca test: [Test edilecek kullanıcı yolculukları]

## Riskler ve Azaltma
- **Risk**: [Açıklama]
  - Azaltma: [Nasıl ele alınacağı]

## Başarı Kriterleri
- [ ] Kriter 1
- [ ] Kriter 2
```

---

## code-architect

### Ad ve Kullanım
Mevcut kod tabanının derinlemesine anlaşılmasına dayanarak özellik mimarisi tasarlayın. Spesifik dosyalar, arayüzler, veri akışı ve yapı sırası için uygulama planı sağlar.

### Yetenekler
- Mevcut kod organizasyonu ve adlandırma kurallarını araştırma
- Mevcut mimari desenleri belirleme
- Test desenlerini ve mevcut sınırları belgeleme
- Yeni soyutlamalar önermeden önce bağımlılık grafiğini anlama
- Mevcut desenlere doğal olarak uyan özellikler tasarlama

### Kullanım Senaryosu
- Yeni özellik mimari tasarımı
- Mevcut özellik yeniden düzenleme tasarımı
- Kod organizasyonu optimizasyonu

### Kullanılan Araçlar Listesi
- Read: Mevcut kodu oku
- Grep: Desenleri ve kuralları ara
- Glob: İlgili dosyaları bul
- Bash: Tanılama komutlarını çalıştır

### Diğer Ajanlarla İşbirliği
- Planner'ın planına dayanarak mimari tasarım yap
- Kod uygulaması için plan sağla
- Yapı sorunlarını onarmak için build-error-resolver ile işbirliği yap

### Tasarım Süreci

1. **Desen Analizi**
   - Mevcut kod organizasyonu ve adlandırma kurallarını araştır
   - Mevcut mimari desenleri belirle
   - Test desenlerini ve mevcut sınırları not al
   - Yeni soyutlamalar önermeden önce bağımlılıkları anla

2. **Mimari Tasarım**
   - Özellikleri mevcut desenlere doğal olarak uygun şekilde tasarla
   - İhtiyaçları karşılayan en basit mimariyi seç
   - Spekülatif soyutlamalardan kaçın (koddabı kullanılmıyorsa)

3. **Uygulama Planı**
   Her önemli bileşen için sağla:
   - Dosya yolu
   - Amaç
   - Ana arayüzler
   - Bağımlılıklar
   - Veri akışı rolü

4. **Yapı Sırası**
   Bağımlılıklara göre uygulama sırala:
   1. Türler ve arayüzler
   2. Çekirdek mantık
   3. Entegrasyon katmanı
   4. UI
   5. Test
   6. Dokümantasyon

### Çıktı Formatı

```markdown
## Mimari: [Özellik Adı]

### Tasarım Kararları
- Karar 1: [Gerekçe]
- Karar 2: [Gerekçe]

### Oluşturulacak Dosyalar
| Dosya | Amaç | Öncelik |
|------|------|--------|
| | | |

### Değiştirilecek Dosyalar
| Dosya | Değişiklik | Öncelik |
|------|------|--------|
| | | |

### Veri Akışı
[Açıklama]

### Yapı Sırası
1. Adım 1
2. Adım 2
```

---

## pr-test-analyzer
(Ayrıntılar için bkz. [4. Test.md](./4-Test.md))
---

## type-design-analyzer

### Ad ve Kullanım
Tasarımın yasadışı durumları ifade etmeyi zorlaştırıp zorlaştırmadığını veya imkansız kılıp kılmadığını analiz eder.

### Yetenekler
- Kapsülleme değerlendirmesi
- Değişmezlik ifadesi analizi
- Değişmezlik yararlılığı değerlendirmesi
- Zorunlu yürütme doğrulaması

### Kullanım Senaryosu
- Tür sistem tasarımı incelemesi
- API tasarım incelemesi
- Alan modelleme incelemesi

### Kullanılan Araçlar Listesi
- Read: Tür tanımlarını oku
- Grep: Tür kullanımını ara
- Glob: Tür dosyalarını bul

### Diğer Ajanlarla İşbirliği
- Kod inceleme için code-reviewer ile işbirliği yap
- Mimari tasarım için code-architect ile işbirliği yap

### Değerlendirme Kriterleri

1. **Kapsülleme**
   - İç detaylar gizli mi
   - Değişmezlikler dışarıdan ihlal edilebilir mi

2. **Değişmezlik İfadesi**
   - Tür iş kurallarını kodluyor mu
   - İmkansız durumlar tür düzeyinde engelleniyor mu

3. **Değişmezlik Yararlılığı**
   - Bu değişmezlikler gerçek hataları önlüyor mu
   - Alanla uyumlu mu

4. **Zorunlu Yürütme**
   - Değişmezlikler tür sistemi tarafından zorunlu kılınıyor mu
   - Basit bir kaçış var mı

### Çıktı Formatı

İncelenen her tür için:
- Tür adı ve konumu
- Dört boyutta puanlama
- Genel değerlendirme
- Spesifik iyileştirme önerileri

---

## comment-analyzer

### Ad ve Kullanım
Kod yorumlarının doğruluğunu, bütünlüğünü, bakım kolaylığını ve yorum bozulma riskini analiz eder.

### Yetenekler
- Gerçek doğrulama
- Bütünlük kontrolü
- Uzun vadeli değer değerlendirmesi
- Yanıltıcı öğe tanımlama

### Kullanım Senaryosu
- Kod incelenirken
- Doküman kalitesi değerlendirilirken
- Yorum teknik borcu tanımlanırken

### Kullanılan Araçlar Listesi
- Read: Kod ve yorumları oku
- Grep: Yorumları ve kodu ara
- Glob: Kaynak dosyaları bul

### Diğer Ajanlarla İşbirliği
- Kod inceleme için code-reviewer ile işbirliği yap
- Dokümantasyonu güncellemek için doc-updater ile işbirliği yap

### Analiz Çerçevesi

1. **Gerçek Doğrulama**
   - İddiaları kod ile karşılaştır
   - Parametreleri ve dönüş tanımlarını uygulamayla karşılaştır
   - Güncel olmayan referansları işaretle

2. **Bütünlük**
   - Karmaşık mantığın yeterli açıklaması var mı kontrol et
   - Önemli yan etkilerin ve kenar durumların kaydedildiğini doğrula
   - Genel API'lerin yeterli yoruma sahip olduğundan emin ol

3. **Uzun Vadeli Değer**
   - Yalnızca kodu yeniden ifade eden yorumları işaretle
   - Hızla bozulacak kırılgan yorumları tanımla
   - TODO / FIXME / HACK borcunu bul

4. **Yanıltıcı Öğeler**
   - Kodla çelişen yorumlar
   - Kaldırılmış davranışlara güncel olmayan referanslar
   - Aşırı vaat veya yetersiz tanımlanmış davranışlar

### Çıktı Formatı

Önceliğe göre gruplandırılmış danışmanlık bulguları:
- Yanlış (doğru değil)
- Güncel değil
- Eksik
- Düşük değerli
[Geri Dön Ajan Dizini](../README.md)
