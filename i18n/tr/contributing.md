# Learn-ECC Katki Rehberi

Learn-ECC'ye icerik katkısında bulunmaktan memnuniyet duyariz! Bu kilavuz, yeni icerik ekleme, mevcut icerigi iyilestirme veya duzeltme gonderme konusunda yol gosterir.

## Katki Turleri

### 1. Yeni Icerik Ekleme

Katkıda bulunabileceğiniz icerik turleri:
- Yeni bolumler veya moduller
- Yeni alistirma sorulari
- Yeni pratik projeler
- Hizli rehber tamamlamalari
- Ceviri iyilestirmeleri

### 2. Mevcut Icerigi Iyilestirme

- Hatalari duzeltme
- Aciklamalarin netligini artirma
- Daha fazla ornek ekleme
- Alistirma cevaplarini tamamlama
- Guncel olmayan bilgileri yenileme

### 3. Sorun Bildirimi

- Yazim veya dilbilgisi hatalari
- Calismayan kod ornekleri
- Bozuk linkler
- Eksik icerik

## Icerik Format Standartlari

### Markdown Formati

Tum icerik Markdown formati kullanilarak yazilmalidir ve su bolumleri icermelidir:

```markdown
# Baslik

## Ogrenme Hedefleri

Bu bolumu tamamladiktan sonra su isleri yapabileceksiniz:
- Hedef1
- Hedef2

---

## Ana Icerik

### Alt Baslik

Icerik...

---

## Alistirmalar

### Alistirma 1

Gorev açiklamasi...

### Dogrulama Yontemi

1. Dogrulama adimi 1
2. Dogrulama adimi 2

---

## Sonraki Adimlar

- Sonraki bolum: [Link](./next.md)
- Geri: [Icindekiler](../README.md)
```

### Dosya Adlandirma

- Türkce dosya adlari kullanilir
- Kelimeleri ayirmak icin tire `-` kullanilir
- Bolum dosyasi adlandirmasi: `01-Bolus Adi.md`, `02-Bolus Adi.md`
- Alistirma dosyasi adlandirmasi: `Alistirma-adi.md`

### Link Standartlari

- Ders kitabi linkleri: `./Referans-belgeleri/dosya-adi.md`
- Kurs ici linkler: `./bolum-adi.md`
- Göreli yollar: Mevcut dosya konumundan hesaplanir

### Kod Ornekleri

```javascript
// Kod dili etiketi
function example() {
  return "Hello ECC"
}
```

```bash
# Komut satir ornegi
node tests/run-all.js
```

### Tablo Formati

| Baslik 1 | Baslik 2 | Baslik 3 |
|----------|----------|----------|
| Icerik 1 | Icerik 2 | Icerik 3 |

## Düzeltme Gonderme

### Adimlar

1. Depoyu Fork edin
2. Dal olusturun: `git checkout -b feature/yeni-icerik-aciklamasi`
3. Duzeltmeleri yapin
4. Commit edin: `git commit -m "feat: yeni icerik ekle"`
5. Uzaga push edin: `git push origin feature/yeni-icerik-aciklamasi`
6. Pull Request olusturun

### Commit Mesaji Standartlari

```
<type>: <description>

<optional body>
```

Type turleri:
- `feat`: Yeni ozellik
- `fix`: Hata duzeltme
- `docs`: Dokuman guncellme
- `test`: Test guncellme
- `refactor`: Yeniden yapilandirma

## Kalite Kontrol

Gondermeden once kontrol edin:

- [ ] Markdown formati dogru
- [ ] Tum linkler calisiyor
- [ ] Kod ornekleri calisiyor
- [ ] Alistirmalar icin dogrulama yontemi var
- [ ] Türkce yazim hatalari yok
- [ ] Format standartlariya uyumlu

## Geri Bildirim

Bir sorun veya öneri fark ederseniz, su bilgileri iceren bir Issue olusturun:
- Sorun açiklamasi
- Bulundugu dosya konumu
- Önerilen iyilestirme yontemi

---

Katkiniz icin tesekkürler!