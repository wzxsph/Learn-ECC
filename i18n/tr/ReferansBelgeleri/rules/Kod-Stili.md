# Kod Stili Kuralları

## Kural Genel Bakış

ECC Rules Kod Stili kuralları, kod organizasyonu için temel prensipleri ve en iyi uygulamaları tanımlar. Bu kurallar kodun okunabilir, bakımı yapılabilir ve tutarlı kalmasını sağlar; değiştirilemezlikten adlandırma kurallarına kadar her yönü kapsar.

## Temel Gereksinimler

### Değiştirilemezlik (Kritik Prensip)

**Zorunlu**: Her zaman yeni nesneler oluşturun, mevcut nesneleri asla değiştirmeyin.

```typescript
# Hatalı Örnek - Orijinal nesneyi değiştiriyor
function modify(user, name) {
  user.name = name;  # Orijinal nesneyi değiştirdi
  return user;
}

# Doğru Örnek - Yeni nesne döndürüyor
function update(user, name) {
  return { ...user, name };  # Yeni kopya oluşturuyor
}
```

**Gerekçe**: Değiştirilemez veri gizli yan etkileri önler, hata ayıklamayı kolaylaştırır ve güvenli eşzamanlılığı destekler.

### Temel Tasarım Prensipleri

| Prensip | Açıklama | Uygulama |
|------|------|------|
| **KISS** | Keep It Simple | En basit çalışan çözümü tercih edin, erken optimizasyondan kaçının, açıklığı cambazlığa tercih edin |
| **DRY** | Don't Repeat Yourself | Tekrarlayan mantığı paylaşılan fonksiyonlara veya araçlara çıkarın, kopyala-yapıştır uygulama kaymasından kaçının |
| **YAGNI** | You Aren't Gonna Need It | Gelecekte gerekebilecek özellikler veya soyutlamalar inşa etmeyin, basit başlayın, gerçekten gerekli olduğunda yeniden yapılandırın |

### Dosya Organizasyonu

| Gereksinim | Açıklama |
|------|------|
| Çok küçük dosya > Az büyük dosya | Yüksek cohesion, düşük coupling |
| Tipik satır sayısı | 200-400 satır, maksimum 800 satır |
| Büyük modülleri ayır | Büyük modüllerden araç fonksiyonları çıkar |
| Organizasyon şekli | Tür yerine işlev/alan bazında organize et |

### Hata Yönetimi

| Gereksinim | Açıklama |
|------|------|
| Açıkça ele al | Her katmanda hataları açıkça ele al |
| UI kodları | Kullanıcı dostu hata mesajları sağla |
| Sunucu tarafı | Ayrıntılı hata bağlamını günlüğe kaydet |
| Yasaklanmış davranış | Hataları sessizce yutmayın |

### Girdi Doğrulama

| Gereksinim | Açıklama |
|------|------|
| Sınır doğrulama | Tüm girdileri sistem sınırlarında doğrula |
| Schema doğrulama | Mümkün olduğunda schema tabanlı doğrulama kullan |
| Hızlı başarısız ol | Açık hata mesajlarıyla hızlı başarısız ol |
| Harici veriler | Harici verilere asla güvenme (API yanıtları, kullanıcı girdileri, dosya içerikleri) |

## Uygulama Detayları

### Adlandırma Kuralları

| Tür | Kural | Örnek |
|------|------|------|
| Değişkenler ve fonksiyonlar | camelCase + Açıklayıcı ad | `calculateTotal`, `userName` |
| Booleanlar | is/has/should/can ön ekleri | `isActive`, `hasPermission` |
| Arayüzler, türler, bileşenler | PascalCase | `UserProfile`, `ButtonComponent` |
| Sabitler | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_TIMEOUT` |
| Özel hooklar | camelCase + use ön eki | `useUserData`, `useFetch` |

### Kod Kokuları

#### Derin İç İçe Geçme

4 seviyeden fazla iç içe geçme erken return kullanarak yeniden yapılandırılmalı:

```typescript
# Kaçının
function processOrder(order) {
  if (order) {
    if (order.items) {
      if (order.items.length > 0) {
        # İşleme mantığı
      }
    }
  }
}

# Önerilen
function processOrder(order) {
  if (!order || !order.items || order.items.length === 0) {
    return;
  }
  # İşleme mantığı
}
```

#### Büyülü Sayılar

Büyülü sayılar için adlandırılmış sabitler kullan:

```typescript
# Kaçının
setTimeout(callback, 300000);

# Önerilen
const SESSION_TIMEOUT_MS = 5 * 60 * 1000; // 5 dakika
setTimeout(callback, SESSION_TIMEOUT_MS);
```

#### Uzun Fonksiyonlar

Büyük fonksiyonları net sorumluluklara sahip küçük fonksiyonlara böl:

| Fonksiyon uzunluğu | Öneri |
|----------|------|
| <50 satır | İdeal |
| 50-100 satır | Kabul edilebilir, ayırmayı düşün |
| >100 satır | Ayırmak zorunlu |

## İhlal İşleme

### Kod Kalitesi Kontrol Listesi

İşi bitirmeden önce zorunlu kontrol:

- [ ] Kod okunabilir ve iyi adlandırılmış
- [ ] Fonksiyonlar küçük (<50 satır)
- [ ] Dosyalar odaklı (<800 satır)
- [ ] Derin iç içe geçme yok (>4 seviye)
- [ ] Uygun hata yönetimi
- [ ] Sabit kodlanmış değer yok (sabit veya yapılandırma kullan)
- [ ] Değiştirilemez örüntüler kullan (mutation yok)

### Şiddet Seviyesi Derecelendirmesi

| Seviye | Anlamı | Eylem |
|------|------|------|
| CRITICAL | Güvenlik açıkları veya veri kaybı riski | **Engelle** - Birleştirmeden önce düzeltme zorunlu |
| HIGH | Hata veya ciddi kalite sorunu | **Uyarı** - Birleştirmeden önce düzeltilmeli |
| MEDIUM | Bakım sorunu | **İpucu** - Düzeltme önerilir |
| LOW | Stil veya küçük öneri | **Not** - İsteğe bağlı |

## İlgili Kurallar

| İlgili kural | Açıklama |
|----------|------|
| Test Kuralları | Test kapsamı gereksinimleri |
| Güvenlik Kuralları | Güvenlik kontrol kalemleri |
| Kod İnceleme kuralları | Detaylı inceleme standartları |
| Geliştirme iş akışı | TDD ve kod inceleme süreçleri |
