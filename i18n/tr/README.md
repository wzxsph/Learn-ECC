# Learn-ECC

> Claude Code'yi (Everything Claude Code) ustalanmak icin yapilandirilmis bir ogrenme yolu

Learn-ECC'ye hos geldiniz! Bu, sizi yeni baslayandan deneyimli kullaniciya dogru ilerleten sistematik bir Claude Code ileri ogrenme yoludur.

---

## 🌐 Language / 语言 / 語言 / Dil / Язык / Ngôn ngữ

English | Português (Brasil) | 简体中文 | 繁體中文 | 日本語 | 한국어 | Türkçe | Русский | Tiếng Việt | ไทย

| [🇺🇸 English](./en/README.md) | [🇯🇵 日本語](./ja/README.md) | [🇰🇷 한국어](./ko/README.md) | [🇩🇪 Deutsch](./de/README.md) |
|:---:|:---:|:---:|:---:|
| [🇷🇺 Русский](./ru/README.md) | [🇹🇷 Türkçe](./README.md) | [🇧🇷 Português](./pt-BR/README.md) | [🇻🇳 Tiếng Việt](./vi/README.md) |
| [🇹🇭 ไทย](./th/README.md) | [🇨🇳 简体中文](../README.md) | | | |

---

## Arka Plan Hikayesi

### Neden Bu Proje Var?

Ingilizcem pek iyi degil, ECC'nin Ingilizce belgelerini okumakta zorlanlyorum. Bu yuzden ECC'deki Komutlari (Commands), Ajanlari (Agents), Becerileri (Skills) ve benzeri bilgileri Cince'den Turkce'ye cevirdim ve bunlari bir ogrenme kursu olarak duzenledim.

Referans belgeleri de boyle ortaya cikti - ECC ana projesindeki icerigi cevirip, her komutun ve her Agent'in ne ise yaradigini size anlatan bir kaynak.

### Kurs Nasil Olustu?

Acikcasi bu kurs AI tarafindan olusturuldu. AI, ECC'nin dosya yapisina gore temel icerik olusturdu, ancak detayli dusunceler ve gercek degerlendirmeler eksikti. Icerikte bir sorun fark ederseniz veya yeterince net degilse, lutfen duzeltme oneririlerinizi paylasiyor.

---

## Temel Kavramlar

### ECC Nedir?

**ECC** (Everything Claude Code), bir Claude Code eklenti projesidir ([orijinal depo](https://github.com/affaan-m/ECC)) ve sunlari saglar:
- **75 komut** - Satir komutlari (orn. `/plan`, `/code-review`) ve script komutlari
- **60 uzmanlikli Agent** - Kod inceleme, build tamir, mimari tasarim ve benzeri
- **232+ Beceri** - Yeniden kullanilabilir is akisi sablonlari
- **Hooks sistemi** - Olay tarafindan tetiklenen otomasyon mekanizmasi
- **Rules kurallari** - Zorunlu gelistirme standartlari

### Learn-ECC Nedir?

**Learn-ECC**, ECC'yi ogrenmek icin duzenledigim sistematik bir ogrenme yoludur, ECC'nin kendisi degildir.

"Ynte ogrenme" tasarim felsefesini benimser ve her asamada hem teorik ogrenme hem de pratik alistirmalar icerir. Gercek projeleri tamamlayarak Claude Code'un temel yeteneklerini edineceksiniz.

### Referans Belgeleri Nedir?

**Referans belgeleri** (./Referans-belgeleri/), ECC ana projesinin Turkce cevirisidir. ECC'nin Ingilizce belgelerini Turkce'ye cevirdim, boylece kendim (ve siz) rahatca basvurabiliriz.

Icerik sudur:
- Tam komut aciklamalari (her komutun ne ise yaradigi)
- Tum Agent'lerin detayli aciklamalari
- Skills dizin yapisi ve yazim standartlari
- Hooks sisteminin tam belgesi
- En iyi uygulamalar ve tasarim kalipolari

---

## Ogrenme Yolu Genel Gorunumu

| Asama | Icerik | Tahmini Sure | Zorluk |
|------|--------|--------------|--------|
| [1-Baslangic](./1-Baslangic/README.md) | ECC komutlari, Agent'ler, Skills'e giris + Kurulum | 2-3 saat | Baslangic |
| [2-Temel-Yetenekler](./2-Temel-Yetenekler/README.md) | Komut sistemi, Agent isbirligi, Hooks ozellestirme, Skills gelistirme, Rules yazimi | 8-12 saat | Ileri |
| [3-Uzman-Yolu](./3-Uzman-Yolu/README.md) | MCP gelistirme, Ozerk agent'ler, Ileri duzey kaliplar | 6-8 saat | Ileri |
| [4-Pratik-Projeler](./4-Pratik-Projeler/README.md) | CLI araclari, Kod inceleme, Otomasyon, Agent sistemleri | 10-15 saat | Pratik |
| [5-Hizli-Bagli](./5-Hizli-Bagli/README.md) | Komut hizli rehberi, Agent hizli rehberi, Is akisi hizli rehberi | Ihtiyac duyuldugunda | Referans |

---

## Hizli Navigasyon

### Birinci Asama: Baslangic
- [Kurs Girisi](./1-Baslangic/01-ECC-Tanimi.md) - ECC nedir, Learn-ECC ne yapabilir
- [Kurulum](./1-Baslangic/02-Kurulum.md) - Hizli baslangic yapilandirmasi
- [Temel Komutlar](./1-Baslangic/03-Temel-Komutlar.md) - Sik kullanilan komutlar
- [Ilk Gorev](./1-Baslangic/04-Ilk-Gorev.md) - Ilk gorevinizi tamamlayin
- [Baslangic Alistirmalari](./1-Baslangic/exercises/Baslangic-Alistirmalari.md) - Temelleri pekistirin

### Ikinci Asama: Temel Yetenekler
- [Komut Sistemi Ustaligi](./2-Temel-Yetenekler/01-Komut-Sistemi-Ustalii/README.md) - Tum komut turlerini ogrenin
- [Agent Isbirligi](./2-Temel-Yetenekler/02-Agent-Isbirligi/README.md) - Coklu agent sistem tasarimi
- [Hooks Ozellestirme](./2-Temel-Yetenekler/03-Hooks-Ozellestirme/README.md) - Otomatik is akislari
- [Skills Gelistirme](./2-Temel-Yetenekler/04-Skills-Gelistirme/README.md) - Yeniden kullanilabilir beceriler olusturun
- [Rules Yazimi](./2-Temel-Yetenekler/05-Rules-Yazimi/README.md) - Gelistirme standartlari olusturun

### Ucuncu Asama: Ileri Duzey Yol
- [MCP Gelistirme](./3-Uzman-Yolu/01-MCP-Gelistirme.md) - Model Context Protocol
- [Ozerk Ajanlar](./3-Uzman-Yolu/02-Ozerk-Ajanlar.md) - Ozerk Agent'ler olusturun
- [Ileri Duzey Kalipalar](./3-Uzman-Yolu/03-Ileri-Duzey-Kalipalar.md) - Tasarim kalipolari ve en iyi uygulamalar

### Dorduncu Asama: Pratik Projeler
- [Proje Bir: CLI Araci](./4-Pratik-Projeler/project-1-cli.md) - Komut satir araci olusturun
- [Proje Iki: Kod Inceleme](./4-Pratik-Projeler/project-2-review.md) - Otomatik inceleme is akisi
- [Proje Uc: Otomasyon Is Akisi](./4-Pratik-Projeler/project-3-auto.md) - Ucten uca otomasyon
- [Proje Dort: Agent Sistemi](./4-Pratik-Projeler/project-4-agent.md) - Coklu agent isbirligi platformu

### Besinci Asama: Hizli Bagli
- [Komut Hizli Rehberi](./5-Hizli-Bagli/cheatsheet-commands.md)
- [Agent Hizli Rehberi](./5-Hizli-Bagli/cheatsheet-agents.md)
- [Is Akisi Hizli Rehberi](./5-Hizli-Bagli/cheatsheet-workflows.md)

---

## Bu Kursu Nasil Kullanabilirsiniz

### Tavsiye Edilen Ogrenme Siralamasi

1. **Asama asama ilerleyin** - Her asama bir onceki asamanin uzerine insa edilir
2. **Teori ve pratigi birlestirin** - Her bolumde alistirma vardir, once anlayin sonra uygulayin
3. **Tum alistirmalari tamamlayin** - Alistirmalar ogrenme yolunun temel bolumleridir

### Ogrenme Tavsiyeleri

- **Baslangic asaması**: 2-3 saatte tamamlanmasi tavsiye edilir, temel kavramlar ve islemlere alisin
- **Temel yetenekler**: 8-12 saat tavsiye edilir, her modulu elinizle uygulayin
- **Uzman yolu**: 6-8 saat tavsiye edilir, belirli proje deneyimi gerektirir
- **Pratik projeler**: 10-15 saat tavsiye edilir, ilginizi ceken projeleri derinlemesine inceleyin

### Ogrenme Kontrol Noktalari

Her asamayi tamamladiktan sonra, su yeteneklere sahip oldugunuzu dogrulayin:
- Tum islemleri otonom olarak tamamlayabilirsiniz
- Temel kavramlari baskasina acıklayabilirsiniz
- Bilgiyi diger senaryolara uygulayabilirsiniz

---

## On Koşullar

### Temel Bilgiler
- Komut satir temelleri (temel komutlari bilme)
- JavaScript/Node.js temelleri (bazı ileri duzey ozellikler icin gerekli)
- Git temelleri (versiyon kontrol kavramlari)

### Ortam Gereksinimleri
- Node.js >= 18
- npm/pnpm/yarn/bun herhangi biri
- Metin duzenleyici (VS Code tavsiye edilir)

### Onceki Dersler
Yok. Baslangic asaması sifir bilgiyle baslayanlar icin tasarlanmistir.

---

## Ogrenme Yol Haritasi

```
┌─────────────────────────────────────────────────────────────────┐
│                     Learn-ECC Ogrenme Yol Haritasi             │
└─────────────────────────────────────────────────────────────────┘

    Asama 1: Baslangic
        │
        ↓
    ┌───────────────────────┐
    │   Asama 2: Temel       │←───────────────────┐
    │   Yetenekler           │                    │
    │  (Komut/Agent/Hooks/   │                    │
    │   Skills/Rules)        │                    │
    └───────────┬───────────┘                    │
                │                                │
                ↓                                │
    ┌───────────────────────┐                    │
    │   Asama 3: Uzman Yolu  │                    │
    │  (MCP/Ozerk Agent/    │                    │
    │   Ileri Kalipalar)    │                    │
    └───────────┬───────────┘                    │
                │                                │
                ↓                                │
    ┌───────────────────────┐                    │
    │   Asama 4: Pratik      │────────────────────┘
    │   Projeler             │
    └───────────┬───────────┘
                │
                ↓
    ┌───────────────────────┐
    │   Asama 5: Hizli Bagli │ ← Her zaman basvurulabilir
    │   (Komut/Agent rehber) │
    └───────────────────────┘
```

**Tahmini toplam ogrenme suresi**: 26-38 saat (yaklasik 2-3 hafta)

---

## Dizin Yapisi

```
Learn-ECC/
├── 1-Baslangic/                         # Asama 1: Baslangic (2-3 saat)
│   ├── 01-ECC-Tanimi.md                # ECC'ye genel bakis, temel degerler, mimari
│   ├── 02-Kurulum.md                   # Ortam hazirlama, kurulum, dogrulama
│   ├── 03-Temel-Komutlar.md            # /plan, /code-review, /build-fix
│   ├── 04-Ilk-Gorev.md                 # Tam gelistirme is akisi ornegi
│   └── exercises/
│       └── Baslangic-Alistirmalari.md  # 4 baslangic alistirmasi
│
├── 2-Temel-Yetenekler/                  # Asama 2: Temel Yetenekler (8-12 saat)
│   ├── 01-Komut-Sistemi-Ustalii/
│   ├── 02-Agent-Isbirligi/
│   ├── 03-Hooks-Ozellestirme/
│   ├── 04-Skills-Gelistirme/
│   └── 05-Rules-Yazimi/
│
├── 3-Uzman-Yolu/                        # Asama 3: Uzman Yolu (6-8 saat)
│   ├── 01-MCP-Gelistirme.md
│   ├── 02-Ozerk-Ajanlar.md
│   └── 03-Ileri-Duzey-Kalipalar.md
│
├── 4-Pratik-Projeler/                   # Asama 4: Pratik Projeler (10-15 saat)
│   ├── project-1-cli.md                # CLI araci gelistirme
│   ├── project-2-review.md             # Otomatik kod inceleme hatlari
│   ├── project-3-auto.md               # Is akisi otomasyonu
│   └── project-4-agent.md              # Ozel Agent gelistirme
│
├── 5-Hizli-Bagli/                      # Asama 5: Hizli Bagli
│   ├── cheatsheet-commands.md          # Komut hizli rehberi
│   ├── cheatsheet-agents.md            # Agent hizli rehberi
│   └── cheatsheet-workflows.md         # Is akisi hizli rehberi
│
├── assets/                              # Gorsel kaynaklar
├── progress-tracker.md                  # Ogrenme ilerleme takibi
├── contributing.md                      # Katki rehberi
└── README.md                            # Bu dosya
```

---

## Basari Icin Ipuclari

1. **Adim adim ilerleyin**: Asamalari atlamayin, her Asama bir sonrakinin temelidir
2. **Pratik yapin**: Her kavram ogrendiginizde hemen uygulayin, sadece okumayin
3. **Alistirmalari tamamlayin**: Her Asama'nın alistirmalari var, bilgiyi pekistirmek icin tamamlayin
4. **Hizli rehberleri kullanin**: Gunluk kullanimda ezber yapmak yerine hizli rehberleri kullanin
5. **Proje odakli olun**: 4. Asama gercek projelerle tum bilgileri birlestirir
6. **Not tutun**: Ogrenme deneyimlerinizi ve karsilastiginiz sorunlari kaydedin
7. **Digerlerine ogretin**: Baskalarina ogretmek, bilgiyi gercekten ogrendiginiz anlamina gelir

---

## Sikca Sorulan Sorular (SSS)

### S: Programlama temeli gerekli mi?

C: Temel komut satir ve programlama bilgisi gerekli (JavaScript/Node.js temelleri bazi ozellikleri anlamak icin faydali), ancak herhangi bir dilde uzmanlik gerekmez. Baslangic asaması sifir bilgiyle baslayanlar icin tasarlanmistir.

### S: Bazi asamalari atlayabilir miyim?

C: Asama 1-3'un sirayla tamamlanmasi tavsiye edilir, Asama 4 ilgi alanlariniza göre secim yapabilirsiniz, Asama 5 her zaman basvurulabilir.

### S: Tamamladiktan sonra ne yapabilirim?

C: Uretim düzeyinde Claude Code is akislarini kullanabilir, kod gelistirme, inceleme, test ve otomasyonu verimli bir sekilde yapabilir; ozel Agent sistemleri tasarlayabilir ve uygulayabilir; Hooks ve Skills ozellestirebilirsiniz.

### S: Ogrenme sürecinde sorunlarla karsilaisirsam ne yapmaliyim?

C: 1) [Referans belgelerine](./Referans-belgeleri/) ilgili bolumlere bakin; 2) [Komut hizli rehberine](./5-Hizli-Bagli/cheatsheet-commands.md) bakin; 3) Yardim icin Issue acin.

### S: Ogrenme etkisini nasil dogrulayabilirim?

C: Ilerlemenizi takip etmek icin [progress-tracker.md](./progress-tracker.md) kullanin, her asamanin açik ogrenme hedefleri var, tum alistirmalari tamamlamak o asamadaki bilgiyi edindiginiz anlamina gelir.

### S: Kurs iciği guncellenecek mi?

C: Evet, düzenli guncellemeler olacak, icerik katkida bulunmak icin PR gonderin. Detayli kilavuz icin [contributing.md](./contributing.md) bakin.

---

## Katki ve Geri Bildirim

Ogrenme sürecinde bir sorun fark ederseniz veya iyilestirme öneriniz varsa, Issue veya Pull Request gonderebilirsiniz.

---

*Son guncelleme: 2026-05-26*