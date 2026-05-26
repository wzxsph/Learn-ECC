# 02 - Kurulum

## Ortam Gereksinimleri

- Node.js >= 18
- npm / pnpm / yarn / bun herhangi biri
- Claude Code CLI v2.1.0+ kurulu

## Kurulum Yontemleri

### Yontem 1: Eklenti Kurulumu (Tavsiye Edilen)

#### 1. Eklenti pazarini ekle

```bash
/plugin marketplace add https://github.com/affaan-m/ECC
```

#### 2. Eklentiyi kur

```bash
/plugin install ecc@ecc
```

#### 3. Rules'u kur (Eklenti otomatik kurmaz)

```bash
# Depoyu klonla
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Kurallari kullanici dizinine kopyala
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
# Teknoloji yigininizi secin
cp -r rules/typescript ~/.claude/rules/ecc/   # veya python, golang, swift, php
```

### Yontem 2: Manuel Kurulum

Daha hassas kontrol istiyorsaniz:

```bash
# Depoyu klonla
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Bagimliliklari kur
npm install        # veya pnpm install | yarn install | bun install

# Agents'i kur
cp agents/*.md ~/.claude/agents/

# Skills'i kur (ana is akisi yuzeyi)
mkdir -p ~/.claude/skills/ecc
cp -r .agents/skills/* ~/.claude/skills/ecc/

# Rules'u kur
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
cp -r rules/typescript ~/.claude/rules/ecc/

# Commands'u kur (Istege bagli, ECC skills'e gecis yapiyor)
mkdir -p ~/.claude/commands
cp commands/*.md ~/.claude/commands/
```

## Kurulumu Dogrula

```bash
# Eklentinin basariyla kurulup kurulmadigini kontrol et
/plugin list ecc@ecc

# Kullanilabilir komutlari kontrol et
/ecc:plan
/ecc:code-review
```

## Temel Yapilandirma

### Paket Yoneticisini Ayarla

ECC otomatik olarak favori paket yoneticinizi algilar, ancak manuel olarak da ayarlayabilirsiniz:

```bash
# Ortam degiskeni yontemi
export CLAUDE_PACKAGE_MANAGER=pnpm

# veya komut kullanarak
/setup-pm
```

Oncelik: Ortam degiskeni > Proje yapilandirmasi > package.json > lock dosyasi > Genel yapilandirma

### Hook Calisma Zamani Kontrolu

```bash
# Hook sikiligi (varsayilan: standard)
export ECC_HOOK_PROFILE=standard

# Belirli Hooks'u devre disi birak
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# SessionStart baglam上限 (varsayilan: 8000 karakter)
export ECC_SESSION_START_MAX_CHARS=4000
```

### Model Ayarlari

`~/.claude/settings.json` icinde ayarlanmasi tavsiye edilir:

```json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "50"
  }
}
```

## Sonraki Adimlar

- [Temel Komutlar](./03-Temel-Komutlar.md) - Sik kullanilan komutlari ogrenin
- [Baslangic icindekilerine don](../README.md)