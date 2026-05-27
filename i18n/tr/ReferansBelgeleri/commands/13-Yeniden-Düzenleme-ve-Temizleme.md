# Yeniden Düzenleme ve Temizleme Komutları

## Genel Bakış

Yeniden düzenleme ve temizleme komutları, kod yeniden yapılandırma, ölü kod temizleme ve otomatik güncelleme için kullanılır.

## Komut Listesi

### /refactor-clean

**Kullanım**: Ölü kod sil + tekrarları birleştir

**Tanım**: Kullanılmayan kodu tanımlar ve siler, tekrarlayan kod parçalarını birleştirir.

**Analiz Edilen İçerik**:
- Kullanılmayan fonksiyonlar ve değişkenler
- Tekrarlayan kod blokları
- Güncelliğini yitirmiş yorumlar
- Kullanılmayan içe aktarmalar

**Desteklenen Araçlar**:
- `knip` - Ölü kod tespiti
- `depcheck` - Kullanılmayan bağımlılık tespiti
- `ts-prune` - TypeScript ölü kod tespiti

---

### /auto-update

**Kullanım**: Otomatik güncelleme yetenekleri

**Tanım**: Proje bağımlılıklarını ve yapılandırmalarını en son sürümlere otomatik olarak günceller.

**Güncellenen İçerik**:
- npm/pip paket güncellemeleri
- Yapılandırma dosyası göçleri
- Kod göçleri
- Sürüm uyumluluk kontrolleri

---

## İlgili Komutlar

- `/refactor-clean` - Yeniden düzenleme temizliği
- `/auto-update` - Otomatik güncelleme
