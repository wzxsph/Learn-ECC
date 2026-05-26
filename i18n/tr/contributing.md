# Learn-ECC Katkı Rehberi

Learn-ECC'ye içerik katkısında bulunmaktan memnuniyet duyarız! Bu kılavuz, yeni içerik ekleme, mevcut içeriği iyileştirme veya düzeltme gönderme konusunda yol gösterir.

## Katkı Türleri

### 1. Yeni İçerik Ekleme

Katkıda bulunabileceğiniz içerik türleri:
- Yeni bölümler veya modüller
- Yeni alıştırma soruları
- Yeni pratik projeler
- Hızlı rehber tamamlamaları
- Çeviri iyileştirmeleri

### 2. Mevcut İçeriği İyileştirme

- Hataları düzeltme
- Açıklamaların netliğini artırma
- Daha fazla örnek ekleme
- Alıştırma cevaplarını tamamlama
- Güncel olmayan bilgileri yenileme

### 3. Sorun Bildirimi

- Yazım veya dilbilgisi hataları
- Çalışmayan kod örnekleri
- Bozuk linkler
- Eksik içerik

## İçerik Format Standartları

### Markdown Formatı

Tüm içerik Markdown formatı kullanılarak yazılmalıdır ve şu bölümleri içermelidir:

```markdown
# Başlık

## Öğrenme Hedefleri

Bu bölümü tamamladıktan sonra şu işleri yapabileceksiniz:
- Hedef1
- Hedef2

---

## Ana İçerik

### Alt Başlık

İçerik...

---

## Alıştırmalar

### Alıştırma 1

Görev açıklaması...

### Doğrulama Yöntemi

1. Doğrulama adımı 1
2. Doğrulama adımı 2

---

## Sonraki Adımlar

- Sonraki bölüm: [Link](./next.md)
- Geri: [İçindekiler](../README.md)
```

### Dosya Adlandırma

- Türkçe dosya adları kullanılır
- Kelimeleri ayırmak için tire `-` kullanılır
- Bölüm dosyası adlandırması: `01-BölümAdı.md`, `02-BölümAdı.md`
- Alıştırma dosyası adlandırması: `Alıştırma-adi.md`

### Link Standartları

- Ders kitabı linkleri: `./ReferansBelgeleri/dosya-adi.md`
- Kurs içi linkler: `./bolum-adi.md`
- Göreli yollar: Mevcut dosya konumundan hesaplanır

### Kod Örnekleri

```javascript
// Kod dili etiketi
function example() {
  return "Hello ECC"
}
```

```bash
# Komut satırı örneği
node tests/run-all.js
```

### Tablo Formatı

| Başlık 1 | Başlık 2 | Başlık 3 |
|----------|----------|----------|
| İçerik 1 | İçerik 2 | İçerik 3 |

## Düzeltme Gönderme

### Adımlar

1. Depoyu Fork edin
2. Dal oluşturun: `git checkout -b feature/yeni-icerik-aciklamasi`
3. Düzeltmeleri yapın
4. Commit edin: `git commit -m "feat: yeni içerik ekle"`
5. Uzağa push edin: `git push origin feature/yeni-icerik-aciklamasi`
6. Pull Request oluşturun

### Commit Mesajı Standartları

```
<type>: <description>

<optional body>
```

Type türleri:
- `feat`: Yeni özellik
- `fix`: Hata düzeltme
- `docs`: Doküman güncelleme
- `test`: Test güncelleme
- `refactor`: Yeniden yapılandırma

## Kalite Kontrol

Göndermeden önce kontrol edin:

- [ ] Markdown formatı doğru
- [ ] Tüm linkler çalışıyor
- [ ] Kod örnekleri çalışıyor
- [ ] Alıştırmalar için doğrulama yöntemi var
- [ ] Türkçe yazım hataları yok
- [ ] Format standartlarıyla uyumlu

## Geri Bildirim

Bir sorun veya öneri fark ederseniz, şu bilgileri içeren bir Issue oluşturun:
- Sorun açıklaması
- Bulunduğu dosya konumu
- Önerilen iyileştirme yöntemi

---

Katkınız için teşekkürler!