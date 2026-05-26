# 03 - Temel Komutlar

## Komut Turleri

ECC uc komut turu saglar:

### 1. Satir Komutlari (Slash Commands)

Dogudan kullanilir:

| Komut | Islev | Ornek |
|------|------|------|
| `/ecc:plan` | Uygulama plani olustur | `/ecc:plan "Kullanici kimlik dogrulama ekle"` |
| `/ecc:code-review` | Kod inceleme | `/ecc:code-review` |
| `/ecc:build-fix` | Build hatalarini tamir et | `/ecc:build-fix` |
| `/ecc:learn` | Oturumdan kalip cikar | `/ecc:learn` |
| `/ecc:skill-create` | Git gecmisinden Skill olustur | `/ecc:skill-create` |
| `/ecc:sessions` | Oturum gecmisi yonetimi | `/ecc:sessions` |

Not: Manuel kurulum sürümü on ek olmadan komut kullanabilir (orn. `[/plan](../Referans-belgeleri/commands/01-Cekirdek-Is-Akis.md)`), eklenti kurulumu `/ecc:` on eki kullanir.

### 2. Skills (Ana Is Akisi Yuzeyi)

Skills, ECC'nin ana is akisi cagirna yontemidir:

| Skill | Islev |
|-------|------|
| `[tdd-workflow](../Referans-belgeleri/skills/Test.md)` | Test-driven gelistirme |
| `[e2e-testing](../Referans-belgeleri/skills/Test.md)` | Ucten uca test |
| `security-review` | Guvenlik inceleme |
| `verification-loop` | Dogrulama dongusu |
| `search-first` | Arastirma oncelikli is akisi |

Cagirna yontemi:
```
[tdd-workflow](../Referans-belgeleri/skills/Test.md) skill'ini kullan
```

### 3. Agent Komutlari

Alt gorevleri Agent uzerinden calistir:

| Agent | Kullanim Amaci |
|-------|------|
| `planner` | Uygulama planlamasi |
| `code-reviewer` | Kod inceleme |
| `build-error-resolver` | Build hatasi tamiri |
| `security-reviewer` | Guvenlik inceleme |
| `architect` | Mimari tasarim |

Cagirna yontemi:
```
@planner
```

## Sik Kullanilan Is Akislari

### Yeni Ozellik Gelistirme

```
/ecc:plan "Kullanici kimlik dogrulama ozelligi ekle"
→ planner uygulama plani olusturur
[tdd-workflow](../Referans-belgeleri/skills/Test.md) skill'ini kullan
→ tdd-guide once test yazmayi zorunlu tutar
/ecc:code-review
→ code-reviewer kodu kontrol eder
```

### Bug Duzeltme

```
[tdd-workflow](../Referans-belgeleri/skills/Test.md) skill'ini kullan
→ tdd-guide: yeniden üretme testi yazar
→ Duzeltmeyi uygula, testlerin gectigini dogrula
/ecc:code-review
→ code-reviewer: geriye donuk uyumlulugu kontrol eder
```

### Yayina Hazirlik

```
/ecc:security-scan
→ security-reviewer: OWASP Top 10 denetimi
[e2e-testing](../Referans-belgeleri/skills/Test.md) skill'ini kullan
→ e2e-runner: Kritik kullanici akislarini test eder
/ecc:test-coverage
→ %80+ kapsama dogrulaması
```

## MCP Komutlari

| Komut | Islev |
|------|------|
| `/mcp` | MCP sunucularini yonet |
| `/ecc:status` | Durumu kontrol et |

## Sonraki Adimlar

- [Ilk Gorev](./04-Ilk-Gorev.md) - Ilk gorevinizi tamamlayin
- [Baslangic icindekilerine don](../README.md)