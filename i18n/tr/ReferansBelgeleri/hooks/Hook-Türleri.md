# Hook Türleri

## Genel Bakış

ECC Hooks Sistemi, Claude Code araç yürütmesinin öncesinde ve sonrasında tetiklenen olay güdümlü bir otomasyon mekanizmasıdır. Kod kalitesini zorunlu kılmak, hataları erken tespit etmek ve tekrarlayan kontrolleri otomatikleştirmek için kullanılır.

```
Kullanıcı isteği → Claude araç seçer → PreToolUse hook çalışır → Araç yürütülür → PostToolUse hook çalışır
```

## Hook Türleri Detayı

### 1. PreToolUse Hookları

#### Tür Açıklaması
PreToolUse hookları araç yürütülmeden **önce** çalışır. Engelleyebilir (exit code 2) veya uyarı verebilir (stderr çıktısı verir ama engellemez).

#### Tetiklenme Zamanı
- Herhangi bir araç (Edit, Write, Bash, Read vb.) yürütülmeden önce
- Tüm araç işlemleri için ön kontrol uygundur

#### Parametre Açıklaması
```typescript
interface HookInput {
  tool_name: string;          // Araç adı: "Bash", "Edit", "Write", "Read" vb.
  tool_input: {
    command?: string;         // Bash: Yürütülecek komut
    file_path?: string;        // Edit/Write/Read: Hedef dosya yolu
    old_string?: string;       // Edit: Değiştirilen metin
    new_string?: string;       // Edit: Değiştirme metni
    content?: string;          // Write: Dosya içeriği
  };
}
```

#### Çıkış Kodları
| Çıkış kodu | Anlamı | Davranış |
|--------|------|------|
| 0 | Başarılı | Devam et, uyarı çıktısı yok |
| 2 | Engelle | Araç yürütmeyi engelle |
| Diğer sıfır olmayan | Hata | Günlüğe kaydet ama engelleme |

#### Kullanım Örneği

**Geliştirme sunucusu engelleyici (tmux dışında npm run dev çalıştırmayı engelle):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "Tmux dışında geliştirme sunucusu çalıştırmayı engelle"
}
```

**Dokümantasyon dosyası uyarısı (standart dışı .md dosyalarını engelle):**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "Standart dışı dokümantasyon dosyası oluşturma konusunda uyar"
}
```

#### Notlar
- PreToolUse **engelleyici** hook'tur, hızlı yürütülmesi zorunludur (<200ms)
- PreToolUse hook'larında ağ çağrısı yapmayın
- Engelleme işlemi (exit 2) dikkatli kullanılmalı, yalnızca kritik güvenlik kontrolleri için

---

### 2. PostToolUse Hookları

#### Tür Açıklaması
PostToolUse hookları araç yürütüldükten **sonra** çalışır. Çıktıyı analiz edebilir ama araç yürütmeyi engelleyemez.

#### Tetiklenme Zamanı
- Herhangi bir araç tamamlandıktan sonra çalışır
- Aracın çıktı sonuçlarına erişebilir

#### Parametre Açıklaması
```typescript
interface HookInput {
  tool_name: string;          // Araç adı
  tool_input: {               // PreToolUse ile aynı
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // Yalnızca PostToolUse'ta mevcut
    output?: string;          // Komut/aracın çıktısı
  };
}
```

#### Çıkış Kodları
| Çıkış kodu | Anlamı |
|--------|------|
| 0 | Başarılı, devam et |
| Sıfır olmayan | Hata, günlüğe kaydet ama araç yürütmeyi engelleme |

#### Kullanım Örneği

**PR günlükleme (gh pr create sonrası PR URL'sini kaydet):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "PR oluşturulduktan sonra URL'yi ve inceleme komutunu kaydet"
}
```

**Kalite kapısı kontrolü (düzenlemeden sonra kalite kontrolü çalıştır):**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Dosya düzenlemesinden sonra kalite kapısı kontrolü çalıştır"
}
```

#### Notlar
- PostToolUse hook'ları yürütmeyi **engelleyemez**
- Asenkron olarak ayarlanabilir (async: true) engellememek için
- Asenkron hook'lar 30 saniye içinde tamamlanmalı

---

### 3. Stop Hookları

#### Tür Açıklaması
Stop hook'ları her Claude yanıtından sonra çalışır. Toplu işleme, son kontroller ve oturum durumu kalıcılığı için kullanılır.

#### Tetiklenme Zamanı
- Her kullanıcı isteği işlendikten ve Claude yanıt oluşturduktan sonra
- Oturumun son yanıtından sonra gerçekleşir

#### Parametre Açıklaması
Stop hook'ları PostToolUse ile aynı girdiyi alır, araç çağrı geçmişinin tamamını içerir.

#### Kullanım Örneği

**Konsol günlüğü kontrolü (yanıttan sonra değiştirilen dosyalardaki console.log'u kontrol et):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "Değiştirilen dosyalarda console.log olup olmadığını kontrol et"
}
```

**Toplu biçimlendirme ve tür kontrolü:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "Tüm düzenlenen JS/TS dosyalarını toplu biçimlendir ve tür kontrolü yap"
}
```

**Oturum özeti kalıcılığı:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "Oturum durumunu kaydet"
}
```

#### Notlar
- Stop hook'ları her yanıttan sonra çalışır, toplu işlemler için uygundur
- Async mod kullanılabilir, engellemez
- Biçimlendirme/Tür kontrolü Stop zamanında toplu olarak çalıştırmaya uygundur

---

### 4. SessionStart Hookları

#### Tür Açıklaması
SessionStart hook'ları oturum yaşam döngüsü **başladığında** tetiklenir. Önceki bağlamı yüklemek ve proje durumunu algılamak için kullanılır.

#### Tetiklenme Zamanı
- Yeni oturum başladığında
- Claude Code başlatıldığında veya yeni oturum oluşturulduğunda

#### Kullanım Örneği

**Önceki bağlamı yükle ve paket yöneticisini algıla:**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "Oturum başladığında sınırlı önceki bağlamı yükle ve proje durumunu algıla",
  "blocking": false
}
```

#### Notlar
- Yaşam döngüsü hook'u, hızlı tamamlanması zorunludur (<30 saniye)
- Ekstra bağlam boyutu `ECC_SESSION_START_MAX_CHARS` ortam değişkeni ile kontrol edilebilir
- `ECC_SESSION_START_CONTEXT=off` ile tamamen devre dışı bırakılabilir

---

### 5. SessionEnd Hookları

#### Tür Açıklaması
SessionEnd hook'ları oturum sona erdiğinde tetiklenir. Oturum sonu özetini kalıcı hale getirmek ve temizlik için kullanılır.

#### Tetiklenme Zamanı
- Oturum sona erdiğinde
- Oturum transcript meta verileri kullanılabilir olduğunda

#### Kullanım Örneği

**Oturum sonu işaretleyici:**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "Transcript meta verileri kullanılabilir olduğunda oturum sonu özetini kalıcı hale getir",
  "blocking": false
}
```

#### Notlar
- Temel olarak asenkron yürütme, oturum sona ermesini engellememeli
- Metrik toplama ve temizlik için yaşam döngüsü işaretleyicisi kullanılır

---

### 6. PreCompact Hookları

#### Tür Açıklaması
PreCompact hook'ları bağlam sıkıştırılmadan **önce** tetiklenir. Durumu kaydetmek için kullanılır.

#### Tetiklenme Zamanı
- Bağlam sıkıştırma/temizleme öncesinde
- Bağlam penceresi dolmak üzereyken

#### Kullanım Örneği

**Bağlam sıkıştırma öncesi durumu kaydet:**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "Bağlam sıkıştırma öncesinde oturum durumunu kalıcı hale getir",
  "blocking": false
}
```

#### Notlar
- Uzun görevlerin durumunu kaydetmek için uygundur
- Engellemez, sıkıştırma akışını etkilemez

---

### 7. PostToolUseFailure Hookları

#### Tür Açıklaması
PostToolUseFailure hook'ları araç yürütmesi **başarısız olduktan sonra** tetiklenir.

#### Tetiklenme Zamanı
- Herhangi bir araç çağrısı başarısız olduktan sonra

#### Kullanım Örneği

**MCP sağlık kontrolü (başarısız MCP çağrılarını takip et):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "Başarısız MCP araç çağrılarını takip et, sağlıksız sunucuları işaretle ve yeniden bağlanmayı dene"
}
```

#### Notlar
- Yeniden deneme mantığı veya sağlık durumu takibi için kullanılabilir
- Aracın başarısız durumunu değiştirmez

---

## Hook Türleri Karşılaştırma Tablosu

| Tür | Tetiklenme Zamanı | Engelleyebilir mi | Engelleyebilir mi | Tipik Kullanım |
|------|----------|----------|----------|----------|
| PreToolUse | Araç yürütmeden önce | Evet (exit 2) | Evet (senkron) | Kalite kontrolleri, Güvenlik doğrulama |
| PostToolUse | Araç yürütüldükten sonra | Hayır | İsteğe bağlı (async) | Günlükleme, analiz |
| Stop | Her yanıttan sonra | Hayır | İsteğe bağlı (async) | Toplu biçimlendirme, oturum özeti |
| SessionStart | Oturum başladığında | Hayır | Hayır | Bağlam yükleme, proje algılama |
| SessionEnd | Oturum sona erdiğinde | Hayır | Hayır | Özet kalıcılığı, temizlik |
| PreCompact | Sıkıştırma öncesi | Hayır | Hayır | Durum kaydetme |
| PostToolUseFailure | Araç başarısız olduktan sonra | Hayır | Hayır | Sağlık kontrolü, yeniden deneme |

---

## Ortam Değişkeni Kontrolü

Hook davranışı ortam değişkenleri ile kontrol edilebilir:

```bash
# minimal | standard | strict (varsayılan: standard)
export ECC_HOOK_PROFILE=standard

# Belirli hook ID'lerini devre dışı bırak (virgülle ayrılmış)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# GateGuard'ı devre dışı bırak
export ECC_GATEGUARD=off

# SessionStart ekstra bağlam boyutunu kontrol et (varsayılan: 8000 karakter)
export ECC_SESSION_START_MAX_CHARS=4000

# SessionStart ekstra bağlamını tamamen devre dışı bırak
export ECC_SESSION_START_CONTEXT=off
```
