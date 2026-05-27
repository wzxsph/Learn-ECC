# Diğer Komutlar

## Genel Bakış

Diğer komutlar; Jira entegrasyonu, GAN operasyonları, güvenlik taraması ve daha fazlasını içerir.

## Komut Listesi

### /jira

**Kullanım**: Jira iş takibi ile etkileşim - alma, analiz, yorum, durum geçişi, arama

**Tanım**: Jira Proje Yönetimi aracıyla entegrasyon, doğrudan iş akışlarında Jira biletleriyle çalışmayı destekler.

**Kullanım**:

| Komut | Açıklama |
|---|---|
| `/jira get <BİLET-ANAHTARI>` | Jira biletini al ve analiz et |
| `/jira comment <BİLET-ANAHTARI>` | İlerleme yorumu ekle |
| `/jira transition <BİLET-ANAHTARI>` | Bilet durumunu değiştir |
| `/jira search <JQL>` | JQL ile sorun ara |

**`/jira get` İş Akışı**:
1. Jira'dan bilet al (MCP veya REST API aracılığıyla)
2. Tüm alanları çıkar: özet, tanım, kabul kriterleri, öncelik, etiketler, ilişkili sorunlar
3. Ek bağlam için isteğe bağlı yorumları al
4. Yapılandırılmış analiz üret

**Ön Koşullar** - Jira kimlik bilgileri yapılandırılmış olmalı (iki seçenek):

**Seçenek A — MCP Sunucusu (Önerilen)**:
`mcpServers` yapılandırmasına `jira` ekleyin.

**Seçenek B — Ortam Değişkenleri**:
```bash
export JIRA_URL="https://sirketiniz.atlassian.net"
export JIRA_EMAIL="email.adrestiniz@ornek.com"
export JIRA_API_TOKEN="api-token-değeriniz"
```

**Diğer Komutlarla Entegrasyon**:
- Bilet analizinden sonra uygulama planı oluşturmak için `/plan` kullan
- Test Güdümlü Geliştirme için `tdd-workflow` becerisini kullan
- Gerçekleştirimi incelemek için `/code-review` kullan
- Bilet durumunu güncellemek için `/jira comment` veya `/jira transition` kullan

---

### /gan-build (Geliştirilme Aşamasında)

**Kullanım**: GAN inşa operasyonları

**Tanım**: GAN (Generative Adversarial Network) inşa operasyonları.

---

### /gan-design (Geliştirilme Aşamasında)

**Kullanım**: GAN tasarım operasyonları

**Tanım**: GAN tasarımı ile ilgili operasyonlar.

---

### /prune

**Kullanım**: 30 günden eski olan bekleyen instinct'leri sil

**Tanım**: 30 günden eski olan ve otomatik oluşturulmuş ancak asla gözden geçirilmemiş veya yükseltilmemiş olan eski instinct'leri temizler.

**Kullanım**:
```
/prune                    # 30 günden eski instinct'leri sil
/prune --max-age 60      # Özel gün eşiği
/prune --dry-run         # Silmeden önizle
```

**Gerçekleştirim**:
```bash
python3 "${CLAUDE_PLUGIN_ROOT}/skills/continuous-learning-v2/scripts/instinct-cli.py" prune
```

El ile kurulumda (`CLAUDE_PLUGIN_ROOT` ayarlanmamışsa):
```bash
python3 ~/.claude/skills/continuous-learning-v2/scripts/instinct-cli.py prune
```

**Parametreler**:

| Parametre | Açıklama |
|---|---|
| yok | 30 günden eski instinct'leri sil |
| `--max-age <gün>` | Özel süre eşiği (gün) |
| `--dry-run` | Silinecekleri silmeden önizle |

**En İyi Uygulamalar**:
- Silinecekleri önizlemek için önce `--dry-run` kullan
- Instinct envanterini düzenli tutmak için düzenli olarak çalıştır
- Not: Yalnızca `pending` durumundaki instinct'leri siler, yükseltilmiş olanları silmez

---

### /security-scan

**Kullanım**: Güvenlik taraması

**Tanım**: Kod tabanında güvenlik açıklarını tarar.

**Tarama İçeriği**:
- Sabit Kodlanmış Kimlik Bilgileri
- SQL Enjeksiyonu riskleri
- XSS güvenlik açıkları
- Bağımlılık güvenlik açıkları

---

### /feature-dev (Geliştirilme Aşamasında)

**Kullanım**: Özellik geliştirme asistanı

**Tanım**: Özellik geliştirme için yardımcı iş akışı sağlar.

---

### /cost-report

**Kullanım**: Model maliyet raporu

**Tanım**: AI model kullanım maliyeti raporu üretir.

---

## İlgili Komutlar

- `/security-scan` - Güvenlik taraması
- `/jira` - Jira entegrasyonu
- `/prune` - Instinct temizleme
