# Test Çalıştırıcıları

Bu belge, ECC projesinin test yapısını ve çalıştırma yöntemlerini açıklar.

## Test Girişi

**Yol**: `tests/run-all.js`

Merkezi test çalıştırıcısı, tüm test dosyalarını otomatik olarak keşfeder ve çalıştırır.

### Kullanım Yöntemi

```bash
# Tüm testleri çalıştır
node tests/run-all.js

# Tek test dosyası çalıştır
node tests/lib/utils.test.js
node tests/hooks/hooks.test.js
```

### Test Dosyası Keşfi

Test çalıştırıcısı, glob kalıbı `tests/**/*.test.js` ile test dosyalarını otomatik olarak keşfeder.

Kurallar:
- Dosya `tests/` dizini altında olmalı
- Dosya adı `.test.js` ile bitmeli
- Dizin yapısı göstermek için korunur

### Çıktı Formatı

```
╔══════════════════════════════════════════════════════════╗
║           Everything Claude Code - Test Suite             ║
╚══════════════════════════════════════════════════════════╝

━━━ Running lib/utils.test.js ━━━
(Test çıktısı...)
━━━ Running hooks/hooks.test.js ━━━
(Test çıktısı...)

╔══════════════════════════════════════════════════════════╗
║                     Final Results                        ║
╠══════════════════════════════════════════════════════════╣
║  Total Tests:    42                                      ║
║  Passed:         42  ✓                                   ║
║  Failed:         0                                        ║
╚══════════════════════════════════════════════════════════╝
```

### Test İstatistikleri

Test çalıştırıcısı her test dosyasının çıktısını ayrıştırarak istatistikleri özetler:
- `Passed: <sayı>`
- `Failed: <sayı>`

---

## Test Yapısı

### Dizin Düzeni

```
tests/
├── run-all.js              # Ana test çalıştırıcısı
├── lib/                    # Kütüphane araç testleri
│   ├── utils.test.js
│   ├── package-manager.test.js
│   └── ...
├── hooks/                  # Hook entegrasyon testleri
│   └── hooks.test.js
├── integration/            # Entegrasyon testleri
├── scripts/                # Betik testleri
├── commands/               # Komut testleri
├── docs/                   # Dokümantasyon testleri
├── opencode-config.test.js
├── plugin-manifest.test.js
├── test_*.py               # Python testleri
└── conftest.py             # pytest yapılandırması
```

### Test Türleri

1. **Birim Testleri** (`*.test.js`)
   - Bağımsız fonksiyonları ve araçları test eder
   - Node.js native assert veya Jest tarzı kullanır

2. **Entegrasyon Testleri** (`tests/integration/`)
   - Birden fazla bileşenin birlikte çalışmasını test eder
   - Hook tam akışlarını test eder

3. **Python Testleri** (`test_*.py`)
   - pytest formatında Python testleri
   - `conftest.py` ile yapılandırılır

---

## Test Yapılandırması

### Node.js Test

Test dosyaları standart Node.js assert modülünü kullanır:

```javascript
const assert = require('assert');

test('örnek test', () => {
  assert.strictEqual(actual, expected, 'İsteğe bağlı mesaj');
});
```

### Python Test (pytest)

**Konum**: `tests/conftest.py`

Paylaşılan fixture'ları ve yapılandırmayı sağlayan pytest yapılandırma dosyası.

```bash
# Python testlerini çalıştır
python -m pytest tests/

# Belirli testi çalıştır
python -m pytest tests/test_builder.py
```

---

## Test En İyi Uygulamaları

### AAA Örüntüsü

```
test('TestTanımı', () => {
  // Arrange - Test verilerini hazırla
  const input = 'test verisi';

  // Act - Test edilen işlemi gerçekleştir
  const result = myFunction(input);

  // Assert - Sonucu doğrula
  assert.strictEqual(result, expected);
});
```

### Test Adlandırma

Testin davranışını açıklayan tanımlayıcı isimler kullan:

```javascript
test('sorguya uyan piyasa olmadığında boş dizi döndürür', () => {});
test('API anahtarı eksik olduğunda hata fırlatır', () => {});
test('Redis kullanılamadığında alt dize aramasına geri döner', () => {});
```

### Test İzolasyonu

Her test şunu yapmalıdır:
- Bağımsız çalışır, diğer testlere bağlı değildir
- Kendi test verilerini temizler
- Harici bağımlılıkları mock'larla izole eder

---

## CI Entegrasyonu

`package.json`'da test betiklerini yapılandır:

```json
{
  "scripts": {
    "test": "validate-commands.js && tests/run-all.js"
  }
}
```

Test çalıştırıcısı CI ortamında çalışmak üzere tasarlanmıştır:
- Sıfır olmayan çıkış kodu başarısızlığı gösterir
- Otomatik ayrıştırma için JSON çıktısı kullanılır
- Hata ayıklama için net hata mesajları kullanılır
