# Learn-ECC

> ECC'yi (Everything Claude Code) ustalanmak için yapılandırılmış bir öğrenme yolu

Learn-ECC'ye hoş geldiniz! Bu, sizi yeni başlayandan deneyimli kullanıcıya doğru ilerleten sistematik bir Claude Code öğrenme yoludur.

---

## 🌐 Language / 语言 / 語言 / Dil / Язык / Ngôn ngữ

English | Português (Brasil) | 简体中文 | 繁體中文 | 日本語 | 한국어 | Türkçe | Русский | Tiếng Việt | ไทย

| [🇺🇸 English](./en/README.md) | [🇯🇵 日本語](./ja/README.md) | [🇰🇷 한국어](./ko/README.md) | [🇩🇪 Deutsch](./de/README.md) |
|:---:|:---:|:---:|:---:|
| [🇷🇺 Русский](./ru/README.md) | [🇹🇷 Türkçe](./README.md) | [🇧🇷 Português](./pt-BR/README.md) | [🇻🇳 Tiếng Việt](./vi/README.md) |
| [🇹🇭 ไทย](./th/README.md) | [🇨🇳 简体中文](../README.md) | | | |
|---

> **⚠️ Çok Dilli Versiyon Bildirimi**
> 
> Diğer dillerdeki (İngilizce, Japonca, Korece, Almanca, Rusça, Portekizce, Vietnamca, Tayca) çeviri belgeleri çok sayıda çeviri hatası ve sorun içermektedir. Kişisel sınırlamalar nedeniyle bu içerikleri desteklemeye ve düzeltmeye devam edemiyorum.
> 
> Belirli bir dilin çevirisini düzeltmeye yardımcı olmak istiyorsanız lütfen bir Pull Request gönderin.
> 
> **Şu anda yalnızca Çince sürüm aktif olarak bakımı yapılmaktadır.**

---

---

## Arka Plan Hikayesi

### Neden Bu Proje Var?

İngilizcem pek iyi değil, ECC'nin İngilizce belgelerini okumakta zorlanıyorum. Bu yüzden ECC'deki Komutları (Commands), Ajanları (Agents), Becerileri (Skills) ve benzeri bilgileri Çince'den Türkçe'ye çevirdim ve bunları bir öğrenme kursu olarak düzenledim.

Referans belgeleri de böyle ortaya çıktı - ECC ana projesindeki içeriği çevirip, her komutun ve her Agent'ın ne işe yaradığını size anlatan bir kaynak.

### Kurs Nasıl Oluştu?

Açıkçası bu kurs AI tarafından oluşturuldu. AI, ECC'nin dosya yapısına göre temel içerik oluşturdu, ancak detaylı düşünceler ve gerçek değerlendirmeler eksikti. İçerikte bir sorun fark ederseniz veya yeterince net değilse, lütfen düzeltme önerilerinizi paylaşın.

---

## Temel Kavramlar

### ECC Nedir?

**ECC** (Everything Claude Code), bir Claude Code eklenti projesidir ([orijinal depo](https://github.com/affaan-m/ECC)) ve şunları sağlar:
- **75 komut** - Satır komutları (örn. `/plan`, `/code-review`) ve script komutları
- **60 uzmanlıkli Agent** - Kod inceleme, build tamir, mimari tasarım ve benzeri
- **232+ Beceri** - Yeniden kullanılabilir iş akışı şablonları
- **Hooks sistemi** - Olay tarafından tetiklenen otomasyon mekanizması
- **Rules kuralları** - Zorunlu geliştirme standartları

### Learn-ECC Nedir?

**Learn-ECC**, ECC'yi öğrenmek için düzenlediğim sistematik bir öğrenme yoludur, ECC'nin kendisi değildir.

"Öğrenerek yapma" tasarım felsefesini benimser ve her aşamada hem teorik öğrenme hem de pratik alıştırmalar içerir. Gerçek projeleri tamamlayarak Claude Code'un temel yeteneklerini edineceksiniz.

### Referans Belgeleri Nedir?

**Referans belgeleri** (./ReferansBelgeleri/), ECC ana projesinin Türkçe çevirisidir. ECC'nin İngilizce belgelerini Türkçe'ye çevirdim, böylece kendim (ve siz) rahatça başvurabiliriz.

İçerik şudur:
- Tam komut açıklamaları (her komutun ne işe yaradığı)
- Tüm Agent'lerin detaylı açıklamaları
- Skills dizin yapısı ve yazım standartları
- Hooks sisteminin tam belgesi
- En iyi uygulamalar ve tasarım kalıpları

---

## Öğrenme Yolu Genel Görünümü

| Aşama | İçerik | Tahmini Süre | Zorluk |
|------|--------|--------------|--------|
| [1-Başlangıç](./1-Başlangıç/README.md) | ECC komutları, Agent'ler, Skills'e giriş + Kurulum | 2-3 saat | Başlangıç |
| [2-Çekirdek Yetenekler](./2-ÇekirdekYetenekler/README.md) | Komut sistemi, Agent işbirliği, Hooks özelleştirme, Skills geliştirme, Rules yazımı | 8-12 saat | Orta |
| [3-Uzman Yolu](./3-UzmanYolu/README.md) | MCP geliştirme, Özerk agent'ler, İleri düzey kalıplar | 6-8 saat | İleri |
| [4-Pratik Projeler](./4-PratikProjeler/README.md) | CLI araçları, Kod inceleme, Otomasyon, Agent sistemleri | 10-15 saat | Pratik |
| [5-Hızlı Başvuru](./5-HızlıBaşvuru/README.md) | Komut hızlı rehberi, Agent hızlı rehberi, İş akışı hızlı rehberi | İhtiyaç duyulduğunda | Referans |

---

## Hızlı Navigasyon

### Birinci Aşama: Başlangıç
- [Kurs Girişi](./1-Başlangıç/01-ECC-Tanıtım.md) - ECC nedir, Learn-ECC ne yapabilir
- [Kurulum](./1-Başlangıç/02-Kurulum.md) - Hızlı başlangıç yapılandırması
- [Temel Komutlar](./1-Başlangıç/03-TemelKomutlar.md) - Sık kullanılan komutlar
- [İlk Görev](./1-Başlangıç/04-İlkGörev.md) - İlk görevinizi tamamlayın
- [Başlangıç Alıştırmaları](./1-Başlangıç/exercises/BaşlangıçAlıştırmaları.md) - Temelleri pekiştirin

### İkinci Aşama: Çekirdek Yetenekler
- [Komut Sistemi Ustalığı](./2-ÇekirdekYetenekler/01-KomutSistemi/README.md) - Tüm komut türlerini öğrenin
- [Agent İşbirliği](./2-ÇekirdekYetenekler/02-Agentİşbirliği/README.md) - Çoklu agent sistem tasarımı
- [Hooks Özelleştirme](./2-ÇekirdekYetenekler/03-HooksÖzelleştirme/README.md) - Otomatik iş akışları
- [Skills Geliştirme](./2-ÇekirdekYetenekler/04-SkillsGeliştirme/README.md) - Yeniden kullanılabilir beceriler oluşturun
- [Rules Yazımı](./2-ÇekirdekYetenekler/05-RulesYazma/README.md) - Geliştirme standartları oluşturun

### Üçüncü Aşama: Uzman Yolu
- [MCP Geliştirme](./3-UzmanYolu/01-MCPGeliştirme.md) - Model Context Protocol
- [Özerk Ajanlar](./3-UzmanYolu/02-OtonomAcenteler.md) - Özerk Agent'ler oluşturun
- [İleri Düzey Kalıplar](./3-UzmanYolu/03-İleriDüzeyDesenler.md) - Tasarım kalıpları ve en iyi uygulamalar

### Dördüncü Aşama: Pratik Projeler
- [Proje Bir: CLI Aracı](./4-PratikProjeler/project-1-cli.md) - Komut satırı aracı oluşturun
- [Proje İki: Kod İnceleme](./4-PratikProjeler/project-2-review.md) - Otomatik inceleme iş akışı
- [Proje Üç: Otomasyon İş Akışı](./4-PratikProjeler/project-3-auto.md) - Uçtan uca otomasyon
- [Proje Dört: Agent Sistemi](./4-PratikProjeler/project-4-agent.md) - Çoklu agent işbirliği platformu

### Beşinci Aşama: Hızlı Başvuru
- [Komut Hızlı Rehberi](./5-HızlıBaşvuru/cheatsheet-commands.md)
- [Agent Hızlı Rehberi](./5-HızlıBaşvuru/cheatsheet-agents.md)
- [İş Akışı Hızlı Rehberi](./5-HızlıBaşvuru/cheatsheet-workflows.md)

---

## Bu Kursu Nasıl Kullanabilirsiniz

### Tavsiye Edilen Öğrenme Sıralaması

1. **Aşama aşama ilerleyin** - Her aşama bir önceki aşamanın üzerine inşa edilir
2. **Teori ve pratiği birleştirin** - Her bölümde alıştırma var, önce anlayın sonra uygulayın
3. **Tüm alıştırmaları tamamlayın** - Alıştırmalar öğrenme yolunun temel bölümleridir

### Öğrenme Tavsiyeleri

- **Başlangıç aşaması**: 2-3 saatte tamamlanması tavsiye edilir, temel kavramlar ve işlemlere alışın
- **Çekirdek yetenekler**: 8-12 saat tavsiye edilir, her modülü elinizle uygulayın
- **Uzman yolu**: 6-8 saat tavsiye edilir, belirli proje deneyimi gerektirir
- **Pratik projeler**: 10-15 saat tavsiye edilir, ilginizi çeken projeleri derinlemesine inceleyin

### Öğrenme Kontrol Noktaları

Her aşamayı tamamladıktan sonra, şu yeteneklere sahip olduğunuzu doğrulayın:
- Tüm işlemleri otonom olarak tamamlayabilirsiniz
- Temel kavramları başkasına açıklayabilirsiniz
- Bilgiyi diğer senaryolara uygulayabilirsiniz

---

## Ön Koşullar

### Temel Bilgiler
- Komut satırı temelleri (temel komutları bilme)
- JavaScript/Node.js temelleri (bazı ileri düzey özellikler için gerekli)
- Git temelleri (versiyon kontrol kavramları)

### Ortam Gereksinimleri
- Node.js >= 18
- npm/pnpm/yarn/bun herhangi biri
- Metin düzenleyici (VS Code tavsiye edilir)

### Önceki Dersler
Yok. Başlangıç aşaması sıfır bilgiyle başlayanlar için tasarlanmıştır.

---

## Öğrenme Yol Haritası

```
┌─────────────────────────────────────────────────────────────────┐
│                     Learn-ECC Öğrenme Yol Haritası             │
└─────────────────────────────────────────────────────────────────┘

    Aşama 1: Başlangıç
        │
        ↓
    ┌───────────────────────┐
    │   Aşama 2: Çekirdek     │←───────────────────┐
    │   Yetenekler           │                    │
    │  (Komut/Agent/Hooks/   │                    │
    │   Skills/Rules)        │                    │
    └───────────┬───────────┘                    │
                │                                │
                ↓                                │
    ┌───────────────────────┐                    │
    │   Aşama 3: Uzman Yolu  │                    │
    │  (MCP/Özerk Agent/    │                    │
    │   İleri Kalıplar)     │                    │
    └───────────┬───────────┘                    │
                │                                │
                ↓                                │
    ┌───────────────────────┐                    │
    │   Aşama 4: Pratik      │────────────────────┘
    │   Projeler             │
    └───────────┬───────────┘
                │
                ↓
    ┌───────────────────────┐
    │   Aşama 5: Hızlı Başvuru │ ← Her zaman başvurulabilir
    │   (Komut/Agent rehber) │
    └───────────────────────┘
```

**Tahmini toplam öğrenme süresi**: 26-38 saat (yaklaşık 2-3 hafta)

---

## Dizin Yapısı

```
Learn-ECC/
├── 1-Başlangıç/                         # Aşama 1: Başlangıç (2-3 saat)
│   ├── 01-ECC-Tanıtım.md                # ECC'ye genel bakış, temel değerler, mimari
│   ├── 02-Kurulum.md                   # Ortam hazırlama, kurulum, doğrulama
│   ├── 03-TemelKomutlar.md             # /plan, /code-review, /build-fix
│   ├── 04-İlkGörev.md                  # Tam geliştirme iş akışı örneği
│   └── exercises/
│       └── BaşlangıçAlıştırmaları.md   # 4 başlangıç alıştırması
│
├── 2-ÇekirdekYetenekler/                # Aşama 2: Çekirdek Yetenekler (8-12 saat)
│   ├── 01-KomutSistemi/
│   ├── 02-Agentİşbirliği/
│   ├── 03-HooksÖzelleştirme/
│   ├── 04-SkillsGeliştirme/
│   └── 05-RulesYazma/
│
├── 3-UzmanYolu/                         # Aşama 3: Uzman Yolu (6-8 saat)
│   ├── 01-MCPGeliştirme.md
│   ├── 02-OtonomAcenteler.md
│   └── 03-İleriDüzeyDesenler.md
│
├── 4-PratikProjeler/                    # Aşama 4: Pratik Projeler (10-15 saat)
│   ├── project-1-cli.md                 # CLI aracı geliştirme
│   ├── project-2-review.md             # Otomatik kod inceleme hatları
│   ├── project-3-auto.md               # İş akışı otomasyonu
│   └── project-4-agent.md              # Özel Agent geliştirme
│
├── 5-HızlıBaşvuru/                     # Aşama 5: Hızlı Başvuru
│   ├── cheatsheet-commands.md          # Komut hızlı rehberi
│   ├── cheatsheet-agents.md            # Agent hızlı rehberi
│   └── cheatsheet-workflows.md         # İş akışı hızlı rehberi
│
├── assets/                              # Görsel kaynaklar
├── progress-tracker.md                  # Öğrenme ilerleme takibi
├── contributing.md                      # Katkı rehberi
└── README.md                           # Bu dosya
```

---

## Başarı İçin İpuçları

1. **Adım adım ilerleyin**: Aşamaları atlamayın, her Aşama bir sonrakinin temelidir
2. **Pratik yapın**: Her kavram öğrendiğinizde hemen uygulayın, sadece okumayın
3. **Alıştırmaları tamamlayın**: Her Aşama'nın alıştırmaları var, bilgiyi pekiştirmek için tamamlayın
4. **Hızlı rehberleri kullanın**: Günlük kullanımda ezber yapmak yerine hızlı rehberleri kullanın
5. **Proje odaklı olun**: 4. Aşama gerçek projelerle tüm bilgileri birlestirir
6. **Not tutun**: Öğrenme deneyimlerinizi ve karşılaştığınız sorunları kaydedin
7. **Diğerlerine öğretin**: Başkalarına öğretmek, bilgiyi gerçekten öğrendiğiniz anlamına gelir

---

## Sıkça Sorulan Sorular (SSS)

### S: Programlama temeli gerekli mi?

C: Temel komut satırı ve programlama bilgisi gerekli (JavaScript/Node.js temelleri bazı özellikleri anlamak için faydalı), ancak herhangi bir dilde uzmanlık gerekmez. Başlangıç aşaması sıfır bilgiyle başlayanlar için tasarlanmıştır.

### S: Bazı aşamaları atlayabilir miyim?

C: Aşama 1-3'ün sırayla tamamlanması tavsiye edilir, Aşama 4 ilgi alanlarınıza göre seçim yapabilirsiniz, Aşama 5 her zaman başvurulabilir.

### S: Tamamladıktan sonra ne yapabilirim?

C: Üretim düzeyinde Claude Code iş akışlarını kullanabilir, kod geliştirme, inceleme, test ve otomasyonu verimli bir şekilde yapabilir; özel Agent sistemleri tasarlayabilir ve uygulayabilir; Hooks ve Skills özelleştirebilirsiniz.

### S: Öğrenme sürecinde sorunlarla karşılaşırsam ne yapmalıyım?

C: 1) [Referans belgelerine](./ReferansBelgeleri/) ilgili bölümlere bakın; 2) [Komut hızlı rehberine](./5-HızlıBaşvuru/cheatsheet-commands.md) bakın; 3) Yardım için Issue açın.

### S: Öğrenme etkisini nasıl doğrulayabilirim?

C: İlerlemenizi takip etmek için [progress-tracker.md](./progress-tracker.md) kullanın, her aşamanın açık öğrenme hedefleri var, tüm alıştırmaları tamamlamak o aşamadaki bilgiyi edindiğiniz anlamına gelir.

### S: Kurs içeriği güncellenecek mi?

C: Evet, düzenli güncellemeler olacak, içerik katkıda bulunmak için PR gönderin. Detaylı kılavuz için [contributing.md](./contributing.md) bakın.

---

## Katkı ve Geri Bildirim

Öğrenme sürecinde bir sorun fark ederseniz veya iyileştirme öneriniz varsa, Issue veya Pull Request gönderebilirsiniz.

---

*Son güncelleme: 2026-05-26*