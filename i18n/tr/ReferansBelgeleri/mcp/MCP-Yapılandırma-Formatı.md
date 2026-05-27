# MCP Yapılandırma Formatı

## Genel Bakış

MCP (Model Context Protocol) sunucuları `~/.claude.json` dosyasındaki `mcpServers` bölümü aracılığıyla yapılandırılır. Claude Code iki tür MCP Sunucusu destekler:

- **İşlem modu (process)**: Yerel komutla başlatılan sunucular
- **HTTP modu (http)**: HTTP/HTTPS üzerinden erişilen uzak sunucular

## JSON Yapılandırma Yapısı

```json
{
  "mcpServers": {
    "SunucuAdı": {
      "type": "http",           // İsteğe bağlı, varsayılan process
      "command": "npx",        // İşlem modu için gerekli
      "url": "https://...",    // HTTP modu için gerekli
      "args": ["parametre1", "parametre2"],  // İsteğe bağlı
      "env": {                 // İsteğe bağlı, ortam değişkenleri
        "ANAHTAR": "değer"
      },
      "headers": {             // İsteğe bağlı, HTTP isteği başlıkları
        "Baslik-Adi": "değer"
      },
      "description": "Sunucu işlevi tanımı"  // İsteğe bağlı
    }
  }
}
```

## Alan Açıklaması

| Alan | Tür | Gerekli | Açıklama |
|------|------|------|------|
| `type` | string | Hayır | Bağlantı türü: `process` (varsayılan) veya `http` |
| `command` | string | type=process时必需 | Başlatma komutu (örn. `npx`, `uvx`, `python3`) |
| `url` | string | type=http时必需 | MCP Sunucusu'nun HTTP/HTTPS URL'si |
| `args` | array | Hayır | Komut için iletilen parametre dizisi |
| `env` | object | Hayır | Ortam değişkeni anahtar-değer çiftleri |
| `headers` | object | Hayır | HTTP isteği başlıkları (yalnızca http modu) |
| `description` | string | Hayır | Sunucu işlevi tanımı |

## Yapılandırma Örneği

### İşlem Modu Örneği

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "GitHub işlemleri - PR'ler, issue'lar, repolar"
    }
  }
}
```

### HTTP Modu Örneği

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Vercel dağıtımı ve projeleri"
    }
  }
}
```

### Kimlik Doğrulama Başlıklı HTTP Modu

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "ANAHTARINIZI_BURAYA_YAZIN"
      },
      "description": "AI tarayıcı aracısı"
    }
  }
}
```

## En İyi Uygulamalar

1. **Ortam Değişkeni Yönetimi**: Hassas bilgiler (API anahtarları, tokenlar) için ortam değişkenleri kullanın, kesinlikle sabit kodlamayın
2. **Bağlam Penceresi**: Bağlam penceresini korumak için 10'dan fazla MCP Sunucusu etkinleştirmemeye çalışın
3. **Sunucuları Devre Dışı Bırakma**: Paketlenmiş MCP Sunucularını `ECC_DISABLED_MCPS=server1,server2` ortam değişkeniyle devre dışı bırakabilirsiniz

## Sorun Giderme

- Komutun çalıştırılabilir olduğundan emin olun (npx, uvx vb. kurulu)
- Ortam değişkenlerinin doğru ayarlandığını kontrol edin
- Sunucu bağlantı hatalarını Claude Code günlüklerinde inceleyin
