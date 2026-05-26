# 04-Ilk Gorev

## Ogrenme Hedefleri

- Tam gelistirme is akisini anlamak
- /ecc:plan → /ecc:code-review → /ecc:build-fix → commit tam is akisini kavramak
- Basit bir islevselligi bagimsiz olarak gelistirebilmek

---

## Tam Gelistirme Is Akisi

ECC su gelistirme is akisini kullanmayi tavsiye eder:

```
Adim 1: /ecc:plan        → Gorevi planla
Adim 2: /ecc:code-review → Kodu incele
Adim 3: /ecc:build-fix   → Build'i tamir et
Adim 4: Kodu commit et   → Git commit
```

---

## Adim 1: /ecc:plan Kullanarak Planlama

### Giris

Diyelim ki basit bir hesap makinesi islevi gerceklestirmek istiyoruz:

```
/ecc:plan Dort islemi destekleyen bir hesap makinesi uygula
```

### Beklenen Cikti

/ecc:plan komutu su islemleri yapar:

1. **Gereksinimi tekrarlar** - "Bir hesap makinesi uygula, dort temel islemi desteklesin..."
2. **Risk degerlendirmesi** - "Dikkat edilmesi gereken: Sifira bolme durumu..."
3. **Adim adim plan** -
   - Asama 1: Hesap makinesi cekirdek islevlerini olustur
   - Asama 2: Dort islemi gerceklestir
   - Asama 3: Hata yonetimi ekle
   - Asama 4: Birim testleri yaz
4. **Onay bekler** - "Yukaridaki planin baslayip baslamayacagini onaylayin"

### Onaydan Sonra

Kullanici onayladiginda, ECC her asamayi baslatmak icin kullanici girisini bekler.

### Ipuclari

- Daha detayli planlama icin PRD dosya kalibini kullanabilirsiniz
- Karmaşik islevsellikler icin once PRD yazmasi, sonra planlama yapmasi tavsiye edilir

---

## Adim 2: /ecc:code-review Kullanarak Inceleme

### Giris

Kod yazimi tamamlandiginda, calistirin:

```
/ecc:code-review
```

### Beklenen Cikti

/ecc:code-review su islemleri yapar:

1. Degisiklik dosyalarini toplar
2. 7 kategoride inceleme yapar:
   - Dogruluk
   - Tip guvenligi
   - Kalip uyumlulugu
   - Guvenlik
   - Performans
   - Tamamlilik
   - Bakim kolayligi
3. CRITICAL/HIGH/MEDIUM/LOW seviyelerinde inceleme raporu olusturur

### Inceleme Raporu Ornegi

```
## Kod Inceleme Raporu

### CRITICAL
- [ ] Sabitlenmis kimlik bilgileri: src/auth.js 23. satir

### HIGH
- [ ] Islev cok uzun: src/calculator.js 45-78. satirlar (34 satir)
- [ ] Hata yonetimi eksik: src/calculator.js bolme islevi

### MEDIUM
- [ ] Adlandirma standart disi: `tmp` degiskeni `temporary` olarak degistirilmeli

### Eylem Onerileri
1. Sabitlenmis API Key'i kaldir
2. calculator.js bolme islevini yeniden yapilandir, sifira bolme kontrolu ekle
3. Cok uzun islevi ayir
```

### Ipuclari

- Commit etmeden once mutlaka /ecc:code-review calistirin
- CRITICAL ve HIGH sorunlari mutlaka duzeltilmelidir

---

## Adim 3: /ecc:build-fix Kullanarak Tamir

### Giris

Build basarisiz olursa, calistirin:

```
/ecc:build-fix
```

### Beklenen Cikti

/ecc:build-fix su islemleri yapar:

1. **Build sistemini algilar** - "TypeScript projesi algilandi"
2. **Build'i calistirir** - "tsc --noEmit calistiriliyor..."
3. **Hatalari ayristirir** - Dosyaya gore gruplandirir
4. **Adim adim tamir eder** - Bir seferde bir hata
5. **Dogrular** - Her tamirden sonra yeniden build eder

### Ornek Cikti

```
[build-fix] TypeScript projesi algilandi
[build-fix] tsc --noEmit calistiriliyor...
[build-fix] 3 hata bulundu:

Hata 1: src/calculator.ts:15 - Tur 'string' tur 'number' atanabilir degil
Hata 2: src/calculator.ts:28 - 'result' ozelligi mevcut degil
Hata 3: src/calculator.ts:42 - 'b' parametresi kullanilmiyor

[build-fix] Hata 1 tamir ediliyor...
[build-fix] Tamir tamamlandi, dogrulaniyor...
[build-fix] Build basarili!
```

### Ipuclari

- Build tamiri artimseldir, bir seferde bir hata
- Isteginiz anda Ctrl+C ile durdurabilirsiniz

---

## Adim 4: Kodu Commit Etme

### Commit Is Akisi

1. Tum testlerin gectiginden emin olun
2. Build'in basarili oldugundan emin olun
3. Kod inceleme sorunlarinin duzeltildiginden emin olun

### Git Commit Standartlari

ECC conventional commits formatini kullanir:

```
<type>: <description>

[optional body]
```

**Type Turleri**:
- `feat` - Yeni ozellik
- `fix` - Bug duzeltme
- `refactor` - Yeniden yapilandirma
- `docs` - Dokuman
- `test` - Test
- `chore` - Rutin bakim
- `perf` - Performans optimizasyonu
- `ci` - CI yapilandirmasi

### Commit Ornegi

```
feat: Dort temel hesaplama islemini gerceklestir

- Toplama islevi eklendi
- Cikarma islevi eklendi
- Carpma islevi eklendi
- Bolme islevi eklendi (sifira bolme kontrolu ile)

Ilgili: Issue #123
```

### Ipuclari

- Commit etmeden once hizli dogrulama icin `/ecc:verify quick` kullanin
- Buyuk ozellikleri birden fazla commit'e ayirin, kucuk ozellikleri tek commit'te birlesitin

---

## Tam Ornek

### Senaryo

Basit bir字符串 arac islevi gerceklestirme: `truncate(str, maxLength)` - 字符串'i belirtilen uzunluga kadar keser.

### Adim 1: Planlama

```
/ecc:plan truncate islevini gerceklestir, 字符串'i belirtilen uzunluga kadar kes
```

Cikti:
```
## Gereksinim Tekrari
truncate(str, maxLength) islevini gerceklestir, maxLength'i asanki kisim ... ile degistir

## Risk Degerlendirmesi
- maxLength 3'ten kucuk oldugunda
- str bos veya null oldugunda
- Unicode karakterlerinin islenmesi

## Adim Adim Plan
1. src/string-utils.ts olustur
2. truncate islevini gerceklestir
3. Sinir durumlari icin testler ekle
4. JSDoc dokumani ekle

Baslayabilir misiniz?
```

### Adim 2: Gerceklestirme

(Kullanici onayladiginde, ECC kodu gerceklestirmeye yardim eder)

### Adim 3: Inceleme

```
/ecc:code-review
```

Cikti:
```
## Kod Inceleme Raporu
Durum: ONAYLANDI

### Inceleme Sonuclari
- Dogruluk: Gecti
- Tip Guvenligi: Gecti
- Kalip Uyumlulugu: Gecti
- Guvenlik: Gecti
- Performans: Gecti
- Tamamlilik: Daha fazla sinir durumu testi eklenmesi tavsiye edilir
- Bakim Kolayligi: Gecti

### Oneriler
- Bos 字符串 testi ekle
- Null deger testi ekle
```

### Adim 4: Build Dogrulama

```
/ecc:build-fix
```

```
[build-fix] TypeScript projesi algilandi
[build-fix] tsc --noEmit calistiriliyor...
[build-fix] Build basarili!
```

### Adim 5: Commit

```
feat: truncate 字符串 kesme islevini gerceklestir

- Maksimum uzunluk belirtmeyi destekler
- Asan kisim ... ile degistirilir
- Bos 字符串 ve null degerleri isler
```

---


## Ozet

1. **Tam is akisi**: /ecc:plan → Gerceklestir → /ecc:code-review → /ecc:build-fix → Commit
2. **/ecc:plan** planlama icin kullanilir, dogr yonu saglar
3. **/ecc:code-review** inceleme icin kullanilir, sorunlari bulur
4. **/ecc:build-fix** build hatalarini tamir etmek icin kullanilir
5. **Commit** conventional commits formatini kullanir

---

## Sonraki Adimlar

- Kursu tamamladiktan sonra [exercises/Baslangic-Alistirmalari.md](./exercises/Baslangic-Alistirmalari.md) yapin
- Asama 2'ye gecin: Temel Komut Ileria