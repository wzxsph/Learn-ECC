# Araç Betikleri

Bu belge, ECC (Everything Claude Code) projesindeki temel araç betiklerini açıklar.

## Dizin

- [ecc.js](#eccjs) - Ana komut satırı giriş noktası
- [install-apply.js](#install-applyjs) - Kurulum yürütücüsü
- [claw.js](#clawjs) - NanoClaw REPL
- [harness-audit.js](#harness-auditjs) - Araç zinciri denetimi
- [catalog.js](#catalogjs) - Bileşen kataloğu görüntüleyici
- [build-opencode.js](#build-opencodejs) - OpenCode derlemesi

---

## ecc.js

**Yol**: `scripts/ecc.js`

ECC'nin isteğe bağlı kurulumu için CLI ana giriş noktası, tüm alt komutları merkezi olarak dağıtır.

### Komut Listesi

| Komut | Betik | Açıklama |
|------|------|------|
| `install` | install-apply.js | ECC içeriğini hedefe kur |
| `plan` | install-plan.js | Kurulum listesi ve planı kontrol et |
| `catalog` | catalog.js | Yükleme yapılandırma dosyalarını ve bileşen ID'lerini keşfet |
| `consult` | consult.js | Doğal dil sorgusuna göre bileşen öner |
| `list-installed` | list-installed.js | Mevcut bağlamın kurulum durumunu kontrol et |
| `doctor` | doctor.js | Eksik veya kaymış ECC yönetim dosyalarını tanı |
| `repair` | repair.js | Kaymış veya eksik ECC yönetim dosyalarını onar |
| `auto-update` | auto-update.js | En son ECC değişikliklerini çek ve yeniden kur |
| `status` | status.js | SQLite durum deposu özetini sorgula |
| `platform-audit` | platform-audit.js | GitHub kuyruğu, tartışmalar, yol haritası vb. denetle |
| `security-ioc-scan` | ci/scan-supply-chain-iocs.js | Bağımlılıklar ve AI araçları yüzeyindeki tedarik zinciri IOC'lerini tara |
| `sessions` | sessions-cli.js | SQLite durum deposundaki oturumları listele veya kontrol et |
| `work-items` | work-items.js | Bağlı Linear, GitHub iş kalemlerini takip et |
| `session-inspect` | session-inspect.js | dmux veya Claude geçmişinden standart oturum anlık görüntüsü oluştur |
| `loop-status` | loop-status.js | Claude betiklerindeki eskimiş döngü uyandırmalarını ve bekleyen araç sonuçlarını kontrol et |
| `uninstall` | uninstall.js | install-state'da kayıtlı ECC yönetim dosyalarını sil |

### Kullanım Örneği

```bash
# Doğrudan dil kur
ecc typescript

# Yapılandırma dosyasıyla kur
ecc install --profile developer --target claude

# Kurulum planını görüntüle
ecc plan --profile core --target cursor

# Kullanılabilir bileşenleri listele
ecc catalog profiles
ecc catalog components --family language

# Bileşen ayrıntılarını göster
ecc catalog show framework:nextjs

# Öneri danış
ecc consult "security reviews"

# Kuruluları listele
ecc list-installed --json

# Sorunları tanı
ecc doctor --target cursor
ecc repair --dry-run

# Otomatik güncelle
ecc auto-update --dry-run

# Durum sorgula
ecc status --json
ecc status --exit-code
ecc status --markdown --write status.md

# Platform denetimi
ecc platform-audit --json --allow-untracked docs/drafts/

# Güvenlik taraması
ecc security-ioc-scan --home

# Oturum yönetimi
ecc sessions
ecc sessions session-active --json

# İş kalemlerini takip et
ecc work-items upsert linear-ecc-20 --source linear --source-id ECC-20 --title "Review control-plane contract" --status blocked
ecc work-items sync-github --repo affaan-m/ECC

# Döngü durumunu kontrol et
ecc loop-status --json

# Kaldır
ecc uninstall --target antigravity --dry-run
```

---

## install-apply.js

**Yol**: `scripts/install-apply.js`

ECC kurulum çalışma zamanı, geleneksel dil kurulum giriş noktalarını korurken hedefe özel değişiklik mantığını test edilebilir Node koduna taşır.

### Desteklenen Hedefler

| Hedef | Açıklama |
|------|------|
| `claude` | ~/.claude/'a kur, kurallar/beceriler rules/ecc ve skills/ecc altına |
| `claude-project` | ./.claude/'a kur (proje düzeyi) |
| `cursor` | Kuralları, hook'ları, cursor yapılandırmasını ./.cursor/'a kur |
| `antigravity` | Kuralları, iş akışlarını, becerileri, aracıları ./.agent/'a kur |
| `codex` | Paylaşılan aracı/yapılandırmayı ~/.codex/'a kur |
| `gemini` | Proje düzeyi Gemini yapılandırmasını ./.gemini/'a kur |
| `opencode` | Paylaşılan komutları/hook'ları/yapılandırmayı ~/.opencode/'a kur |
| `codebuddy` | Komutları, aracıları, becerileri ./.codebuddy/'a kur |
| `joycode` | Komutları, aracıları, becerileri ./.joycode/'a kur |
| `qwen` | ~/.qwen/'a kur |
| `zed` | ./.zed/'a kur |

### Kurulum Modları

1. **Geleneksel dil modu**: `install.sh <language>` (örn. `ecc-install typescript`)
2. **Profil modu**: `install.sh --profile <name> [--with <component>]... [--without <component>]...`
3. **Modül modu**: `install.sh --modules <id,id,...>`
4. **Beceri modu**: `install.sh --skills <skill-id[,skill-id...]>`
5. **Yerelleştirme modu**: `install.sh --target claude|claude-project --locale <locale-code>`

### Seçenekler

- `--dry-run` - Kurulum planını göster ama dosyaları kopyalama
- `--json` - Makine tarafından okunabilir JSON formatı çıktıla
- `--config <path>` - Kurulum niyetini dosyadan yükle

---

## claw.js

**Yol**: `scripts/claw.js`

NanoClaw v2 - Sıfır dış bağımlılık, oturum bilincli, `claude -p` üzerine inşa edilmiş Everything Claude Code REPL.

### İşlevler

- Oturum geçmişini `~/.claude/claw/` altında kalıcı depolar
- Dinamik beceri yükleme desteği bağlam olarak
- Bağlam uzunluğunu yönetmek için oturum sıkıştırma
- Oturum dallandırma ve dışa aktarma
- Oturumlar arası arama

### REPL Komutları

| Komut | Açıklama |
|------|------|
| `/help` | Yardımı göster |
| `/clear` | Mevcut oturum geçmişini temizle |
| `/history` | Tüm konuşma geçmişini yazdır |
| `/sessions` | Kayıtlı oturumları listele |
| `/model [name]` | Modeli göster/ayarla |
| `/load <skill-name>` | Beceriyi aktif bağlama yükle |
| `/branch <session-name>` | Mevcut oturumu yeni oturuma dallandır |
| `/search <query>` | Oturumlar arasında ara |
| `/compact` | Son turları koru, eski bağlamı sıkıştır |
| `/export <md\|json\|txt> [path]` | Oturumu dışa aktar |
| `/metrics` | Oturum metriklerini göster |
| `exit` | REPL'den çık |

### Ortam Değişkenleri

| Değişken | Varsayılan | Açıklama |
|------|--------|------|
| `CLAW_SESSION` | `default` | Başlangıç oturum adı |
| `CLAW_MODEL` | `sonnet` | Varsayılan model |
| `CLAW_SKILLS` | boş | Yüklü beceri listesi (virgülle ayrılmış) |

### Kullanım Örneği

```bash
# Varsayılan oturumla başlat
node scripts/claw.js

# Belirtilen oturumla başlat
CLAW_SESSION=my-session node scripts/claw.js

# Beceri yükleyerek başlat
CLAW_SKILLS="javascript-patterns,python-testing" node scripts/claw.js
```

---

## harness-audit.js

**Yol**: `scripts/harness-audit.js`

Açık dosya/kural kontrollerine dayanan deterministik araç zinciri denetim aracı. ECC depo kalıbı vs tüketici proje kalıbı otomatik algılama.

### Denetim Kategorileri

| Kategori | Açıklama |
|------|------|
| Tool Coverage | Araç kapsam bütünlüğü |
| Context Efficiency | Bağlam verimliliği |
| Quality Gates | Kalite kapıları |
| Memory Persistence | Bellek kalıcılığı |
| Eval Coverage | Değerlendirme kapsamı |
| Security Guardrails | Güvenlik bariyerleri |
| Cost Efficiency | Maliyet verimliliği |
| GitHub Integration | GitHub entegrasyonu |
| Vercel/Netlify/Cloudflare/Fly Integration | Platform entegrasyonları |

### Kullanım Örneği

```bash
# Mevcut dizini denetle
node scripts/harness-audit.js

# Belirtilen kapsam
node scripts/harness-audit.js --scope repo
node scripts/harness-audit.js --scope hooks
node scripts/harness-audit.js --scope skills

# Belirtilen kök dizin
node scripts/harness-audit.js --root /proje/yolu

# JSON formatında çıktı
node scripts/harness-audit.js --format json

# Kombinasyonlu kullanım
node scripts/harness-audit.js repo --format json --root .
```

---

## catalog.js

**Yol**: `scripts/catalog.js`

ECC kurulum bileşenlerini ve yapılandırma dosyalarını keşfetme aracı.

### Komutlar

#### profiles

Tüm kurulum profillerini listeler.

```bash
node scripts/catalog.js profiles
node scripts/catalog.js profiles --json
```

#### components

Kurulum bileşenlerini listeler, aile ve hedefe göre filtrelenebilir.

```bash
node scripts/catalog.js components
node scripts/catalog.js components --family language
node scripts/catalog.js components --family framework
node scripts/catalog.js components --target claude
node scripts/catalog.js components --json
```

#### show

Belirli bileşenin ayrıntılarını gösterir.

```bash
node scripts/catalog.js show <component-id>
node scripts/catalog.js show framework:nextjs --json
```

### Bileşen Aileleri

- `baseline` - Temel bileşenler
- `language` - Dil bileşenleri (javascript, typescript, python, vb.)
- `framework` - Çerçeve bileşenleri (nextjs, react, vue, vb.)
- `capability` - Yetenek bileşenleri
- `agent` - Aracı bileşenleri
- `skill` - Beceri bileşenleri

---

## build-opencode.js

**Yol**: `scripts/build-opencode.js`

OpenCode eklentisinin TypeScript kodunu derler.

### İşlevler

1. `dist` dizinini temizle
2. `.opencode` dizini altındaki kodu TypeScript derleyicisiyle derle
3. `.opencode/dist`'e çıktı ver

### Ön Koşullar

Geliştirme bağımlılıklarının kök dizinde kurulu olduğundan ve TypeScript derleyicisinin kullanılabilir olduğundan emin olun.

### Kullanım

```bash
node scripts/build-opencode.js
```
