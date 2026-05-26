# 01 Komut Sistemi Ustaligi

Claude Code'un 75+ komutunu ogrenin, esnek kombinasyonlarla verimli gelistirme saglayin.

## Modul Hedefleri

Bu modulu tamamladiktan sonra su isleri yapabileceksiniz:

- Tum komut siniflandirmalari altindaki komutlari tanimlayabilecek ve kullanabileceksiniz
- Senaryoya gore en uygun komutu seceebileceksiniz
- Birden fazla komutu birlestirerek ozel is akislari olusturabileceksiniz
- Komut dokumanlarini kullanarak komut kullanimini hizla arayabileceksiniz

## Komut Genel Gorunumu

Claude Code 75+ komut saglar ve su siniflandirmalari kapsar:

| Siniflandirma | Komut Sayisi | Ornek Komutlar |
|------|--------|----------|
| Gelistirme akisi | 15+ | `/plan`, `/tdd`, `/build-fix`, `/verify` |
| Kod inceleme | 10+ | `/code-review`, `/security-review`, `/python-review` |
| Build ve test | 15+ | `/go-build`, `/rust-build`, `/flutter-build` |
| Proje yonetimi | 10+ | `/project-init`, `/projects`, `/santa-loop` |
| Beceri yonetimi | 10+ | `/skill-create`, `/skill-stocktake`, `/skill-scout` |
| Ogrenme ve arastirma | 10+ | `/learn`, `/learn-eval`, `/deep-research` |
| Yardimci araclar | 15+ | `/config`, `/jira`, `/pr`, `/e2e` |

## Komut Dokuman Yapisi

Her komutun standart bir dokuman yapisi vardir:

```
Komut Adi
├── Komut Aciklamasi
├── Kullanim Senaryolari
├── Parametre Aciklamasi
├── Kullanim Ornekleri
└── Dikkat Edilecekler
```

## Sik Kullanilan Komutlar Hizli Rehberi

### Gelistirme Akisi
- `/plan` - Uygulama plani olustur
- `/tdd` - Test-driven gelistirme
- `/build-fix` - Build hatalarini tamir et
- `/verify` - Kod degisikliklerini dogrula
- `/review` - Kod inceleme

### Build Iliskili
- `/go-build`, `/rust-build`, `/kotlin-build` - Cesitli dillerin build islemleri
- `/cpp-build`, `/flutter-build` - Derlenebilir dillerin build islemleri
- `/docker-build` - Docker imaj build

### Proje Yonetimi
- `/project-init` - Yeni proje baslat
- `/projects` - Tum projeleri listele
- `/santa-loop` - Otomatik gorev dongusu

### Beceri Gelistirme
- `/skill-create` - Git gecmisinden Skill olustur
- `/skill-scout` - Mevcut Skills'i ara
- `/learn` - Oturumdan kalip cikar

## Komut Dokumanlarini Nasil Kullanabilirsiniz

Tamamlayici komut dokumanlari su konumda: `../../Referans-belgeleri/commands/01-Cekirdek-Is-Akis.md`

Dokuman su bolumleri icerir:
- Her komutun detayli aciklamasi
- Parametre ve opsiyon aciklamalari
- Kullanim ornekleri ve ciktilar
- En iyi uygulama tavsiyeleri

## Komutlari Birlikte Kullanma

Komutlar birlestirilerek karmaşik is akislari olusturulabilir:

```
/plan → /tdd → /build-fix → /code-review → /verify
```

Her komutun ciktisi bir sonraki komuta aktarilabilir ve boylece otomatiklesmis bir boru hatti olusur.

## Sonraki Adimlar

- [Alistirma sorulari](./exercises/Alistirma.md) ogren
- [Tam komut dokumanlarini oku](../../Referans-belgeleri/commands/01-Cekirdek-Is-Akis.md)