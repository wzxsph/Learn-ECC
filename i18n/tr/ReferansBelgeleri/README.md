# ECC Dokümantasyon Sistemi

ECC (Everything Claude Code), 75 komut, 60 Agent, 16 Skill, 7 kural dokümanı ve tam Hook/MCP yapılandırma sistemi sunan bir Claude Code eklentisidir.

---

## 1. ECC Dokümantasyon Sistemine Genel Bakış

ECC, Claude Code için üretim düzeyinde geliştirme iş akışı desteği sağlar ve birden fazla AI Agent çerçevesini (Claude Code, Codex, OpenCode, Cursor, Gemini) kapsar.

### Teknoloji Yığını

| Katman | Teknoloji | Sürüm |
|------|------|------|
| Birincil Dil | JavaScript/Node.js | >=18 |
| İkincil Dil | Python | >=3.11 |
| Paket Yöneticisi | Yarn | 4.9.2 |
| Test Çalıştırıcı | Özel Node.js test çalıştırıcı | - |
| Kapsama | c8 | 11.x |
| Kod Kontrolü | ESLint + markdownlint-cli | - |

### Dizin Yapısı

```
ECC/
├── agents/        # 60 profesyonel Agent
├── commands/      # 75 komut dosyası
├── skills/        # 16 Skill dizini
├── rules/         # 7 kural dokümanı (common + dile özel)
├── hooks/         # 3 Hook dokümanı
├── mcp/           # 6 MCP sunucu yapılandırması
├── scripts/       # 54 yardımcı betik
├── README.md
```

---

## 2. Hızlı Navigasyon

| Kategori | Doküman | Açıklama |
|------|------|------|
| [Komutlara Genel Bakış](./commands/) | [commands/](commands/) | 75 komutun tam listesi |
| [Agent Dizini](./agents/) | [agents/](agents/) | 60 profesyonel Agent |
| [Skills Dizini](./skills/) | [skills/](skills/) | 16 Skill alana göre sınıflandırılmış |
| [Kurallar Dizini](./rules/) | [rules/](rules/) | 7 kural dokümanı (common + dile özel) |
| [Hooks Dizini](./hooks/) | [hooks/](hooks/) | Hook sistem yapılandırması ve geliştirme |
| [MCP Dizini](./mcp/) | [mcp/](mcp/) | MCP sunucu yapılandırması ve geliştirme |
| [Scripts Dizini](./scripts/) | [scripts/](scripts/) | 54 yardımcı betik |

---

## 3. Komut Dizini

Toplam **75 komut**, kategorilere göre gruplandırılmış:

### 3.1 Çekirdek İş Akışları (6)

| Komut | Dosya | Amaç |
|------|------|------|
| `/plan` | [01-ÇekirdekİşAkışları.md](commands/01-ÇekirdekİşAkışları.md) | Gereksinim analizi + risk değerlendirmesi + adım adım plan |
| `/code-review` | [01-ÇekirdekİşAkışları.md](commands/01-ÇekirdekİşAkışları.md) | Kod kalitesi/güvenlik/bakım kolaylığı incelemesi |
| `/build-fix` | [01-ÇekirdekİşAkışları.md](commands/01-ÇekirdekİşAkışları.md) | Otomatik dil algılama + artımlı build hatası düzeltme |
| `/verify` | [01-ÇekirdekİşAkışları.md](commands/01-ÇekirdekİşAkışları.md) | Tam doğrulama döngüsü: build → lint → test → tip kontrolü |
| `/quality-gate` | [01-ÇekirdekİşAkışları.md](commands/01-ÇekirdekİşAkışları.md) | İsteğe bağlı ECC kalite hattı çalıştırma |
| `/tdd` | [01-ÇekirdekİşAkışları.md](commands/01-ÇekirdekİşAkışları.md) | Genel TDD iş akışı |

### 3.2 Testle İlgili (8)

| Komut | Dosya | Amaç |
|------|------|------|
| `/go-test` | [02-Testleİlgili.md](commands/02-Testleİlgili.md) | Go TDD (tablo güdümlü, %80+ kapsama) |
| `/kotlin-test` | [02-Testleİlgili.md](commands/02-Testleİlgili.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-Testleİlgili.md](commands/02-Testleİlgili.md) | Rust TDD (cargo test + entegrasyon testleri) |
| `/cpp-test` | [02-Testleİlgili.md](commands/02-Testleİlgili.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-Testleİlgili.md](commands/02-Testleİlgili.md) | Flutter TDD |
| `/e2e` | [02-Testleİlgili.md](commands/02-Testleİlgili.md) | Playwright e2e testleri oluştur + çalıştır |
| `/test-coverage` | [02-Testleİlgili.md](commands/02-Testleİlgili.md) | Test kapsama raporu + boşluk analizi |
| `/python-testing` | [02-Testleİlgili.md](commands/02-Testleİlgili.md) | Python test en iyi uygulamaları |

### 3.3 Dil İnceleme (7)

| Komut | Dosya | Amaç |
|------|------|------|
| `/python-review` | [03-Dilİnceleme.md](commands/03-Dilİnceleme.md) | Python PEP 8, tip ipuçları, güvenlik |
| `/go-review` | [03-Dilİnceleme.md](commands/03-Dilİnceleme.md) | Go deyimleri, eşzamanlılık, hata işleme |
| `/kotlin-review` | [03-Dilİnceleme.md](commands/03-Dilİnceleme.md) | Kotlin null güvenliği, coroutine'ler, mimari |
| `/rust-review` | [03-Dilİnceleme.md](commands/03-Dilİnceleme.md) | Rust sahiplik, yaşam süreleri, unsafe |
| `/cpp-review` | [03-Dilİnceleme.md](commands/03-Dilİnceleme.md) | C++ bellek güvenliği, modern deyimler |
| `/flutter-review` | [03-Dilİnceleme.md](commands/03-Dilİnceleme.md) | Flutter/Dart kalıpları |
| `/fastapi-review` | [03-Dilİnceleme.md](commands/03-Dilİnceleme.md) | FastAPI mimarisi, async, Pydantic |

### 3.4 Build Düzeltme (6)

| Komut | Dosya | Amaç |
|------|------|------|
| `/go-build` | [04-BuildDüzeltme.md](commands/04-BuildDüzeltme.md) | Go build hatalarını + go vet uyarılarını düzelt |
| `/kotlin-build` | [04-BuildDüzeltme.md](commands/04-BuildDüzeltme.md) | Kotlin/Gradle derleyici hatalarını düzelt |
| `/rust-build` | [04-BuildDüzeltme.md](commands/04-BuildDüzeltme.md) | Rust build + ödünç kontrolörü sorunlarını düzelt |
| `/cpp-build` | [04-BuildDüzeltme.md](commands/04-BuildDüzeltme.md) | C++ CMake + bağlayıcı sorunlarını düzelt |
| `/gradle-build` | [04-BuildDüzeltme.md](commands/04-BuildDüzeltme.md) | Android/KMP Gradle hatalarını düzelt |
| `/flutter-build` | [04-BuildDüzeltme.md](commands/04-BuildDüzeltme.md) | Flutter build düzeltmeleri |

### 3.5 Planlama ve Mimari (7)

| Komut | Dosya | Amaç |
|------|------|------|
| `/plan-prd` | [05-PlanlaveMimari.md](commands/05-PlanlaveMimari.md) | Etkileşimli PRD oluşturucu |
| `/prp-plan` | [05-PlanlaveMimari.md](commands/05-PlanlaveMimari.md) | Kapsamlı özellik planlama |
| `/prp-prd` | [05-PlanlaveMimari.md](commands/05-PlanlaveMimari.md) | PRP iş akışı PRD oluşturucu |
| `/prp-implement` | [05-PlanlaveMimari.md](commands/05-PlanlaveMimari.md) | PRP planını çalıştır + doğrulama döngüsü |
| `/prp-pr` | [05-PlanlaveMimari.md](commands/05-PlanlaveMimari.md) | PRP iş akışından PR oluştur |
| `/prp-commit` | [05-PlanlaveMimari.md](commands/05-PlanlaveMimari.md) | PRP doğrulamalı commit |
| `/multi-plan` | [05-PlanlaveMimari.md](commands/05-PlanlaveMimari.md) | Çoklu model işbirlikçi planlama (Codex + Gemini) |

### 3.6 Oturum Yönetimi (5)

| Komut | Dosya | Amaç |
|------|------|------|
| `/save-session` | [06-OturumYönetimi.md](commands/06-OturumYönetimi.md) | Oturum durumunu kaydet |
| `/resume-session` | [06-OturumYönetimi.md](commands/06-OturumYönetimi.md) | Kayıtlı oturumu yükle ve geri yükle |
| `/sessions` | [06-OturumYönetimi.md](commands/06-OturumYönetimi.md) | Oturum geçmişini göz at + ara + yönet |
| `/checkpoint` | [06-OturumYönetimi.md](commands/06-OturumYönetimi.md) | İş akışı checkpoint oluştur / doğrula |
| `/aside` | [06-OturumYönetimi.md](commands/06-OturumYönetimi.md) | Bağlamı kaybetmeden yan soruları yanıtla |

### 3.7 Öğrenme ve İyileştirme (10)

| Komut | Dosya | Amaç |
|------|------|------|
| `/learn` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | Oturumdan yeniden kullanılabilir kalıplar çıkar |
| `/learn-eval` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | Kalıp çıkar + kalite öz-değerlendirmesi |
| `/evolve` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | İçgüdüyü analiz et + evrim yapısı öner |
| `/promote` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | Proje içgüdüsünü global kapsama yükselt |
| `/instinct-status` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | Tüm öğrenilen içgüdüleri göster |
| `/instinct-export` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | İçgüdüyü dosyaya aktar |
| `/instinct-import` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | İçgüdüyü dosya/URL'den içe aktar |
| `/skill-create` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | Git geçmişini analiz et + skill dosyası oluştur |
| `/skill-health` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | Skill bileşimi sağlık panosu |
| `/rules-distill` | [07-Öğreniveİyileştirme.md](commands/07-Öğreniveİyileştirme.md) | Skill'leri tara + alanlar arası ilkeler çıkar |

### 3.8 Döngüler ve Otomasyon (3)

| Komut | Dosya | Amaç |
|------|------|------|
| `/loop-start` | [08-DöngülerveOtomasyon.md](commands/08-DöngülerveOtomasyon.md) | Yönetilen özerk döngü modunu başlat |
| `/loop-status` | [08-DöngülerveOtomasyon.md](commands/08-DöngülerveOtomasyon.md) | Çalışan döngünün durumunu kontrol et |
| `/santa-loop` | [08-DöngülerveOtomasyon.md](commands/08-DöngülerveOtomasyon.md) | Santa tarzı özerk döngü |

### 3.9 Proje Yönetimi (6)

| Komut | Dosya | Amaç |
|------|------|------|
| `/projects` | [09-ProjeYönetimi.md](commands/09-ProjeYönetimi.md) | Bilinen projeleri listele + içgüdü istatistikleri |
| `/harness-audit` | [09-ProjeYönetimi.md](commands/09-ProjeYönetimi.md) | Agent harness yapılandırmasını denetle |
| `/model-route` | [09-ProjeYönetimi.md](commands/09-ProjeYönetimi.md) | Görevleri uygun modele yönlendir |
| `/pm2` | [09-ProjeYönetimi.md](commands/09-ProjeYönetimi.md) | PM2 süreç yöneticisi başlatma |
| `/setup-pm` | [09-ProjeYönetimi.md](commands/09-ProjeYönetimi.md) | Paket yöneticisini yapılandır |
| `/project-init` | [09-ProjeYönetimi.md](commands/09-ProjeYönetimi.md) | Proje başlatma |

### 3.10 PR İş Akışı (6)

| Komut | Dosya | Amaç |
|------|------|------|
| `/pr` | [10-PRİşAkışı.md](commands/10-PRİşAkışı.md) | Mevcut dalndan GitHub PR oluştur |
| `/review-pr` | [10-PRİşAkışı.md](commands/10-PRİşAkışı.md) | GitHub PR'u incele |
| `/multi-workflow` | [10-PRİşAkışı.md](commands/10-PRİşAkışı.md) | Çoklu model işbirlikçi geliştirme |
| `/multi-backend` | [10-PRİşAkışı.md](commands/10-PRİşAkışı.md) | Backend çoklu model geliştirme |
| `/multi-frontend` | [10-PRİşAkışı.md](commands/10-PRİşAkışı.md) | Frontend çoklu model geliştirme |
| `/multi-execute` | [10-PRİşAkışı.md](commands/10-PRİşAkışı.md) | Çoklu model işbirlikçi yürütme |

### 3.11 Hookify Sistemi (4)

| Komut | Dosya | Amaç |
|------|------|------|
| `/hookify` | [11-HookifySistemi.md](commands/11-HookifySistemi.md) | Kötü davranışları önlemek için hook oluştur |
| `/hookify-list` | [11-HookifySistemi.md](commands/11-HookifySistemi.md) | Tüm yapılandırılmış hookify kurallarını listele |
| `/hookify-configure` | [11-HookifySistemi.md](commands/11-HookifySistemi.md) | Hookify kurallarını etkileşimli etkinleştir/devre dışı bırak |
| `/hookify-help` | [11-HookifySistemi.md](commands/11-HookifySistemi.md) | Hookify sistemi yardımı |

### 3.12 Dokümantasyon ve Araştırma (3)

| Komut | Dosya | Amaç |
|------|------|------|
| `/update-docs` | [12-DokümantasyonveAraştırma.md](commands/12-DokümantasyonveAraştırma.md) | Proje dokümantasyonunu güncelle |
| `/update-codemaps` | [12-DokümantasyonveAraştırma.md](commands/12-DokümantasyonveAraştırma.md) | Codemap'leri yeniden oluştur |
| `/ecc-guide` | [12-DokümantasyonveAraştırma.md](commands/12-DokümantasyonveAraştırma.md) | ECC kullanıcı rehberi |

### 3.13 Yeniden Düzenleme ve Temizleme (2)

| Komut | Dosya | Amaç |
|------|------|------|
| `/refactor-clean` | [13-YenidenDüzenlemeveTemizleme.md](commands/13-YenidenDüzenlemeveTemizleme.md) | Ölü kodu sil + tekrarları birleştir |
| `/auto-update` | [13-YenidenDüzenlemeveTemizleme.md](commands/13-YenidenDüzenlemeveTemizleme.md) | Otomatik güncelleme yetenekleri |

### 3.14 Diğer Komutlar (7)

| Komut | Dosya | Amaç |
|------|------|------|
| `/jira` | [14-DiğerKomutlar.md](commands/14-DiğerKomutlar.md) | Jira bilet etkileşimi |
| `/gan-build` | [14-DiğerKomutlar.md](commands/14-DiğerKomutlar.md) | GAN build işlemleri |
| `/gan-design` | [14-DiğerKomutlar.md](commands/14-DiğerKomutlar.md) | GAN tasarım işlemleri |
| `/prune` | [14-DiğerKomutlar.md](commands/14-DiğerKomutlar.md) | Eski içgüdüleri sil (>30 gün) |
| `/security-scan` | [14-DiğerKomutlar.md](commands/14-DiğerKomutlar.md) | Güvenlik taraması |
| `/feature-dev` | [14-DiğerKomutlar.md](commands/14-DiğerKomutlar.md) | Özellik geliştirme asistanı |
| `/cost-report` | [14-DiğerKomutlar.md](commands/14-DiğerKomutlar.md) | Model maliyet raporu |

---

## 4. Agent Dizini

Toplam **60 Agent**, kategorilere göre gruplandırılmış:

| Agent Kategorisi | Dosya | Açıklama |
|------------|------|-------------|
| [Kod İnceleme](agents/1.%20Kodİnceleme.md) | [1. Kodİnceleme.md](agents/1.%20Kodİnceleme.md) | 14 inceleme Agent'i |
| [Build Düzeltme](agents/2.%20BuildDüzeltme.md) | [2. BuildDüzeltme.md](agents/2.%20BuildDüzeltme.md) | 14 build düzeltme Agent'i |
| [Planlama](agents/3.%20Planlama.md) | [3. Planlama.md](agents/3.%20Planlama.md) | 5 planlama Agent'i |
| [Test](agents/4.%20Test.md) | [4. Test.md](agents/4.%20Test.md) | 2 test Agent'i |
| [Güvenlik](agents/5.%20Güvenlik.md) | [5. Güvenlik.md](agents/5.%20Güvenlik.md) | 3 güvenlik Agent'i |
| [Mimari](agents/6.%20Mimari.md) | [6. Mimari.md](agents/6.%20Mimari.md) | 3 mimari Agent'i |

---

## 5. Skills Dizini

**16 Skill alanı**, alana göre sınıflandırılmış:

| Alan | Dosya | Açıklama |
|------|------|-------------|
| [En İyi Uygulamalar](skills/EnİyiUygulamalar.md) | [EnİyiUygulamalar.md](skills/EnİyiUygulamalar.md) | Kodlama standartları, hata işleme, özerk döngüler |
| [Programlama Dilleri](skills/ProgramlamaDilleri.md) | [ProgramlamaDilleri.md](skills/ProgramlamaDilleri.md) | Python/Go/Rust/Kotlin/C++ vb. |
| [Framework'ler](skills/Frameworkler.md) | [Frameworkler.md](skills/Frameworkler.md) | Django/Laravel/NestJS/Spring Boot vb. |
| [Test](skills/Test.md) | [Test.md](skills/Test.md) | TDD/Ünite Testi/Entegrasyon Testi/E2E |
| [Güvenlik](skills/Güvenlik.md) | [Güvenlik.md](skills/Güvenlik.md) | Güvenlik incelemesi, güvenlik açığı taraması |
| [Frontend ve Tasarım](skills/FrontendveTasarım.md) | [FrontendveTasarım.md](skills/FrontendveTasarım.md) | Frontend geliştirme, tasarım sistemleri |
| [Backend ve API](skills/BackendveAPI.md) | [BackendveAPI.md](skills/BackendveAPI.md) | Backend servisleri, API tasarımı, veritabanları |
| [Deployment ve DevOps](skills/DeploymentveDevOps.md) | [DeploymentveDevOps.md](skills/DeploymentveDevOps.md) | Docker/K8s/dağıtım stratejileri |
| [İzleme ve Gözlemlenebilirlik](skills/İzlemeveGözlemlenebilirlik.md) | [İzlemeveGözlemlenebilirlik.md](skills/İzlemeveGözlemlenebilirlik.md) | Gözlemlenebilirlik, ağ tanılama |
| [Otomasyon ve Betik](skills/OtomasyonveBetik.md) | [OtomasyonveBetik.md](skills/OtomasyonveBetik.md) | Özerk döngüler, sürekli öğrenme, aracı mühendisliği |
| [Arama ve Veri Alma](skills/AramaveVeriAlma.md) | [AramaveVeriAlma.md](skills/AramaveVeriAlma.md) | Exa arama, web kazıma, MCP |
| [GitHub ve İşbirliği](skills/GitHubveİşbirliği.md) | [GitHubveİşbirliği.md](skills/GitHubveİşbirliği.md) | GitHub iş akışları, kod inceleme |
| [AI ve Makine Öğrenmesi](skills/AIveMakineÖğrenmesi.md) | [AIveMakineÖğrenmesi.md](skills/AIveMakineÖğrenmesi.md) | Sinir ağları, PyTorch, MLOps |
| [Cloud Native ve Altyapı](skills/CloudNativeveAltyapı.md) | [CloudNativeveAltyapı.md](skills/CloudNativeveAltyapı.md) | Kubernetes, Docker, Terraform |
| [Özel Alan Becerileri](skills/ÖzelAlanBecerileri.md) | [ÖzelAlanBecerileri.md](skills/ÖzelAlanBecerileri.md) | Blockchain, oyun geliştirme, ses/video, IoT |
| [Geliştirme Araç Zinciri](skills/GeliştirmeAraçZinciri.md) | [GeliştirmeAraçZinciri.md](skills/GeliştirmeAraçZinciri.md) | Test framework'leri, CI/CD, kod kalitesi |
| [Öncü Teknolojiler](skills/ÖncüTeknolojiler.md) | [ÖncüTeknolojiler.md](skills/ÖncüTeknolojiler.md) | AI Agent, kuantum hesaplama, edge computing |

---

## 6. Kurallar Dizini

Toplam **7 kural dokümanı** (common + dile özel):

| Kurallar | Dosya | Açıklama |
|-------|------|-------------|
| [Git İş Akışı](rules/GitİşAkışı.md) | [GitİşAkışı.md](rules/GitİşAkışı.md) | Git commit standartları ve PR iş akışı |
| [Hooks Sistemi](rules/HooksSistemi.md) | [HooksSistemi.md](rules/HooksSistemi.md) | Hook yapılandırma ve kullanım rehberi |
| [Aracı Orkestrasyonu](rules/AracıOrkestrasyonu.md) | [AracıOrkestrasyonu.md](rules/AracıOrkestrasyonu.md) | Agent orkestrasyon kalıpları |
| [Performans Optimizasyonu](rules/PerformansOptimizasyonu.md) | [PerformansOptimizasyonu.md](rules/PerformansOptimizasyonu.md) | Performans optimizasyonu rehberi |
| [Kod Stili](rules/KodStili.md) | [KodStili.md](rules/KodStili.md) | Kodlama stili standartları |
| [Test Kuralları](rules/TestKuralları.md) | [TestKuralları.md](rules/TestKuralları.md) | Test gereksinimleri (%80 kapsama) |
| [Güvenlik Kuralları](rules/GüvenlikKuralları.md) | [GüvenlikKuralları.md](rules/GüvenlikKuralları.md) | Güvenlik kontrol listesi |

---

## 7. Hooks Dizini

Toplam **4 doküman**:

| Doküman | Dosya | Açıklama |
|----------|------|-------------|
| [Hook Türleri](hooks/HookTürleri.md) | [HookTürleri.md](hooks/HookTürleri.md) | PreToolUse, PostToolUse, Stop türleri |
| [Dahili Hooks](hooks/DahiliHooks.md) | [DahiliHooks.md](hooks/DahiliHooks.md) | Dahili hook listesi ve kullanım |
| [Özel Geliştirme](hooks/ÖzelGeliştirme.md) | [ÖzelGeliştirme.md](hooks/ÖzelGeliştirme.md) | Özel hook geliştirme rehberi |
| [Yapılandırma Formatı](hooks/YapılandırmaFormatı.md) | [YapılandırmaFormatı.md](hooks/YapılandırmaFormatı.md) | hooks.json yapılandırma formatı |

---

## 8. MCP Dizini

Toplam **6 MCP sunucu yapılandırması**:

| Doküman | Dosya | Açıklama |
|----------|------|-------------|
| [MCP Yapılandırma Formatı](mcp/MCPYapılandırmaFormatı.md) | [MCPYapılandırmaFormatı.md](mcp/MCPYapılandırmaFormatı.md) | MCP yapılandırma dosyası formatı |
| [Dahili Sunucular](mcp/DahiliSunucular.md) | [DahiliSunucular.md](mcp/DahiliSunucular.md) | Dahili MCP sunucuları |
| [Özel Geliştirme](mcp/ÖzelGeliştirme.md) | [ÖzelGeliştirme.md](mcp/ÖzelGeliştirme.md) | Özel MCP sunucu geliştirme |

---

## 9. Scripts Dizini

Toplam **54 yardımcı betik**:

| Doküman | Dosya | Açıklama |
|----------|------|-------------|
| [Yardımcı Betikler](scripts/YardımcıBetikler.md) | [YardımcıBetikler.md](scripts/YardımcıBetikler.md) | ecc.js, install-apply.js vb. |
| [Yardımcı Fonksiyon Kütüphanesi](scripts/YardımcıFonksiyonKütüphanesi.md) | [YardımcıFonksiyonKütüphanesi.md](scripts/YardımcıFonksiyonKütüphanesi.md) | Paylaşılan fonksiyon kütüphanesi |
| [Test Çalıştırıcı](scripts/TestÇalıştırıcı.md) | [TestÇalıştırıcı.md](scripts/TestÇalıştırıcı.md) | Test çalıştırıcı kullanımı |
| [Build Betikleri](scripts/BuildBetikleri.md) | [BuildBetikleri.md](scripts/BuildBetikleri.md) | Build betikleri |

---

## 10. Katkı Rehberi

### Dosya Adlandırma Kuralları

- **Komutlar, Skills, Agents, Hooks**: Küçük harf + tire (örn. `code-review.md`)
- **Betikler**: camelCase veya kebab-case (örn. `session-start.js`)
- **Kurallar**: Dil/konuya göre organize edilmiş

### Komut Dosyası Formatı

```yaml
---
description: "Kısa açıklama"
argument-hint: "[İsteğe bağlı parametre]"
name: command-name
command: true
allowed_tools: ["Bash"]
---
```

### Agent Dosyası Formatı

```yaml
---
name: agent-name
description: Agent amacı
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

### Skill Dosyası Formatı

```markdown
# Skill Adı

## When to Use
## How It Works
## Examples
```

### Gönderim Süreci

1. Yeni dosya oluştur veya mevcut dosyayı değiştir
2. Adlandırma kurallarına uyduğundan emin ol
3. Testleri çalıştır: `node tests/run-all.js`
4. Markdown lint çalıştır: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. İnceleme için PR oluştur

---

*ECC Dokümantasyon Sistemi - Claude Code için üretim düzeyinde geliştirme iş akışları*