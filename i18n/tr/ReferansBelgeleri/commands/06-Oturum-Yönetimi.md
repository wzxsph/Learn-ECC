# Oturum Yönetimi Komutları

## Genel Bakış

Oturum yönetimi komutları, Claude Code oturum durumlarını kaydetmek, geri yüklemek ve yönetmek için kullanılır. Bu komutlar, uzun oturumlarda bağlamı korumak, oturumlar arasında iş devretmek ve ilerlemeyi takip etmek için kritik öneme sahiptir.

## Komut Listesi

### /save-session

**Kullanım**: Oturum durumunu kaydet - Mevcut oturumun tam bağlamını tarih dosyasına yazar

**Tanım**: Mevcut oturumun tüm bağlamını `~/.claude/session-data/` dizini altındaki tarihli bir dosyaya kaydeder. Neyin inşa edildiğini, neyin çalıştığını, neyin başarısız olduğunu, kalan işleri, dosya durumlarını, alınan kararları ve çözülmemiş engelleri içerir.

**Oturum Dosyası Adlandırma Kuralları**:
- Format: `YYYY-MM-DD-<kısa-id>-session.tmp`
- Geçerli karakterler: Harfler `a-z`/`A-Z`, rakamlar `0-9`, tire `-`, alt çizgi `_`
- Minimum uzunluk: 1 karakter (ancak çok kısa olması önerilmez)
- Önerilen: Aynı gün çakışmalarını önlemek için 8+ karakter küçük harf/rakam/tire kombinasyonu

**Kullanım Zamanı**:
- Çalışma sonunda (oturum bitmeden önce)
- Bağlam sınırına yaklaşırken (önce kaydet, sonra yeni oturum başlat)
- Karmaşık bir sorunu çözdükten sonra (başarılı kalıpları kaydet)
- Gelecek oturumlara devir gerekli olduğunda

**İş Akışı**:
1. **Bağlamı topla** - Tüm değiştirilen dosyaları oku, tartışmaları gözden geçir
2. **Dizin oluştur** - `mkdir -p ~/.claude/session-data`
3. **Dosyaya yaz** - Zaman damgalı oturum dosyası oluştur
4. **Kullanıcıya göster** - İçeriği görüntüle ve onay iste

**Oturum Dosyası Formatı**:

```markdown
# Session: YYYY-MM-DD

**Started:** [tahmini başlangıç saati, biliniyorsa]
**Last Updated:** [şu anki saat]
**Project:** [proje adı veya yolu]
**Topic:** [Bu oturumun ne hakkında olduğunu tek cümleyle özetleyen]

---

## What We Are Building
[Özellik, hata düzeltme veya görev hakkında 1-3 paragraf]

## What WORKED (kanıtla)
- **[çalışan şey]** — doğrulandı: [spesifik kanıt]

## What Did NOT Work (ve neden)
- **[denenen yaklaşım]** — başarısız oldu çünkü: [tam sebep / hata mesajı]

## What Has NOT Been Tried Yet
- [yaklaşım / fikir]

## Current State of Files
| Dosya              | Durum         | Notlar                      |
| ----------------- | -------------- | -------------------------- |
| `path/to/file.ts` | GEÇTİ: Tamamlandı | [ne yaptığı]             |

## Decisions Made
- **[karar]** — sebep: [Alternatifler yerine bu neden seçildi]

## Blockers & Open Questions
- [engel / açık soru]

## Exact Next Step
[Devam edildiğinde yapılacak en önemli tek şey]
```

**Temel İlkeler**:
- "What Did NOT Work" bölümü en önemlisidir - Gelecek oturumlar başarısız yaklaşımları körü körüne yeniden deneyecektir
- Her oturum bağımsız dosyadır - Önceki oturumlara asla ekleme yapma
- Dosya bir sonraki oturumda `/resume-session` tarafından okunmak içindir

**En İyi Uygulamalar**:
- Ortada kaydedildiğinde (bitiş değil), devam eden işleri açıkça işaretle
- Spesifik hata mesajlarını ve başarısızlık nedenlerini dahil et ("X hatası fırlattı çünkü Y" gibi)
- Bir sonraki adım yeterince spesifik olmalı, düşünmeden nereden başlanacağı bilinmelidir

---

### /resume-session

**Kullanım**: Kaydedilen oturumu geri yükle

**Tanım**: `~/.claude/session-data/` adresinden önceden kaydedilmiş oturum bağlamını yükler ve kurtarır.

**Kullanım Senaryosu**:
- Yeni oturum başlatırken
- Önceki çalışmaya devam etmek gerekli olduğunda
- Önceki sorun çözme süreçlerini gözden geçirmek gerekli olduğunda

---

### /sessions

**Kullanım**: Oturum geçmişini gözden geçir+ara+yönet

**Tanım**: Kaydedilmiş tüm oturum geçmişini gözden geçirir, arar ve yönetir.

**İşlevler**:
- Tüm oturumları listele
- Tarihe göre ara
- Oturum takma adlarını yönet
- Eski oturumları temizle

---

### /checkpoint

**Kullanım**: İş akışı kontrol noktası oluştur/doğrula

**Tanım**: İş akışının kritik düğümlerinde kontrol noktaları oluşturur, ilerlemenin izlenebilir olduğundan emin olur.

**Kullanım Senaryosu**:
- Önemli adımlar tamamlandığında
- Kritik kilometre taşlarını doğrulamak gerekli olduğunda
- Çok aşamalı görevlerde

---

### /aside

**Kullanım**: Bağlamı kaybetmeden yan soruları yanıtla

**Tanım**: Yan paralel diyalog moduna geçer, ikincil sorunları işledikten sonra ana göreve sorunsuz döner.

**Kullanım Senaryosu**:
- Hızlı bir soru sormak gerekli olduğunda
- Dokümantasyona bakmak gerekli olduğunda
- Yardımcı bir görevi yürütmek gerekli olduğunda

---

## İlgili Komutlar

- `/save-session` - Mevcut oturumu kaydet
- `/resume-session` - Oturumu geri yükle
- `/sessions` - Oturum geçmişini yönet
