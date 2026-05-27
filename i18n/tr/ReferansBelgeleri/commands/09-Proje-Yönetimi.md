# Proje Yönetimi Komutları

## Genel Bakış

Proje yönetimi komutları, proje yapılandırması, altyapı kurulumu ve araç zinciri yönetimi için kullanılır.

## Komut Listesi

### /projects

**Kullanım**: Bilinen projeleri ve içgüdü istatistiklerini listele

**Tanım**: Kullanıcının tüm bilinen projelerini ve ilgili içgüdü istatistiklerini listeler.

---

### /harness-audit

**Kullanım**: Ajan harness yapılandırmasını denetle

**Tanım**: Claude Code'un harness yapılandırmasını kontrol eder ve denetler.

**Kontrol Edilen İçerik**:
- Araç zinciri yapılandırması
- Ajan ayarları
- MCP Sunucu durumu
- Kural yapılandırması

---

### /model-route

**Kullanım**: Görevleri uygun modele yönlendir

**Tanım**: Görev türüne göre en uygun modeli seçer (Haiku/Sonnet/Opus).

**Model Seçim Kılavuzu**:
- **Haiku**: Hafif görevler, kod üretimi, sık çağrılar
- **Sonnet**: Ana geliştirme işleri, karmaşık kodlama görevleri
- **Opus**: Mimari kararlar, derin çıkarım, araştırma analizi

---

### /pm2

**Kullanım**: PM2 süreç yöneticisi başlatma

**Tanım**: PM2 süreç yöneticisi yapılandırmasını başlatır.

---

### /setup-pm

**Kullanım**: Paket yöneticisi yapılandır

**Tanım**: Projede kullanılan paket yöneticisini yapılandırır.

**Desteklenen Paket Yöneticileri**:
- npm
- pnpm
- yarn
- bun

**`CLAUDE_PACKAGE_MANAGER` ortam değişkeni ile yapılandırılabilir**

---

### /project-init

**Kullanım**: Proje başlatma

**Tanım**: Yeni proje için standart yapı ve yapılandırma başlatır.

---

## İlgili Komutlar

- `/projects` - Projeleri listele
- `/harness-audit` - Yapılandırmayı denetle
- `/model-route` - Model yönlendirme
