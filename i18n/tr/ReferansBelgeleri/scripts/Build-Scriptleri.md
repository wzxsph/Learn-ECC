# Derleme Betikleri

Bu belge, ECC projesinin derleme ve yayınlama betiklerini açıklar.

---

## release.sh

**Yol**: `scripts/release.sh`

Eklenti sürüm yükseltme ve yayınlama betiği.

### İşlevler

1. **Sürüm Doğrulama**
   - Semver formatında sürüm numarası sağlandığını onayla (X.Y.Z veya X.Y.Z-prerelease)
   - Mevcut dalın main olduğunu onayla
   - Çalışma alanının temiz olduğunu onayla

2. **Dosya Güncelleme**
   - Tüm paket/eklenti manifestolarındaki sürüm numaralarını güncelle
   - Dokümantasyondaki sürüm referanslarını güncelle
   - Aracı yapılandırmasını güncelle

3. **Doğrulama Derleme**
   - OpenCode derlemesini çalıştır
   - Yapılandırma testlerini çalıştır
   - Eklenti manifest testlerini çalıştır

4. **Git İşlemleri**
   - Tüm değişiklikleri aşamala
   - Commit oluştur
   - Etiket oluştur ve it

### Kullanım Yöntemi

```bash
# Yeni sürüm yayınla
./scripts/release.sh 1.5.0

# Ön sürüm
./scripts/release.sh 2.0.0-rc.1
```

### Güncellenen Dosyalar

| Dosya | Güncellenen İçerik |
|------|----------|
| `package.json` | version alanı |
| `package-lock.json` | version ve packages[""].version |
| `AGENTS.md` | Sürüm satırı |
| `docs/tr/AGENTS.md` | Sürüm satırı |
| `docs/zh-CN/AGENTS.md` | Sürüm satırı |
| `agent.yaml` | version alanı |
| `VERSION` | Sürüm dosyası |
| `.claude-plugin/plugin.json` | version alanı |
| `.claude-plugin/marketplace.json` | version alanı |
| `.agents/plugins/marketplace.json` | ecc eklenti sürümü |
| `.codex-plugin/plugin.json` | version alanı |
| `.opencode/package.json` | version alanı |
| `.opencode/package-lock.json` | version ve packages[""].version |
| `.opencode/plugins/ecc-hooks.ts` | Başlık sürümü |
| `README.md` | Sürüm tablosu satırı |
| `README.zh-CN.md` | Sürüm tablosu satırı |
| `docs/tr/README.md` | En son yayın başlığı |
| `docs/pt-BR/README.md` | En son yayın başlığı |
| `docs/zh-CN/README.md` | En son yayın başlığı |
| `docs/SELECTIVE-INSTALL-ARCHITECTURE.md` | repoVersion örneği |

### Ön Koşullar

- Git çalışma alanı temiz olmalı
- main dalında olunmalı
- Tüm manifest dosyaları mevcut olmalı

---

## build-opencode.js

**Yol**: `scripts/build-opencode.js`

OpenCode eklentisinin TypeScript kodunu derler.

### Derleme Akışı

1. `dist` dizinini temizle
2. TypeScript derleyicisini bul (`typescript/bin/tsc`)
3. `.opencode` dizinindeki kodu proje kökündeki tsconfig.json ile derle

### Kullanım Yöntemi

```bash
# OpenCode derle
node scripts/build-opencode.js

# release'in parçası olarak otomatik çalışır
./scripts/release.sh 1.5.0
```

### Ön Koşullar

Geliştirme bağımlılıklarının kök dizinde kurulu olduğundan ve TypeScript derleyicisinin kullanılabilir olduğundan emin olun.

---

## Diğer Betikler

### gan-harness.sh

**Yol**: `scripts/gan-harness.sh`

GAN (General Agent Network) test aracı ile ilgili.

### orchestrate-codex-worker.sh

**Yol**: `scripts/orchestrate-codex-worker.sh`

Codex worker süreçlerini orkestre eder.

### sync-ecc-to-codex.sh

**Yol**: `scripts/sync-ecc-to-codex.sh`

ECC'yi Codex'e senkronize eder.

---

## Yayın İş Akışı

```
1. main dalında olduğundan ve çalışma alanının temiz olduğundan emin ol
2. Sürüm numarasını sağlayarak yayın betiğini çalıştır
3. Betik otomatik olarak tamamlar:
   ├── Ön koşulları doğrula
   ├── Tüm sürüm referanslarını güncelle
   ├── OpenCode'u derle
   ├── Test doğrulamalarını çalıştır
   ├── Git commit oluştur
   └── Etiketi it
4. GitHub Actions etiketi algılar ve otomatik olarak yayınlar
```
