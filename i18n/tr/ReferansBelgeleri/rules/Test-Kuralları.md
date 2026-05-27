# Test Kuralları

## Kural Genel Bakış

ECC Rules Test Kuralları, kod kalitesini sağlamak için zorunlu test standartlarını tanımlar. Bu kural, test güdümlü geliştirme (TDD), minimum kapsam gereksinimleri ve test yapısı özelliklerini kapsar; tüm kodların yeterince doğrulandığından emin olur.

## Temel Gereksinimler

### Minimum Test Kapsamı: %80

Üç test türünün hepsi zorunludur:

| Test türü | Açıklama | Kapsam alanı |
|----------|------|----------|
| Birim testleri | Bağımsız fonksiyonlar, araçlar, bileşenler | Bireysel birimler |
| Entegrasyon testleri | API uç noktaları, veritabanı işlemleri | Entegrasyon noktaları |
| Uçtan uca testler | Kritik kullanıcı akışları | Kritik kullanıcı akışları |

### Test Güdümlü Geliştirme (TDD)

**Zorunlu iş akışı**:

```
1. Test yaz (RED)   - Önce başarısız olan bir test yaz
2. Testi çalıştır      - Testin başarısız olduğunu doğrula
3. Minumum gerçekleştir yaz (GREEN) - Testi geçecek minimum kodu yaz
4. Testi çalıştır      - Testin geçtiğini doğrula
5. Yeniden yapılandır (IMPROVE)    - Kod yapısını iyileştir
6. Kapsamı doğrula        - %80+'a ulaşıldığından emin ol
```

## Uygulama Detayları

### AAA Örüntüsü (Arrange-Act-Assert)

Tüm testler AAA yapısını takip etmelidir:

```typescript
test('kosinüs benzerliğini doğru hesaplar', () => {
  # Arrange - Test verilerini hazırla
  const vector1 = [1, 0, 0];
  const vector2 = [0, 1, 0];

  # Act - Test edilen işlemi gerçekleştir
  const similarity = calculateCosineSimilarity(vector1, vector2);

  # Assert - Sonucu doğrula
  expect(similarity).toBe(0);
});
```

### Test Adlandırma Kuralları

Test edilen davranışı açıklayan tanımlayıcı isimler kullan:

```typescript
# Önerilen - Beklenen davranışı açıkla
test('sorguya uyan piyasa olmadığında boş dizi döndür', () => {});
test('API anahtarı eksik olduğunda hata fırlatır', () => {});
test('Redis kullanılamadığında alt dize aramasına geri döner', () => {});

# Kaçının - Belirsiz adlandırma
test('test1', () => {});
test('edge case', () => {});
```

### Test Hata Giderme

Test başarısız olduğunda:

| Adımlar | İşlem |
|------|------|
| 1 | **tdd-guide** aracısını kullan |
| 2 | Test izolasyonunu kontrol et |
| 3 | Mock'ların doğru olduğunu doğrula |
| 4 | Uygulamayı düzelt, Testleri değil (testlerin kendisi hatalı değilse) |

### Aracı Desteği

| Aracı | Amaç | Kullanım Zamanı |
|-------|------|----------|
| **tdd-guide** | Test güdümlü geliştirme rehberliği | Yeni özellik geliştirme, hata düzeltme sırasında zorunlu kullanım |
| **code-reviewer** | Kod inceleme | Kod yazımı tamamlandıktan sonra |

## İhlal İşleme

### Yetersiz Kapsam

- %80'in altında kapsamı olan kod birleştirilmemeli
- CI/CD pipeline düşük kapsamlı kod commitlerini engellemeli
- Kapsam raporu oluşturmak için `c8` veya benzeri araçlar kullan

### Test Başarısızlığı İşleme

```
1. Başarısızlık nedenini analiz et
2. Uygulama mı yoksa test mi sorunlu olduğunu tanımla
3. Uygulama sorunuysa - Uygulama kodunu düzelt
4. Test sorunuysa - Test kodunu düzelt
5. Tüm testlerin geçtiğinden emin ol, ardından commit yap
```

### Yaygın Sorunlar

| Sorun | Neden | Çözüm |
|------|------|------|
| Kararsız testler | Asenkron işlemler, yarış koşulları | Bekleme sürelerini artır, mock kullan |
| Test izolasyonu başarısızlığı | Paylaşılan durum kirliliği | Her test öncesi durumu sıfırla |
| Yanlış mock | Beklenen değerler gerçek değerlerle uyuşmuyor | Mock ayarlarının doğru olduğunu doğrula |

## İlgili Kurallar

| İlgili kural | Açıklama |
|----------|------|
| Kod Stili kuralları | Kod okunabilirliği ve bakımı |
| Güvenlik Kuralları | Güvenlik test gereksinimleri |
| Kod İnceleme kuralları | Test kapsamı kontrolleri |
| Geliştirme iş akışı | TDD süreç entegrasyonu |
