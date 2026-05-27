# Araç Fonksiyon Kütüphanesi

Bu belge, `scripts/lib/` dizini altındaki core araç fonksiyon kütüphanesini açıklar.

---

## utils.js

**Yol**: `scripts/lib/utils.js`

Windows, macOS ve Linux için uygun çapraz platform araç fonksiyon kütüphanesi.

### Platform Algılama

```javascript
const { isWindows, isMacOS, isLinux } = require('./utils');

// isWindows: Windows'ta true
// isMacOS: macOS'ta true
// isLinux: Linux'ta true
```

### Dizin İşlemleri

```javascript
const {
  getHomeDir,      // Kullanıcı ana dizinini al
  getClaudeDir,    // ~/.claude dizinini al
  getSessionsDir,  // ~/.claude/session-data dizinini al
  getTempDir,      // Geçici dizini al
  ensureDir,       // Dizin mevcut değilse oluştur
  findFiles        // Kalıpla eşleşen dosyaları bul
} = require('./utils');

// Dizin oluştur (mevcut değilse)
ensureDir('/yol/ile/dizin');

// Dosya bul
const files = findFiles('/yol', '*.js', { recursive: true });
```

### Tarih Saat

```javascript
const {
  getDateString,      // YYYY-AA-GG formatında döndür
  getTimeString,      // SS:DD formatında döndür
  getDateTimeString   // YYYY-AA-GG SS:DD:SS formatında döndür
} = require('./utils');
```

### Oturum ve Proje

```javascript
const {
  sanitizeSessionId,    // Oturum ID'sini temizle, geçersiz karakterleri kaldır
  getSessionIdShort,    // Kısa oturum ID'sini al
  getGitRepoName,       // Git deposu adını al
  getProjectName        // Proje adını al
} = require('./utils');

// Oturum ID'sini temizle
const cleanId = sanitizeSessionId('my-session@123'); // 'my-session-123'

// Proje adını al
const project = getProjectName();
```

### Dosya İşlemleri

```javascript
const {
  readFile,        // Güvenli dosya oku, başarısızlıkta null döndür
  writeFile,       // Dosyaya yaz
  appendFile,      // Dosyaya ekle
  replaceInFile,   // Dosya içeriğini değiştir
  countInFile,     // Kalıp eşleşme sayısını say
  grepFile         // Dosyada kalıp ara
} = require('./utils');

// Dosya oku
const content = readFile('/dosya/yolu.txt');

// Dosya içeriğini değiştir (ilk eşleşme)
replaceInFile('/yol', 'eski', 'yeni');

// Tüm eşleşmeleri değiştir
replaceInFile('/yol', 'eski', 'yeni', { all: true });

// Eşleşme sayısını say
const count = countInFile('/yol', /kalıp/g);

// Eşleşen satırları döndür
const matches = grepFile('/yol', /regex/);
// Döndürür: [{ lineNumber: 1, content: '...' }]
```

### Dize İşleme

```javascript
const { stripAnsi } = require('./utils');

// ANSI kaçış dizilerini kaldır (renk vb.)
const clean = stripAnsi('\x1b[32mMerhaba\x1b[0m'); // 'Merhaba'
```

### Hook I/O

```javascript
const { readStdinJson, log, output } = require('./utils');

// stdin'den JSON oku (hook'larda kullanılır)
const input = await readStdinJson({ timeoutMs: 5000 });

// stderr'ye çıktı ver (kullanıcıya görünür)
log('Hata ayıklama mesajı');

// stdout'a çıktı ver (Claude'a döndür)
output({ result: 'veri' });
```

### Sistem Komutları

```javascript
const { commandExists, runCommand, isGitRepo, getGitModifiedFiles } = require('./utils');

// Komutun mevcut olup olmadığını kontrol et
if (commandExists('git')) { /* ... */ }

// Komut çalıştır (yalnızca beyaz liste komutlar)
const result = runCommand('git rev-parse --show-toplevel');
if (result.success) {
  console.log(result.output);
}

// Git deposu olup olmadığını kontrol et
if (isGitRepo()) { /* ... */ }

// Değiştirilen dosyaları al
const files = getGitModifiedFiles(['\\.js$', '\\.md$']);
```

---

## package-manager.js

**Yol**: `scripts/lib/package-manager.js`

Paket yöneticisi algılama ve seçim aracı.

### Desteklenen Paket Yöneticileri

- npm
- pnpm
- yarn
- bun

### Algılama önceliği

1. Ortam değişkeni `CLAUDE_PACKAGE_MANAGER`
2. Proje yapılandırması `.claude/package-manager.json`
3. `package.json`'daki `packageManager` alanı
4. lock dosyası algılama
5. Global kullanıcı tercihi `~/.claude/package-manager.json`
6. Varsayılan: npm

### Temel API

```javascript
const {
  getPackageManager,           // Mevcut projenin paket yöneticisini al
  setPreferredPackageManager,  // Global tercih belirle
  setProjectPackageManager,    // Proje tercihi belirle
  getAvailablePackageManagers, // Sistemdeki paket yöneticilerini al
  detectFromLockFile,          // Lock dosyasından algıla
  detectFromPackageJson,       // package.json'dan algıla
  getRunCommand,               // Betik çalıştırma komutunu al
  getExecCommand,              // Binary çalıştırma komutunu al
  getSelectionPrompt,          // Etkileşimli seçim ipucu al
  getCommandPattern            // Komut eşleştirme regex oluştur
} = require('./package-manager');

// Mevcut paket yöneticisini al
const { name, config, source } = getPackageManager();
// name: 'pnpm', 'npm', 'yarn', veya 'bun'
// config: { installCmd, runCmd, execCmd, testCmd, ... }
// source: 'environment', 'project-config', 'lock-file', ...

// Global tercih belirle
setPreferredPackageManager('pnpm');

// Proje tercihi belirle
setProjectPackageManager('yarn', '/proje/yolu');

// Çalıştırma komutu al
const testCmd = getRunCommand('test'); // 'pnpm test' veya 'npm test' vb.
const buildCmd = getRunCommand('build');

// Çalıştırma komutu al
const eslintCmd = getExecCommand('eslint', '--fix .');
```

### Güvenlik Kontrolü

`getRunCommand` ve `getExecCommand` girdiyi doğrular:
- Betik adı yalnızca harf, rakam, tire, alt çizgi, nokta, ters slash ve @ içerebilir
- Parametreler yalnızca harf, rakam, boşluk, tire, nokta, eğik çizgi, eşittir, iki nokta, virgül, tırnak ve @ içerebilir

---

## session-manager.js

**Yol**: `scripts/lib/session-manager.js`

Oturum yönetimi kütüphanesi, oturum CRUD işlemlerini sağlar.

### Oturum Deposu

- Yeni format: `~/.claude/session-data/YYYY-AA-GG-[session-id]-session.tmp`
- Eski format uyumluluğu: `~/.claude/sessions/YYYY-AA-GG-session.tmp`

### Temel API

```javascript
const {
  parseSessionFilename,      // Oturum dosya adını ayrıştır
  getSessionPath,            // Oturum dosyası tam yolunu al
  getSessionContent,         // Oturum içeriğini oku
  parseSessionMetadata,      // Oturum meta verilerini ayrıştır
  getSessionStats,           // Oturum istatistiklerini al
  getSessionTitle,           // Oturum başlığını al
  getSessionSize,            // Oturum boyutunu al (insan okunabilir format)
  getAllSessions,            // Tüm oturumları al (sayfalama ve filtreleme destekler)
  getSessionById,            // ID'ye göre tek oturum al
  writeSessionContent,       // Oturum içeriği yaz
  appendSessionContent,      // Oturum içeriğine ekle
  deleteSession,             // Oturumu sil
  sessionExists              // Oturumun mevcut olup olmadığını kontrol et
} = require('./session-manager');

// Tüm oturumları al
const { sessions, total, offset, limit, hasMore } = getAllSessions({
  limit: 50,
  offset: 0,
  date: '2026-05-26',
  search: 'abc123'
});

// Tek oturum al
const session = getSessionById('abc123', true);
// session: { filename, shortId, date, sessionPath, content, metadata, stats, ... }

// Oturum meta verilerini ayrıştır
const metadata = parseSessionMetadata(content);
// metadata: { title, date, started, lastUpdated, project, branch, worktree, completed, inProgress, notes, context }

// Oturum istatistiklerini al
const stats = getSessionStats(sessionPathOrContent);
// stats: { totalItems, completedItems, inProgressItems, lineCount, hasNotes, hasContext }

// Oturum yaz
writeSessionContent(sessionPath, '# Yeni Oturum\n\nİçerik...');

// İçerik ekle
appendSessionContent(sessionPath, '\n\nDaha fazla içerik...');

// Oturumu sil
deleteSession(sessionPath);
```

### Oturum Meta Veri Formatı

Oturum içeriğindeki meta veriler:

```markdown
# Oturum Başlığı

**Date:** 2026-05-26
**Started:** 14:30
**Last Updated:** 15:45
**Project:** my-project
**Branch:** main
**Worktree:** frontend

### Completed
- [x] Görev 1
- [x] Görev 2

### In Progress
- [ ] Görev 3

### Notes for Next Session
Config dosyasını kontrol et.

### Context to Load
```
onceki-baglam-icerigi
```
```

---

## install-executor.js

**Yol**: `scripts/lib/install-executor.js`

ECC kurulum yürütücüsü, kurulum işlemlerinin gerçek yürütülmesinden sorumludur.

### Temel İşlevler

- Kurulum planı oluştur
- Dosya kopyalama işlemlerini yürüt
- Kurulum durumu kaydını işle

### Temel API

```javascript
const {
  applyInstallPlan,       // Kurulum planını uygula
  listAvailableLanguages  // Kullanılabilir dilleri listele
} = require('./install-executor');

// Kullanılabilir dilleri listele
const languages = listAvailableLanguages();
// ['typescript', 'javascript', 'python', ...]
```

---

## install-lifecycle.js

**Yol**: `scripts/lib/install-lifecycle.js`

Kurulum yaşam döngüsü yönetimi, kurulumun çeşitli aşamalarını işler.

---

## install-manifests.js

**Yol**: `scripts/lib/install-manifests.js`

Kurulum manifestoları tanımı, şunları içerir:
- Desteklenen kurulum hedefleri
- Kullanılabilir dil listesi
- Desteklenen yerelleştirme listesi

---

## install-state.js

**Yol**: `scripts/lib/install-state.js`

Kurulum durumu yönetimi, hangi dosyaların nereye kurulduğunu kaydeder.

---

## project-detect.js

**Yol**: `scripts/lib/project-detect.js`

Proje türü algılama, mevcut dizindeki proje türünü algılar.

---

## observer-sessions.js

**Yol**: `scripts/lib/observer-sessions.js`

Oturum gözlemleyicisi, oturum aktivitelerini izlemek için kullanılır.

---

## shell-substitution.js

**Yol**: `scripts/lib/shell-substitution.js`

Shell değişken değiştirme aracı, ortam değişkenleri ve shell ifadelerini işler.

---

## tmux-worktree-orchestrator.js

**Yol**: `scripts/lib/tmux-worktree-orchestrator.js`

Tmux + Git Worktree orkestratörü, karmaşık geliştirme ortamlarını yönetir.

---

## mcp-config.js

**Yol**: `scripts/lib/mcp-config.js`

MCP (Model Context Protocol) yapılandırma yönetimi.

---

## Diğer Araçlar

| Dosya | Açıklama |
|------|------|
| `agent-compress.js` | Aracı bağlam sıkıştırma |
| `cost-estimate.js` | Maliyet tahmini |
| `github-discussions.js` | GitHub tartışma entegrasyonu |
| `hook-flags.js` | Hook bayrak yönetimi |
| `inspection.js` | İnceleme araçları |
| `orchestration-session.js` | Orkestrasyon oturum yönetimi |
| `resolve-ecc-root.js` | ECC kök dizin çözümleme |
| `resolve-formatter.js` | Formatlayıcı çözümleme |
| `session-aliases.js` | Oturum takma ad yönetimi |
| `session-bridge.js` | Oturum köprüleme |
| `shell-split.js` | Shell komut bölme |
| `state-store/` | Durum deposu alt modülü |
| `install/` | Kurulum ile ilgili alt modüller |
| `skill-evolution/` | Beceri evrim alt modülü |
| `skill-improvement/` | Beceri iyileştirme alt modülü |
