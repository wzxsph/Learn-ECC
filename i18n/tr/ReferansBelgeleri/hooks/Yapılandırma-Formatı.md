# Yapılandırma Formatı

## hooks.json Yapısı Genel Bakış

ECC Hooks Sistemi yapılandırma dosyası JSON formatında olup temel yapısı şöyledir:

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "hooks": {
    "PreToolUse": [...],
    "PostToolUse": [...],
    "Stop": [...],
    "SessionStart": [...],
    "SessionEnd": [...],
    "PreCompact": [...],
    "PostToolUseFailure": [...]
  }
}
```

## Üst Düzey Yapı

| Alan | Tür | Açıklama |
|------|------|------|
| `$schema` | string | JSON Schema URL'si, yapılandırmayı doğrulamak için kullanılır |
| `hooks` | object | Tüm hook türlerini içeren nesne |

---

## Hook Dizi Yapısı

Her olay türü (PreToolUse, PostToolUse vb.) bir hook dizisi içerir, her hook nesnesi şu yapıya sahiptir:

```json
{
  "matcher": "Bash|Edit|Write",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/my-hook.js",
      "async": false,
      "timeout": 30
    }
  ],
  "description": "Hook tanımı",
  "id": "unique-hook-id"
}
```

### matcher Alanı

#### Tür Açıklaması
matcher alanı hook'u tetikleyecek araç türlerini belirtmek için kullanılır, tek bir araç veya çoklu araçları destekler (`|` ile ayrılmış).

#### Kullanılabilir Değerler
| matcher değeri | Eşleşen araçlar |
|-----------|-----------------|
| `*` | Tüm araçlar |
| `Bash` | Bash aracı |
| `Edit` | Edit aracı |
| `Write` | Write aracı |
| `Read` | Read aracı |
| `MultiEdit` | MultiEdit aracı |
| `Bash\|Edit\|Write` | Bash, Edit veya Write |
| `Edit\|Write\|MultiEdit` | Edit, Write veya MultiEdit |

#### Kullanım Örneği
```json
// Tüm araçlarla eşleş
"matcher": "*"

// Yalnızca Bash araçlarıyla eşleş
"matcher": "Bash"

// Birden fazla araçla eşleş
"matcher": "Edit|Write|MultiEdit"
```

---

## hooks Dizi Yapısı

hooks dizisi bir veya daha fazla hook komut nesnesi içerir:

### type Alanı

| type değeri | Açıklama |
|--------|------|
| `command` | Node.js komutu yürüt |

### command Alanı

Yürütülecek komut, genellikle Node.js betik yoludur.

### async Alanı (İsteğe Bağlı)

```json
"async": true
```

- `true`: Asenkron yürütme, araç yürütmeyi engellemez
- `false` veya yok: Senkron yürütme, araç yürütmeyi engeller

### timeout Alanı (İsteğe Bağlı)

```json
"timeout": 30
```

Maksimum yürütme süresi (saniye). Önerilen:
- Senkron hook'lar: <200ms
- Asenkron hook'lar: ≤30 saniye

---

## Tam Yapılandırma Örneği

### PreToolUse Hook Örneği

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/pre-bash-dispatcher.js"
    }
  ],
  "description": "Kalite, tmux, push ve GateGuard kontrolleri için Bash ön kontrol dağıtıcısı",
  "id": "pre:bash:dispatcher"
}
```

### PostToolUse Hook Örneği

```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/run-with-flags.js post:quality-gate scripts/hooks/quality-gate.js standard,strict",
      "async": true,
      "timeout": 30
    }
  ],
  "description": "Dosya düzenlemesinden sonra kalite kapısı kontrolü çalıştır",
  "id": "post:quality-gate"
}
```

### Stop Hook Örneği

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/run-with-flags.js stop:session-end scripts/hooks/session-end.js minimal,standard,strict",
      "async": true,
      "timeout": 10
    }
  ],
  "description": "Her yanıttan sonra oturum durumunu kaydet",
  "id": "stop:session-end"
}
```

### SessionStart Hook Örneği

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/session-start-bootstrap.js"
    }
  ],
  "description": "Önceki bağlamı yükle ve yeni oturumun paket yöneticisini algıla",
  "id": "session:start"
}
```

---

## Yaşam Döngüsü Olay Yapılandırma Formatı

SessionStart, SessionEnd, PreCompact gibi yaşam döngüsü olayları için farklı yapılandırma yapısı kullanılır:

```json
{
  "description": "Bellek kalıcılığının yaşam döngüsü hook tanımı",
  "events": [
    {
      "event": "SessionStart",
      "id": "session:start",
      "script": "scripts/hooks/session-start-bootstrap.js",
      "purpose": "Oturum başladığında sınırlı önceki bağlamı yükle ve proje durumunu algıla",
      "blocking": false
    },
    {
      "event": "PreCompact",
      "id": "pre:compact",
      "script": "scripts/hooks/pre-compact.js",
      "purpose": "Bağlam sıkıştırma öncesinde oturum durumunu kalıcı hale getir",
      "blocking": false
    }
  ]
}
```

### Yaşam Döngüsü Olay Alanları

| Alan | Açıklama |
|------|------|
| `event` | Olay türü (SessionStart, PreCompact, SessionEnd vb.) |
| `id` | Benzersiz tanımlayıcı |
| `script` | Betik yolu |
| `purpose` | Kullanım tanımı |
| `blocking` | Engelleme durumu (genellikle false) |

---

## Hook'ları Devre Dışı Bırakma

### Yapılandırmayı Düzenleyerek Devre Dışı Bırakma

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Override: Tüm .md dosyası oluşturmalara izin ver"
      }
    ]
  }
}
```

### Ortam Değişkeni ile Devre Dışı Bırakma

```bash
# Belirli hook'ları devre dışı bırak (virgülle ayrılmış)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# GateGuard'ı devre dışı bırak
export ECC_GATEGUARD=off
```

---

## Hook ID Adlandırma Kuralları

ECC iki nokta üst üste ile ayrılmış adlandırma kuralı kullanır:

| Önek | Kullanım | Örnek |
|------|------|------|
| `pre:` | PreToolUse hook'ları | `pre:bash:dispatcher` |
| `post:` | PostToolUse hook'ları | `post:quality-gate` |
| `stop:` | Stop hook'ları | `stop:session-end` |
| `session:` | Oturum yaşam döngüsü hook'ları | `session:start` |

---

## run-with-flags.js Sarıcı

ECC hook'ları çalıştırmak için `run-with-flags.js` sarıcıyı kullanır, yapılandırma dosyasının çalışma zamanı kapısını destekler:

```
node scripts/hooks/run-with-flags.js <hook-id> <script-path> <profiles>
```

### Parametre Açıklaması
| Parametre | Açıklama |
|------|------|
| `hook-id` | Hook'un benzersiz tanımlayıcısı |
| `script-path` | Asıl çalıştırılacak betik yolu |
| `profiles` | Virgülle ayrılmış yapılandırma dosyası listesi (minimal, standard, strict) |

### Nasıl Çalışır
1. `ECC_HOOK_PROFILE` ortam değişkenini okur (varsayılan: standard)
2. Hook ID'nin `ECC_DISABLED_HOOKS`'ta olup olmadığını kontrol eder
3. İzin veriliyorsa betiği çalıştırır

---

## Çapraz Platform Yol İşleme

ECC hook komutları Node.js betikleriyle çapraz platform davranışı uygular:

```javascript
const path = require('path');
const homedir = require('os').homedir();

// Çapraz platform yol çözümleme
const configDir = path.join(homedir, '.claude');
const hookScript = path.join(configDir, 'scripts', 'hooks', 'my-hook.js');
```

---

## Yapılandırma Doğrulama

Yapılandırmayı doğrulamak için JSON Schema kullanılması önerilir:
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json"
}
```
