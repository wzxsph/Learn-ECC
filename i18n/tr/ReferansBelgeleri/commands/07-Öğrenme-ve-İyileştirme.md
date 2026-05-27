# Öğrenme ve İyileştirme Komutları

## Genel Bakış

Öğrenme ve iyileştirme komutları, oturumlardan desenleri çıkarmak, içgüdüleri takip etmek ve organizasyonel bilgiyi yönetmek için kullanılır.

## Komut Listesi

### /learn

**Kullanım**: Mevcut oturumdan yeniden kullanılabilir desenleri çıkarır ve beceri olarak kaydeder

**Tanım**: Mevcut oturumu analiz eder, beceri olarak kaydetmeye değer desenleri çıkarır. `/learn` oturum sırasında herhangi bir zamanda kullanılabilir.

**Çıkarılan İçerik Türleri**:

| Tür | Açıklama |
|---|---|
| **Hata çözümleme desenleri** | Hangi hatalar? Temel sebep ne? Ne düzeldi? Benzer hatalar yeniden kullanılabilir mi? |
| **Hata ayıklama teknikleri** | Önemsiz olmayan hata ayıklama adımları, etkili araç kombinasyonları, tanılama desenleri |
| **Geçici çözümler** | Kütüphane tuhaflıkları, API sınırlamaları, belirli sürüm düzeltmeleri |
| **Proje özgü desenler** | Keşfedilen kod tabanı kuralları, mimari kararlar, entegrasyon desenleri |

**Çıktı Formatı**: `~/.claude/skills/learned/[desen-adı].md` olarak kaydeder

```markdown
# [Açıklayıcı Desen Adı]

**Çıkarılan:** [Tarih]
**Bağlam:** [Ne zaman uygulandığının kısa açıklaması]

## Problem
[Bu desenin çözdüğü sorun - spesifik olun]

## Çözüm
[Desen/teknik/geçici çözüm]

## Örnek
[Geçerliyse kod örneği]

## Ne Zaman Kullanılır
[Tetikleme koşulları - bu beceriyi ne aktive etmeli]
```

**İş Akışı**:
1. Oturumdan çıkarılabilecek desenleri gözden geçir
2. En değerli/yeniden kullanılabilir içgörüleri tanımla
3. Beceri dosyası taslağı oluştur
4. Kaydetmeden önce kullanıcı onayı iste
5. `~/.claude/skills/learned/` dizinine kaydet

**En İyi Uygulamalar**:
- Önemsiz düzeltmeleri çıkarma (yazım hataları, basit sözdizimi hataları)
- Tek seferlik sorunları çıkarma (belirli API kesintileri vb.)
- Gelecek oturumlarda zaman kazandıracak desenlere odaklan
- Becerileri odaklı tut - bir desen bir beceri

---

### /learn-eval

**Kullanım**: Desen çıkarımı + öz-değerlendirme kalitesi

**Tanım**: Desenleri çıkarırken aynı zamanda kalite değerlendirmesi yapar.

---

### /evolve

**Kullanım**: İçgüdüleri analiz et + yapı evrim öner

**Tanım**: Öğrenilen içgüdüleri analiz eder, yapısal evrim önerileri sağlar.

---

### /promote

**Kullanım**: Proje içgüdülerini global kapsama yükselt

**Tanım**: Proje özgü içgüdüleri global olarak kullanılabilir bilgiye yükseltir.

---

### /instinct-status

**Kullanım**: Tüm öğrenilen içgüdülerin durumunu göster

**Tanım**: Mevcut tüm içgüdülerin durumunu ve güvenilirlik düzeyini gösterir.

---

### /instinct-export

**Kullanım**: İçgüdüleri dosyaya aktar

**Tanım**: İçgüdüleri paylaşılabilir dosya formatına aktarır.

---

### /instinct-import

**Kullanım**: Dosya/URL'den içgüdüleri içe aktar

**Tanım**: Dosya veya URL'den sistem içgüdülerini içe aktarır.

---

### /skill-create

**Kullanım**: Git geçmişini analiz et + beceri dosyası oluştur

**Tanım**: Proje git geçmişini analiz eder, yeniden kullanılabilir desenleri çıkarır ve beceri dosyaları oluşturur.

**İş Akışı**:
1. Git işlem geçmişini analiz et
2. Tekrarlanan desenleri tanımla
3. Beceri dosyaları oluştur
4. Doğrula ve kaydet

---

### /skill-health

**Kullanım**: Beceri kompozisyonu sağlık kontrol paneli

**Tanım**: Beceri kompozisyonunun sağlık durumunu ve kullanım istatistiklerini gösterir.

---

### /rules-distill

**Kullanım**: Becerileri tara + çapraz alan prensipleri çıkar

**Tanım**: Tüm becerileri tarar, çapraz alan genel prensipleri çıkarır.

---

## İlgili Komutlar

- `/learn` - Desen çıkar
- `/skill-create` - Git geçmişinden beceri oluştur
- `/instinct-status` - İçgüdü durumunu görüntüle
