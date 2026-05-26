# 01 - [ECC](../Referans-belgeleri/README.md) Tanitimi

## [ECC](../Referans-belgeleri/README.md) Nedir?

[ECC](../Referans-belgeleri/README.md) (Everything Claude Code), uretim-düzeyinde [Agent](../Referans-belgeleri/agents/1.%20Kod-Inceleme.md), [Skills](../Referans-belgeleri/skills/En-Iyi-Uygulamalar.md), hooks, rules, [Commands](../Referans-belgeleri/commands/01-Cekirdek-Is-Akis.md) ve MCP yapilandirma saglayan bir Claude Code eklenti sistemidir. Anthropic Hackathon kazanan projesinden dogmustur ve 10+ aydir gunluk kullanima tabi tutulmustur.

## [ECC](../Referans-belgeleri/README.md) Ne Yapabilir?

### 1. [Agent](../Referans-belgeleri/agents/1.%20Kod-Inceleme.md) Sistemi
[ECC](../Referans-belgeleri/README.md) coklu dil inceleme, build tamiri, mimari tasarim ve benzeri alanlarda 60+ uzman [Agent](../Referans-belgeleri/agents/1.%20Kod-Inceleme.md) saglar:

| [Agent](../Referans-belgeleri/agents/1.%20Kod-Inceleme.md) | Kullanim |
|-------|------|
| `planner` | Islevsellik uygulama planlamasi |
| `code-reviewer` | Kod kalitesi ve guvenlik incelemesi |
| `security-reviewer` | Guvenlik acigi analizi |
| `build-error-resolver` | Build hatasi tamiri |
| `architect` | Sistem mimari tasarimi |
| `tdd-guide` | Test-driven gelistirme rehberi |

### 2. [Skills](../Referans-belgeleri/skills/En-Iyi-Uygulamalar.md) Is Akislari
[Skills](../Referans-belgeleri/skills/En-Iyi-Uygulamalar.md) ana is akisi yuzeyidir ve [ECC](../Referans-belgeleri/README.md) 232+ [Skills](../Referans-belgeleri/skills/En-Iyi-Uygulamalar.md) saglar:

- `tdd-workflow` - Test-driven gelistirme
- `e2e-testing` - Ucten uca test
- `security-review` - Guvenlik inceleme kontrol listesi
- `continuous-learning-v2` - Instinct tabanli surekli ogrenme
- `eval-harness` - Dogrulama dongusu degerlendirmesi

### 3. [Commands](../Referans-belgeleri/commands/01-Cekirdek-Is-Akis.md) Komutlari
Geleneksel satir komutu uyumlulugu korunur (toplam 75+ komut):

| Komut | Islev |
|------|------|
| `/plan` | Uygulama plani olustur |
| `/code-review` | Kod inceleme |
| `/build-fix` | Build hatalarini tamir et |
| `/learn` | Oturumdan kalip cikar |
| `/skill-create` | Git gecmisinden Skill olustur |
| `/sessions` | Oturum gecmisi yonetimi |

### 4. Hooks Otomasyonu
Esnek hook sistemi (8 olay turu):
- `SessionStart` - Oturum basladiginda baglam yukle
- `SessionEnd` - Oturum bittiginde durumu kaydet
- `PreToolUse` - Aracin calistirilmadan once dogrula
- `PostToolUse` - Arac calistirildiktan sonra formatla

### 5. Rules Kurallar Sistemi
Kapsamli gelistirme standartlari (ortak + dile ozel):
- **common/** - Genel ilkeler (guvenlik, kod stili, test, performans)
- **typescript/** - TypeScript/JavaScript
- **python/** - Python
- **golang/** - Go
- **swift/** - Swift
- **php/** - PHP
- **arkts/** - HarmonyOS/ArkTS

### 6. MCP Hizmetleri
14+ MCP sunucu yapilandirmasini destekler: GitHub, Supabase, Vercel, Railway, Context7, Exa, Playwright ve digerleri.

## Coklu Platform Destegi

[ECC](../Referans-belgeleri/README.md) cesitli AI programlama araclarini destekler:

| Arayuz | Agents | Commands | Skills | Hooks |
|------|--------|----------|--------|-------|
| Claude Code | 60 | 75 | 232 | 8 tur |
| Cursor | 48 | Paylasimli | Paylasimli | 15 tur |
| Codex | Paylasimli | Komut tabanli | 32 | - |
| OpenCode | 12 | 35 | 37 | 11 tur |
| GitHub Copilot | - | 6 prompts | Komut tabanli | - |

## Temel Kavramlar

### [Agent](../Referans-belgeleri/agents/1.%20Kod-Inceleme.md)
Yeniden kullanilabilir alt gorev yurutucu, her [Agent](../Referans-belgeleri/agents/1.%20Kod-Inceleme.md)'in acik bir sorumlulugu vardir. Birden fazla [Agent](../Referans-belgeleri/agents/1.%20Kod-Inceleme.md) birlesitirerek karmaşik is akislari olusturulur.

### [Skill](../Referans-belgeleri/skills/En-Iyi-Uygulamalar.md)
Belirli bir gorevi tamamlamak icin is akisini tanimlar, [ECC](../Referans-belgeleri/README.md)'nin ana is akisi yuzeyidir.

### Hook
Belirli zamanlarda otomatik olarak tetiklenen betikler, is akisi otomasyonunu destekler.

### Rule
Claude Code'un uymasi gereken zorunlu kurallar, cikti kalitesini garanti eder.


- [Kurulum](./02-Kurulum.md) - [ECC](../Referans-belgeleri/README.md) kullanmaya baslayin
- [Baslangic icindekilerine don](./README.md)