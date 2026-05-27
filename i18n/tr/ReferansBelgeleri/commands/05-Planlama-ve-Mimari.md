# Planlama ve Mimari Komutları

## Genel Bakış

Planlama ve mimari komutları, ürün planlaması, özellik tasarımı ve sistem mimari kararları için kullanılır. Bu komutlar, kod yazmadan önce net bir planın olduğundan emin olur, yeniden işi azaltır ve kod kalitesini artırır.

## Komut Listesi

### /plan

**Kullanım**: Genel uygulama planlaması - Gereksinimleri yeniden ifade eder, riskleri değerlendirir, adım adım uygulama planı oluşturur

**Tanım**: Herhangi bir koda dokunmadan önce, kullanıcı onayını bekledikten sonra kapsamlı bir uygulama planı oluşturur. Serbest form gereksinimleri veya PRD markdown dosyalarını kabul eder.

**Giriş Modları**:

| Giriş | Mod | Davranış |
|---|---|---|
| `path/to/name.prd.md` | PRD artifact modu | PRD'yi okur, işlenecek bir sonraki teslimat dönüm noktasını seçer, `.claude/plans/{name}.plan.md` oluşturur |
| Diğer markdown yolları | Referans modu | Dosyayı bağlam olarak okur, satır içi plan üretir |
| Serbest metin | Sohbet modu | Satır içi plan üretir |
| Boş giriş | Netleştirme modu | Ne planlanacağını sorar |

**Temel Parametreler**:
- `$ARGUMENTS`: `[özellik açıklaması | path/to/*.prd.md]` - Özellik tanımı veya PRD dosya yolu

**İş Akışı**:
1. **Gereksinimleri yeniden ifade et** - Ne inşa edilmesi gerektiğini netleştir
2. **Riskleri tanımla** - Potansiyel sorunları ve engelleri listele
3. **Adım adım plan oluştur** - Uygulamayı aşamalara ayrıştır
4. **Onay bekler** - Devam etmeden önce kullanıcı onayını zorunlu kıl

**Çıktı Formatı** (PRD artifact modu):
```markdown
# Plan: {Özellik Adı}

**Kaynak PRD**: {yol}
**Seçili Kilometre Taşı**: {kilometre taşı veya aşama adı}
**Karmaşıklık**: {Küçük | Orta | Büyük}

## Özet
{2-3 cümle}

## Taklit Edilecek Desenler
| Kategori | Kaynak | Desen |
|---|---|---|
| Adlandırma | `yol:satır` | {kısa açıklama} |
| Hatalar | `yol:satır` | {kısa açıklama} |
| Testler | `yol:satır` | {kısa açıklama} |

## Değiştirilecek Dosyalar
| Dosya | Eylem | Neden |
|---|---|---|
| `yol` | OLUŞTUR / GÜNCELLE / SİL | {sebep} |

## Görevler
### Görev 1: {ad}
- **Eylem**: {ne yapılacak}
- **Taklit**: {izlenecek desen}
- **Doğrula**: {doğruluğu kanıtlayan komut}

## Doğrulama
```bash
{proje özgü doğrulama komutları}
```

## Riskler
| Risk | Olasılık | Azaltma |
|---|---|---|
```

**En İyi Uygulamalar**:
- Kullanmadan önce kod tabanındaki mevcut desenleri araştır
- PRD artifact modunda `.claude/plans/` dizinini oluştur (yoksa)
- PRD `Delivery Milestones` tablosu içeriyorsa, yalnızca seçili satır durumunu güncelle

**Yaygın Tuzaklar**:
- Boş girişle karşılaşınca sormadan planlamaya çalışmak
- Kullanıcı onayını beklemeden kod yazmaya başlamak
- Desen Temelleme Adımlarını atlamak

**Diğer Komutlarla Entegrasyon**:
- Planladıktan sonra test güdümlü geliştirme için `tdd-workflow` skill kullan
- Yapı hatalarını onarmak için `/build-fix` kullan
- Uygulananı incelemek için `/code-review` kullan
- Pull Request açmak için `/pr` veya `/prp-pr` kullan

---

### /plan-prd

**Kullanım**: İnteraktif PRD üretici

**Tanım**: Problem odaklı ürün gereksinimleri dokümanı oluşturur, problem tanımı, kullanıcı profilleri, başarı metrikleri ve MVP kapsamını içerir.

**Kullanım Senaryosu**:
- İnşa edilecek özellik belirlendiğinde
- Ürün gereksinimlerini netleştirmek gerektiğinde
- Sıfırdan yeni özellik planlaması

**İş Akışı**:
1. ÇERÇEVELE - Kullanıcıyı ve problemi anla
2. TEMELLE - Kanıtları topla
3. KARAR VER - Kapsamı ve varsayımları belirle
4. ÜRET - PRD dosyası oluştur

---

### /prp-plan

**Kullanım**: Kapsamlı özellik planlaması

**Tanım**: Kod tabanı analizi, risk değerlendirmesi ve adım adım uygulama planını kapsayan tam proje planlaması.

**Kullanım Senaryosu**:
- Karmaşık özelliklerin detaylı planlaması
- Kod tabanı bağlam analizi gerekli olduğunda
- Çok aşamalı uygulama projeleri

---

### /prp-prd

**Kullanım**: PRP iş akışı PRD üretici

**Tanım**: PRP (Plan-Record-Produce) iş akışı ile ürün gereksinimleri dokümanı oluşturur.

---

### /prp-implement

**Kullanım**: PRP planı+doğrulama döngüsünü yürüt

**Tanım**: Sürekli doğrulama ve kontrollerle PRP iş akışını yürütür.

---

### /prp-pr

**Kullanım**: PRP iş akışından PR oluştur

**Tanım**: PRP iş akışının sonuçlarından GitHub Pull Request oluşturur.

---

### /prp-commit

**Kullanım**: PRP doğrulama işlemi

**Tanım**: PRP iş akışında doğrulama yürütür ve kodu işler.

---

### /multi-plan

**Kullanım**: Çoklu model işbirliği planlaması

**Tanım**: Codex ve Gemini'nin çift model analizini birleştirir, kapsamlı uygulama planı üretir.

**Kullanım Senaryosu**:
- Çoklu perspektif analiz gerekli olduğunda
- Karmaşık çapraz alan projeleri
- Ön ve arka uç değerlendirmelerini dengeleme gerekli olduğunda

---

## İlgili Komutlar

- `/plan` - Genel uygulama planlaması
- `/code-review` - Kod inceleme
- `/build-fix` - Yapı onarımı
