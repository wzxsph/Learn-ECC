# Özel MCP Sunucusu Geliştirme

## Genel Bakış

MCP Sunucusu, Claude Code'un yeteneklerini genişletmek için kullanılan Model Context Protocol üzerine inşa edilmiştir. Bu kılavuz, özel MCP Sunucusu geliştirme ve entegrasyonunu açıklar.

## Sunucu Türleri

### İşlem Modu Sunucuları

Yerel işlem başlatma için uygun, yerel yürütme veya yerel kaynaklara erişim gerektiren hizmetler için idealdir.

```json
{
  "command": "node",
  "args": ["/sunucu/betik/yolu.js"],
  "env": {
    "API_ANAHTARINIZ": "değer"
  }
}
```

### HTTP Modu Sunucuları

HTTP/HTTPS üzerinden uzak sunuculara erişim için, mikrohizmet mimarisi veya bulut hizmetleri için uygundur.

```json
{
  "type": "http",
  "url": "https://mcp-sunucunuz.ornek.com/mcp",
  "headers": {
    "Authorization": "Bearer TOKENINIZ"
  }
}
```

## Geliştirme Süreci

### 1. MCP SDK'yı Kullanma

Resmi olarak birden fazla dilde SDK sağlanır:

```bash
# Node.js
npm install @modelcontextprotocol/sdk

# Python
pip install mcp
```

### 2. Sunucu Gerçekleştirme

**Node.js Örneği:**

```javascript
const { Server } = require('@modelcontextprotocol/sdk');
const { StdioServerTransport } = require('@modelcontextprotocol/sdk');

const server = new Server(
  {
    name: "my-custom-server",
    version: "1.0.0"
  },
  {
    capabilities: {
      tools: {},
      resources: {}
    }
  }
);

// Araçları kaydet
server.setRequestHandler('tools/list', async () => {
  return {
    tools: [
      {
        name: "my_tool",
        description: "Özel işlem yürüt",
        inputSchema: {
          type: "object",
          properties: {
            param: { type: "string" }
          }
        }
      }
    ]
  };
});

// Araç çağrılarını işle
server.setRequestHandler('tools/call', async (request) => {
  const { name, arguments: args } = request.params;

  if (name === "my_tool") {
    // İşlemi yürüt
    return { content: [{ type: "text", text: "Sonuç" }] };
  }
});

// Sunucuyu başlat
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
}

main();
```

**Python Örneği:**

```python
from mcp.server import Server
from mcp.server.stdio import stdio_server

server = Server("my-custom-server")

@server.list_tools()
async def list_tools():
    return [
        Tool(
            name="my_tool",
            description="Özel işlem yürüt",
            inputSchema={
                "type": "object",
                "properties": {
                    "param": {"type": "string"}
                }
            }
        )
    ]

@server.call_tool()
async def call_tool(name, arguments):
    if name == "my_tool":
        # İşlemi yürüt
        return [TextContent(type="text", text="Sonuç")]

async def main():
    async with stdio_server() as (read_stream, write_stream):
        await server.run(read_stream, write_stream, server.create_initialization_options())
```

## Yapılandırma ve Entegrasyon

### 1. Yerel Test

```json
{
  "mcpServers": {
    "my-custom": {
      "command": "node",
      "args": ["/mutlak/yol/betik.js"],
      "env": {
        "DEBUG": "true"
      }
    }
  }
}
```

### 2. ECC'ye Ekleme

Yapılandırmayı `mcp-configs/mcp-servers.json` dosyasına ekleyin:

```json
{
  "mcpServers": {
    "my-custom": {
      "command": "npx",
      "args": ["-y", "/sunucu/betik/yolu"],
      "env": {
        "API_ANAHTARINIZ": "ANAHTARINIZI_BURAYA_YAZIN"
      },
      "description": "Özel sunucu tanımınız"
    }
  }
}
```

## En İyi Uygulamalar

1. **Hata Yönetimi**: Her zaman yapılandırılmış hata yanıtları döndürün
2. **Girdi Doğrulama**: Tüm girdi parametrelerini doğrulayın, geçersiz istekleri reddedin
3. **Zaman Aşımı Kontrolü**: Makul zaman aşımları ayarlayın
4. **Günlükleme**: Hata ayıklama için kritik işlemleri günlüğe kaydedin
5. **Kaynak Temizliği**: Bağlantıların ve kaynakların doğru serbest bırakıldığından emin olun
6. **Sürüm Yönetimi**: Anlamsal sürümlendirmeyi izleyin

## Yaygın Araç Türleri

| Tür | Kullanım |
|------|------|
| `tools/list` | Kullanılabilir araçları listele |
| `tools/call` | Aracı çağır |
| `resources/list` | Kaynakları listele |
| `resources/read` | Kaynağı oku |
| `prompts/list` | İpucu şablonlarını listele |
| `prompts/get` | İpucunu al |

## Referans Kaynaklar

- [MCP Resmi Dokümantasyonu](https://modelcontextprotocol.io/)
- [MCP SDK (Node.js)](https://github.com/modelcontextprotocol/typescript-sdk)
- [MCP SDK (Python)](https://github.com/modelcontextprotocol/python-sdk)
- [MCP Spesifikasyonu](https://modelcontextprotocol.io/specification)
