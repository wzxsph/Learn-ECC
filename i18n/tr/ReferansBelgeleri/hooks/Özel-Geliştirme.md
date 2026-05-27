# Özel Geliştirme

## Genel Bakış

Bu kılavuz, ECC Hooks Sistemi için özel hook geliştirmeyi, temel yapıyı, en iyi uygulamaları ve exit 0 gereksinimlerini açıklar.

## Temel Yapı

### Minimum Hook Şablonu

```javascript
// my-custom-hook.js
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Araç bilgilerine eriş
  const toolName = input.tool_name;        // "Edit", "Bash", "Write" vb.
  const toolInput = input.tool_input;      // Araça özel parametreler
  const toolOutput = input.tool_output;    // Yalnızca PostToolUse'ta mevcut

  // Uyarı (engellemez): stderr'ye yaz
  console.error('[Hook] Uyarı bilgisi');

  // Engelle (yalnızca PreToolUse): exit code 2
  // process.exit(2);

  // Her zaman orijinal veriyi stdout'a çıktıla
  console.log(data);
});
```

---

## Çıkış Kodu Gereksinimleri

### Kritik Kural: Kritik Olmayan Hatalarda exit 0

**Önemli**: Tüm hook'lar kritik olmayan hatalarda exit 0 ile çıkmalıdır, araç yürütmeyi beklenmedik şekilde engellememek için.

| Çıkış kodu | Anlamı | Kullanım Senaryosu |
|--------|------|----------|
| 0 | Başarılı | Devam et veya kritik olmayan uyarı |
| 2 | Engelle | Yalnızca PreToolUse'ta kritik engelleme için |
| Diğer sıfır olmayan | Hata | Yalnızca günlüğe kaydet, **kullanmayın** |

### Neden Sıfır Olmayan Çıkış Kodları Kötüdür?

Hook sıfır olmayan bir çıkış koduyla çıkarsa (2 hariç), Claude Code tüm araç çağrısını başarısız olarak işaretler, bu şunlara yol açabilir:
- Araç yürütmesi beklenmedik şekilde kesintiye uğrar
- Kullanıcı deneyimi bozulur
- Hata ayıklaması zor sorunlar ortaya çıkar

### Hataları Doğru Şekilde Yönetme

```javascript
// Hatalı Örnek - Bunu yapmayın
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // İşleme mantığı
  } catch (e) {
    console.error('[Hook] Hata:', e.message);
    process.exit(1);  // Hatalı: aracı engeller
  }
  console.log(data);
});

// Doğru Örnek - Bunu yapın
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // İşleme mantığı
  } catch (e) {
    console.error('[Hook] Hata:', e.message);
    // Kritik olmayan hata, devam etmek için exit 0
    console.log(data);
    return;
  }
  console.log(data);
});
```

---

## Hook Girdi Şeması

### HookInput Arayüzü

```typescript
interface HookInput {
  tool_name: string;          // Araç adı
  tool_input: {               // Araç girdi parametreleri
    command?: string;         // Bash: Komut
    file_path?: string;        // Edit/Write/Read: Dosya yolu
    old_string?: string;       // Edit: Değiştirilen metin
    new_string?: string;       // Edit: Değiştirme metni
    content?: string;          // Write: Dosya içeriği
  };
  tool_output?: {             // Yalnızca PostToolUse
    output?: string;          // Komut/aracın çıktısı
  };
}
```

### Araç Bilgilerine Erişme

```javascript
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Araç adını al
  console.log('Araç:', input.tool_name);

  // Araç türüne göre işle
  if (input.tool_name === 'Bash') {
    console.log('Komut:', input.tool_input.command);
  }

  if (input.tool_name === 'Edit' || input.tool_name === 'Write') {
    console.log('Dosya:', input.tool_input.file_path);
  }

  // PostToolUse çıktıya erişebilir
  if (input.tool_output) {
    console.log('Çıktı:', input.tool_output.output);
  }
});
```

---

## Yaygın Hook Tarifleri

### TODO/FIXME Yorumlarını Uyar

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "TODO/FIXME yorumu ekleme konusunda uyar"
}
```

### Çok Büyük Dosya Oluşturmayı Engelle

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');console.error('[Hook] Split into smaller, focused modules');process.exit(2)}console.log(d)})\""
  }],
  "description": "800 satırdan fazla dosya oluşturmayı engelle"
}
```

### Python Dosyalarını ruff ile Otomatik Biçimlendir

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/\\.py$/.test(p)){const{execFileSync}=require('child_process');try{execFileSync('ruff',['format',p],{stdio:'pipe'})}catch(e){}}console.log(d)})\""
  }],
  "description": "Düzenlemeden sonra Python dosyalarını ruff ile otomatik biçimlendir"
}
```

### Yeni Kaynak Dosyalarına Test Eşlik Etmeyi Zorunlu Kıl

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p);console.error('[Hook] Expected: '+testPath);console.error('[Hook] Consider writing tests first (/tdd)')}}console.log(d)})\""
  }],
  "description": "Yeni kaynak dosyası eklerken test oluşturmayı hatırlat"
}
```

---

## Asenkron Hooklar

Ana akışı engellememesi gereken hook'lar için (arka plan analizi gibi):

```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node my-slow-hook.js",
    "async": true,
    "timeout": 30
  }]
}
```

### Asenkron Hook Notları

- Asenkron hook'lar arka planda çalışır
- Araç yürütmeyi engelleyemez
- 30 saniye içinde tamamlanmalıdır
- Uygundur: günlükleme, analiz, telemetri

---

## Engelleme Hookları

Araç yürütmeyi engellemek zorunlu olduğunda (yalnızca PreToolUse):

```javascript
// PreToolUse'ta engelle
process.exit(2);  // Çıkış kodu 2 engelleme anlamına gelir
```

### Ne Zaman Engelleme Kullanılır

- Güvenlik kontrolü başarısız olduğunda (anahtar tespit edildi gibi)
- Katı politika ihlali olduğunda
- Veri kaybına yol açabilecek işlemlerde

### Ne Zaman Engelleme Kullanılmaz

- Kod stili sorunları (uyarı kullanın)
- Kritik olmayan kontroller
- Öneri türündeki kontroller

---

## Çalışma Zamanı Yapılandırması

### run-with-flags.js Kullanma

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const p=require('path');const r=(()=>{var e=process.env.CLAUDE_PLUGIN_ROOT;if(e&&e.trim())return e.trim();var p=require('path'),f=require('fs'),h=require('os').homedir(),d=p.join(h,'.claude'),q=p.join('scripts','lib','utils.js');if(f.existsSync(p.join(d,q)))return d;for(var s of [['ecc'],['ecc@ecc'],['marketplaces','ecc'],['everything-claude-code'],['everything-claude-code@everything-claude-code'],['marketplaces','everything-claude-code']]){var l=p.join(d,'plugins',...s);if(f.existsSync(p.join(l,q)))return l}try{for(var g of ['ecc','everything-claude-code']){var b=p.join(d,'plugins','cache',g);for(var o of f.readdirSync(b,{withFileTypes:true})){if(!o.isDirectory())continue;for(var v of f.readdirSync(p.join(b,o.name),{withFileTypes:true})){if(!v.isDirectory())continue;var c=p.join(b,o.name,v.name);if(f.existsSync(p.join(c,q)))return c}}}}catch(x){}return d})();const s=p.join(r,'scripts/hooks/plugin-hook-bootstrap.js');process.env.CLAUDE_PLUGIN_ROOT=r;process.argv.splice(1,0,s);require(s)\" node scripts/hooks/run-with-flags.js my-hook scripts/hooks/my-hook.js standard,strict"
  }]
}
```

### Basitleştirilmiş Versiyon

Eklenti kök dizini biliniyorsa doğrudan betik yolu kullanın:

```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/my-hook.js"
  }]
}
```

---

## Çapraz Platform Notları

### Yol İşleme

```javascript
const path = require('path');
const os = require('os');

// Sabit kodlanmış yol ayırıcıları yerine path.join kullan
const configPath = path.join(os.homedir(), '.claude', 'config.json');

// Platform tespiti
if (process.platform === 'win32') {
  // Windows'a özel işleme
}
```

### Süreç Çıktısı

```javascript
// Uyarıları stdout yerine stderr'ye yaz (kullanıcıya gösterilir)
console.error('[Hook] Warning: some issue detected');

// Orijinal verileri stdout'a yaz (zorunlu)
console.log(data);

// stdout'a başka mesaj metinleri yazmayın (veri akışını bozar)
console.log('Some message');  // Bu veri akışını bozar
```

---

## Performans En İyi Uygulamaları

### Hızlı Tutma

- PreToolUse hook'ları: <200ms
- PostToolUse senkron hook'ları: <1 saniye
- Asenkron hook'lar: <30 saniye

### Engelleyen İşlemlerden Kaçınma

```javascript
// Kötü: Senkron dosya okuma
const content = fs.readFileSync('large-file.txt', 'utf8');

// İyi: Asenkron veya gecikmeli yükleme
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // işle
});
```

### Önbellekleme Sonuçları

```javascript
// Pahalı işlemleri önbellekle
let cachedResult = null;
let cacheTime = 0;
const CACHE_TTL = 60000; // 1 dakika

function getCachedResult() {
  const now = Date.now();
  if (!cachedResult || now - cacheTime > CACHE_TTL) {
    cachedResult = expensiveOperation();
    cacheTime = now;
  }
  return cachedResult;
}
```

---

## Hook Test Etme

### Manuel Test

```bash
# PreToolUse hook'u test et
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"}}' | node scripts/hooks/my-hook.js

# PostToolUse hook'u test et
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"},"tool_output":{"output":"test\n"}}' | node scripts/hooks/my-hook.js
```

### Test Çıktı Formatı

```javascript
// Doğru stdout çıktısı (JSON)
console.log(data);  // Orijinal girdi verileri

// Doğru stderr çıktısı (Uyarı)
console.error('[Hook] Warning message');

// Yanlış yaklaşım - stdout'a başka içerik yazmayın
console.log('Some message');  // Bu veri akışını bozar
```

---

## Hook Hata Ayıklama

### Hata Ayıklama Çıktısı Ekleme

```javascript
const DEBUG = process.env.DEBUG_HOOKS === '1';

process.stdin.on('end', () => {
  if (DEBUG) {
    console.error('[DEBUG] Received input:', data);
  }
  // İşleme mantığı
  if (DEBUG) {
    console.error('[DEBUG] Sending output');
  }
  console.log(data);
});
```

### Yaygın Sorunlar

| Sorun | Neden | Çözüm |
|------|------|------|
| Araç beklenmedik şekilde engellendi | Hook sıfır olmayan çıkış kodu verdi (2 hariç) | Bunun yerine exit 0 ve stderr uyarısı kullan |
| Askıda kalma | Hook sonlanmadı | Her zaman stdout'a çıktı verdiğinden emin ol |
| Veri bozulması | stdout'a JSON olmayan çıktı | Yalnızca orijinal data'yı çıktıla |
| Performans sorunu | Hook çok yavaş | Optimize et veya async yap |

---

## hooks.json'a Entegrasyon

### Yeni Hook Ekleme

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "node scripts/hooks/my-new-hook.js"
          }
        ],
        "description": "Yeni hook tanımım",
        "id": "pre:custom:my-new-hook"
      }
    ]
  }
}
```

### Yerleşik Hook'u Devre Dışı Bırakma

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Dokümantasyon dosyası uyarısını devre dışı bırak"
      }
    ]
  }
}
```

---

## En İyi Uygulamalar Özeti

1. **Kritik olmayan hatalarda her zaman exit 0** - Sıfır olmayan çıkış kodları kullanmayın (2 hariç)
2. **Uyarıları stderr'ye yaz** - console.log kullanmayın
3. **Hızlı tut** - PreToolUse <200ms
4. **Her zaman orijinal data'yı stdout'a çıktıla** - Veri akışını değiştirmeyin
5. **Uzun süreli işlemler için async kullan** - async: true ayarlayın
6. **Çapraz platform yol işleme** - path.join kullan
7. **Çıktı formatını test et** - stdout'un orijinal JSON olduğundan emin ol
8. **Hook'larınızı belgelendirin** - Açık description ekleyin
