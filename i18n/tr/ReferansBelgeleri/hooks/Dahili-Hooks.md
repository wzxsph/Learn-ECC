# Yerleşik Hooks

## Genel Bakış

ECC eklentisi, `ECC/scripts/hooks/` dizininde zengin yerleşik Hook sağlar.

## PreToolUse Yerleşik Hookları

### pre:bash:dispatcher

**Tür**: PreToolUse
**matcher**: Bash
**Tanım**: Kalite kontrolleri, tmux hatırlatıcıları, git push hatırlatıcıları ve GateGuard kontrollerini birleştiren Bash ön kontrol dağıtıcısı

**Özellikler**:
- Geliştirme sunucusu engelleyici - tmux dışında `npm run dev` gibi komutları engeller
- Tmux hatırlatıcı - Uzun süreli komutlar (npm test, cargo build, docker) için tmux kullanımını önerir
- Git push hatırlatıcı - `git push` öncesi değişiklikleri incelemeyi hatırlatır
- Pre-commit kalite kontrolleri - Kalite kontrolleri çalıştır: lint aşamalı dosyalar, commit mesajı formatını doğrula, console.log/debugger/anahtar tespiti

**Çıkış kodu**:
- 0: Uyarı ama devam et
- 2: Yürütmeyi engelle

---

### pre:write:doc-file-warning

**Tür**: PreToolUse
**matcher**: Write
**Tanım**: Standart dışı dokümantasyon dosyası oluşturma konusunda uyarı

**Özellikler**:
- İzin verilen standart dosyalar: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- İzin verilen dizinler: docs/, skills/
- Diğer .md/.txt dosyaları için uyarı
- Çapraz platform yol işleme

**Çıkış kodu**: 0 (sadece uyarı)

---

### pre:edit-write:suggest-compact

**Tür**: PreToolUse
**matcher**: Edit|Write
**Tanım**: Mantıksal aralıklarda (yaklaşık her 50 araç çağrısında) manuel `/compact` öner

**Özellikler**:
- Araç çağrı sayısını takip eder
- Uygun aralıklarda bağlam sıkıştırma hatırlatması yapar
- Engellemez, sadece öneri sağlar

**Çıkış kodu**: 0 (öneri)

---

### pre:observe:continuous-learning

**Tür**: PreToolUse
**matcher**: *
**Tanım**: Sürekli öğrenme sinyallerini desteklemek için araç niyetini kaydeder

**Özellikler**:
- Araç kullanım gözlemlerini yakalar
- Desen çıkarımı ve analizi için kullanılır
- Asenkron yürütme, engellemez

**Çıkış kodu**: 0

---

### pre:governance-capture

**Tür**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Tanım**: Yönetişim olaylarını yakalar (anahtarlar, politika ihlalleri, onay talepleri)

**Özellikler**:
- Secret/anahtar sızıntısını tespit eder
- Politika ihlallerini yakalar
- Onay taleplerini kaydeder

**Etkinleştirme koşulu**: `ECC_GOVERNANCE_CAPTURE=1`

**Çıkış kodu**: 0

---

### pre:config-protection

**Tür**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**Tanım**: Linter/formatter yapılandırma dosyalarını değiştirmeyi engeller

**Özellikler**:
- .eslintrc, .prettierrc gibi değişiklikleri engeller
- Yapılandırmayı zayıflatmak yerine kodu düzeltmeye yönlendirir

**Çıkış kodu**: 0

---

### pre:mcp-health-check

**Tür**: PreToolUse
**matcher**: *
**Tanım**: MCP araç yürütmesinden önce MCP sunucu sağlık durumunu kontrol eder

**Özellikler**:
- MCP sunucu durumunu kontrol eder
- Sağlıksız MCP sunucularına yapılan çağrıları engeller

**Çıkış kodu**: 0

---

### pre:edit-write:gateguard-fact-force

**Tür**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**Tanım**: Gerçek zorlama GateGuard: Her dosyadaki ilk düzenleme/yazmayı engeller, izin verilmeden önce araştırma gerektirir

**Özellikler**:
- İlk düzenlemeyi engeller
- Onay gerektirir: içe aktarma, veri şeması, kullanıcı talimatı
- Değişiklik için yeterli gerekçe olduğundan emin olur

**Çıkış kodu**: 0

---

## PostToolUse Yerleşik Hookları

### post:bash:dispatcher

**Tür**: PostToolUse
**matcher**: Bash
**Tanım**: Günlükleirme, PR ve build bildirimleri için Bash son kontrol dağıtıcısı

**Özellikler**:
- PR günlüğü - `gh pr create` sonrası PR URL'sini ve inceleme komutunu kaydeder
- Build analizi - Build komutlarından sonra arka plan analizi (asenkron, engellemez)

**Çıkış kodu**: 0

---

### post:quality-gate

**Tür**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Tanım**: Dosya düzenlemesinden sonra hızlı kalite kontrolü çalıştırır

**Özellikler**:
- Kod kalitesi kontrolleri
- Asenkron yürütme
- Düzenleme işlemini engellemez

**Çıkış kodu**: 0

---

### post:edit:design-quality-check

**Tür**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Tanım**: UI düzenlemelerinin genel şablon görünümünden saptığında uyarı

**Özellikler**:
- UI tasarım kalitesini tespit eder
- Çok genel şablon görünümünü önler

**Çıkış kodu**: 0

---

### post:edit:accumulator

**Tür**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Tanım**: Stop için toplu biçimlendirme ve tür kontrolü amacıyla düzenlenen JS/TS dosya yollarını kaydeder

**Özellikler**:
- Düzenlenen dosya listesini biriktirir
- stop:format-typecheck tarafından kullanılır

**Çıkış kodu**: 0

---

### post:edit:console-warn

**Tür**: PostToolUse
**matcher**: Edit
**Tanım**: Düzenlemeden sonra console.log ifadelerini uyarır

**Özellikler**:
- Kod içindeki console.log'ları tespit eder
- Geliştiricileri hata ayıklama kodunu temizlemeye çağırır

**Çıkış kodu**: 0

---

### post:governance-capture

**Tür**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Tanım**: Araç çıktısından yönetişim olaylarını yakalar

**Özellikler**:
- Araç çıktısını analiz eder
- Potansiyel sorunları tespit eder

**Etkinleştirme koşulu**: `ECC_GOVERNANCE_CAPTURE=1`

**Çıkış kodu**: 0

---

### post:session-activity-tracker

**Tür**: PostToolUse
**matcher**: *
**Tanım**: Her oturumun araç çağrılarını ve dosya aktivitelerini kaydeder

**Özellikler**:
- Araç kullanım istatistiklerini takip eder
- Dosya aktivitesini kaydeder
- ECC2 metrikleri için kullanılır

**Çıkış kodu**: 0

---

### post:observe:continuous-learning

**Tür**: PostToolUse
**matcher**: *
**Tanım**: Sürekli öğrenme sinyallerini desteklemek için araç sonuçlarını kaydeder

**Özellikler**:
- Araç yürütme sonuçlarını yakalar
- Desen analizini destekler

**Çıkış kodu**: 0

---

### post:ecc-metrics-bridge

**Tür**: PostToolUse
**matcher**: *
**Tanım**: Çalışan oturum metrik toplamalarını sürdürür

**Özellikler**:
- Token ve maliyet metriklerini takip eder
- Durum çubuğu ve bağlam izleyicisi tarafından kullanılır

**Çıkış kodu**: 0

---

### post:ecc-context-monitor

**Tür**: PostToolUse
**matcher**: *
**Tanım**: Bağlam tükendiğinde, yüksek maliyette, kapsam sızmasında veya araç döngüsünde aracı uyarı enjekte eder

**Özellikler**:
- Bağlam kullanım izleme
- Maliyet uyarısı
- Kapsam sızması tespiti
- Araç döngüsü tespiti

**Çıkış kodu**: 0

---

### post:mcp-health-check

**Tür**: PostToolUseFailure
**matcher**: *
**Tanım**: Başarısız MCP araç çağrılarını takip eder, sağlıksız sunucuları işaretler ve yeniden bağlanmayı dener

**Özellikler**:
- Başarısızlık takibi
- Sağlık durumu işaretleme
- Otomatik yeniden bağlanma denemeleri

**Çıkış kodu**: 0

---

## Stop Yerleşik Hookları

### stop:format-typecheck

**Tür**: Stop
**matcher**: *
**Tanım**: Bu yanıtta düzenlenen tüm JS/TS dosyalarını toplu biçimlendirir (Biome/Prettier) ve tür kontrolü yapar (tsc)

**Özellikler**:
- Prettier/Biome biçimlendirme
- TypeScript tür kontrolü
- Her düzenlemeden sonra değil, Stop zamanında bir kez çalışır

**Çıkış kodu**: 0

**Zaman aşımı**: 300 saniye

---

### stop:check-console-log

**Tür**: Stop
**matcher**: *
**Tanım**: Her yanıttan sonra değiştirilen dosyalardaki console.log'u kontrol eder

**Özellikler**:
- Değiştirilen dosyaları tarar
- console.log kullanımını raporlar

**Çıkış kodu**: 0

---

### stop:session-end

**Tür**: Stop
**matcher**: *
**Tanım**: Her yanıttan sonra oturum durumunu kalıcı hale getirir (Stop transcript_path taşır)

**Özellikler**:
- Oturum durumu kaydetme
- Bağlam kalıcılığı

**Çıkış kodu**: 0

---

### stop:evaluate-session

**Tür**: Stop
**matcher**: *
**Tanım**: Öğrenilebilir desenleri çıkarmak için oturumu değerlendirir

**Özellikler**:
- Desen tanıma
- Sürekli öğrenme desteği
- Asenkron yürütme

**Çıkış kodu**: 0

---

### stop:cost-tracker

**Tür**: Stop
**matcher**: *
**Tanım**: Her oturumun token ve maliyet metriklerini takip eder

**Özellikler**:
- Token kullanım istatistikleri
- Maliyet tahmini
- Hafif çalışma maliyeti telemetri işaretleme

**Çıkış kodu**: 0

---

### stop:desktop-notify

**Tür**: Stop
**matcher**: *
**Tanım**: Claude yanıt verdiğinde görev özeti içeren macOS/WSL masaüstü bildirimi gönderir

**Özellikler**:
- Masaüstü bildirimi
- Görev özeti görüntüleme

**Çıkış kodu**: 0

---

## SessionStart Yerleşik Hookları

### session:start

**Tür**: SessionStart
**matcher**: *
**Tanım**: Önceki bağlamı yükler ve yeni oturumun paket yöneticisini algılar

**Özellikler**:
- Sınırlı önceki bağlamı yükler
- Proje durumu algılama
- Paket yöneticisi algılama (npm/pnpm/yarn/bun)

**Ortam değişkenleri**:
- `ECC_SESSION_START_MAX_CHARS`: Ekstra bağlam boyutunu kontrol eder (varsayılan 8000 karakter)
- `ECC_SESSION_START_CONTEXT=off`: Ekstra bağlamı tamamen devre dışı bırakır

**Çıkış kodu**: 0

---

## PreCompact Yerleşik Hookları

### pre:compact

**Tür**: PreCompact
**matcher**: *
**Tanım**: Bağlam sıkıştırmadan önce durumu kaydeder

**Özellikler**:
- Oturum durumu kalıcılığı
- Sıkıştırma için bağlam hazırlama

**Çıkış kodu**: 0

---

## SessionEnd Yerleşik Hookları

### session:end:marker

**Tür**: SessionEnd
**matcher**: *
**Tanım**: Oturum sona erme yaşam döngüsü işaretleyicisi

**Özellikler**:
- Yaşam döngüsü olay işaretleme
- Temizlik günlüğü

**Çıkış kodu**: 0

---

## Yerleşik Hook Hızlı Başvuru

### PreToolUse Hookları

| ID | Matcher | Özellik | Engelleme |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | Kalite/tmux/push/GateGuard kontrolleri | Engelleyebilir |
| pre:write:doc-file-warning | Write | Dokümantasyon dosyası uyarısı | Hayır |
| pre:edit-write:suggest-compact | Edit\|Write | Sıkıştırma önerisi | Hayır |
| pre:observe:continuous-learning | * | Sürekli öğrenme gözlemi | Hayır |
| pre:governance-capture | Bash\|Write\|Edit\|MultiEdit | Yönetişim olay yakalama | Hayır |
| pre:config-protection | Write\|Edit\|MultiEdit | Yapılandırma koruması | Hayır |
| pre:mcp-health-check | * | MCP sağlık kontrolü | Engelleyebilir |
| pre:edit-write:gateguard-fact-force | Edit\|Write\|MultiEdit | İlk düzenleme GateGuard | Engelleyebilir |

### PostToolUse Hookları

| ID | Matcher | Özellik | Asenkron |
|----|---------|------|------|
| post:bash:dispatcher | Bash | PR günlüğü/build bildirimleri | Evet |
| post:quality-gate | Edit\|Write\|MultiEdit | Kalite kapısı kontrolü | Evet |
| post:edit:design-quality-check | Edit\|Write\|MultiEdit | Tasarım kalitesi kontrolü | Hayır |
| post:edit:accumulator | Edit\|Write\|MultiEdit | Düzenleme biriktirici | Hayır |
| post:edit:console-warn | Edit | console.log uyarısı | Hayır |
| post:governance-capture | Bash\|Write\|Edit\|MultiEdit | Yönetişim olay yakalama | Hayır |
| post:session-activity-tracker | * | Oturum aktivitesi takibi | Hayır |
| post:observe:continuous-learning | * | Sürekli öğrenme gözlemi | Evet |
| post:ecc-metrics-bridge | * | Metrik köprüsü | Hayır |
| post:ecc-context-monitor | * | Bağlam izleyici | Hayır |
| post:mcp-health-check | * (PostToolUseFailure) | MCP sağlık kontrolü | Hayır |

### Stop Hookları

| ID | Matcher | Özellik | Zaman aşımı |
|----|---------|------|------|
| stop:format-typecheck | * | Toplu biçimlendirme ve tür kontrolü | 300s |
| stop:check-console-log | * | console.log kontrolü | 30s |
| stop:session-end | * | Oturum durumu kalıcılığı | 10s |
| stop:evaluate-session | * | Oturum değerlendirmesi | 10s |
| stop:cost-tracker | * | Maliyet takibi | 10s |
| stop:desktop-notify | * | Masaüstü bildirimi | 10s |

### Yaşam Döngüsü Hookları

| ID | Olay | Özellik |
|----|-------|------|
| session:start | SessionStart | Bağlam yükleme ve paket yöneticisi algılama |
| pre:compact | PreCompact | Sıkıştırma öncesi durum kaydetme |
| session:end:marker | SessionEnd | Oturum sonu işaretleme |
