# Yerleşik MCP Sunucuları

ECC projesi `mcp-configs/mcp-servers.json` dosyasında birden fazla yaygın MCP Sunucusu yapılandırması önceden tanımlanmış olarak sağlar.

## İşlevlere Göre Sınıflandırma

### Geliştirme Araçları

| Sunucu | Tür | Tanım |
|--------|------|------|
| `github` | process | GitHub işlemleri - PR'ler, issue'lar, repolar |
| `jira` | process | Jira sorun takibi - arama, oluşturma, güncelleme, yorum, sorun geçişleri |
| `confluence` | process | Confluence Cloud entegrasyonu - sayfa arama, içerik alma, alan keşfi |
| `filesystem` | process | Dosya sistemi işlemleri (yol yapılandırması gerekli) |
| `memory` | process | Oturumlar arası kalıcı bellek |
| `omega-memory` | process | Gelişmiş kalıcı aracı belleği, anlamsal arama, çoklu aracı işbirliği ve bilgi grafiği destekler |
| `sequential-thinking` | process | Zincirleme düşünce akışı |

### Dağıtım ve Altyapı

| Sunucu | Tür | Tanım |
|--------|------|------|
| `vercel` | http | Vercel dağıtımı ve projeleri |
| `railway` | process | Railway dağıtımı |
| `cloudflare-docs` | http | Cloudflare dokümantasyon araması |
| `cloudflare-workers-builds` | http | Cloudflare Workers derlemeleri |
| `cloudflare-workers-bindings` | http | Cloudflare Workers bağlamaları |
| `cloudflare-observability` | http | Cloudflare gözlemlenebilirlik/günlükleri |

### Veritabanı

| Sunucu | Tür | Tanım |
|--------|------|------|
| `supabase` | process | Supabase veritabanı işlemleri |
| `clickhouse` | http | ClickHouse analitik sorguları |

### AI ve Makine Öğrenmesi

| Sunucu | Tür | Tanım |
|--------|------|------|
| `fal-ai` | process | fal.ai modelleri ile AI görsel/video/ses oluşturma |
| `exa-web-search` | process | Exa API ile web araması ve araştırma |
| `context7` | process | Gerçek zamanlı dokümantasyon sorgusu - /docs komutu ve documentation-lookup becerisiyle birlikte kullanılır |
| `magic` | process | Magic UI bileşenleri |
| `evalview` | process | AI aracı regresyon testi - Anlık görüntü davranışı, araç çağrılarında ve çıktı kalitesinde regresyon tespiti |

### Tarayıcı Otomasyonu

| Sunucu | Tür | Tanım |
|--------|------|------|
| `playwright` | process | Playwright ile tarayıcı otomasyonu ve test |
| `browserbase` | process | Browserbase bulut tarayıcı oturumları |
| `browser-use` | http | AI tarayıcı aracısı, web görevlerini yürütür |

### Diğer Araçlar

| Sunucu | Tür | Tanım |
|--------|------|------|
| `firecrawl` | process | Web kazıma ve tarama |
| `longhand` | process | Kayıpsız Claude Code oturum geçmişi - Ham araç çağrılarını yerel SQLite + ChromaDB'ye indeksler |
| `token-optimizer` | process | Token optimizasyonu - İçerik tekilleştirme ve sıkıştırma ile %95+ bağlam küçültme gerçekleştirir |
| `devfleet` | http | Çoklu Aracı Orkestrasyonu - İzole worktree'lerde paralel Claude Code aracılarını planlar |
| `laraplugins` | http | Laravel eklenti keşfi - Anahtar kelime ile paket arama, sağlık skorları, Laravel/PHP sürüm uyumluluğu |

## Hızlı Etkinleştirme

Gerekli sunucu yapılandırmasını `~/.claude.json` dosyasındaki `mcpServers` bölümüne kopyalayın, `YOUR_*_HERE` yer tutucularını gerçek değerlerle değiştirin.

**Örnek - GitHub sunucusunu etkinleştirme:**

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_gerçek_token"
      },
      "description": "GitHub işlemleri - PR'ler, issue'lar, repolar"
    }
  }
}
```

## Ortam Değişkeni Gereksinimleri

| Sunucu | Gerekli ortam değişkenleri |
|--------|----------------|
| `github` | `GITHUB_PERSONAL_ACCESS_TOKEN` |
| `jira` | `JIRA_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN` |
| `firecrawl` | `FIRECRAWL_API_KEY` |
| `supabase` | `--project-ref` parametresi |
| `exa-web-search` | `EXA_API_KEY` |
| `fal-ai` | `FAL_KEY` |
| `browserbase` | `BROWSERBASE_API_KEY` |
| `browser-use` | `x-browser-use-api-key` isteği başlığı |
| `confluence` | `CONFLUENCE_BASE_URL`, `CONFLUENCE_EMAIL`, `CONFLUENCE_API_TOKEN` |
| `evalview` | `OPENAI_API_KEY` (isteğe bağlı) |
