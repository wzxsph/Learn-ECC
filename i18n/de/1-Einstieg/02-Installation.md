# 02 - Installation und Konfiguration

## Umgebungsanforderungen

- Node.js >= 18
- Eines von npm / pnpm / yarn / bun
- Claude Code CLI v2.1.0+ installiert

## Installationsmethoden

### Methode 1: Plugin-Installation (Empfohlen)

#### 1. Plugin-Marktplatz hinzufügen

```bash
/plugin marketplace add https://github.com/affaan-m/ECC
```

#### 2. Plugin installieren

```bash
/plugin install ecc@ecc
```

#### 3. Rules installieren (Plugins installieren keine automatisch)

```bash
# Repository klonen
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Regeln in Benutzerverzeichnis kopieren
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
# Deinen Tech-Stack wählen
cp -r rules/typescript ~/.claude/rules/ecc/   # oder python, golang, swift, php
```

### Methode 2: Manuelle Installation

Wenn du eine feinere Kontrolle über die Installation möchtest:

```bash
# Repository klonen
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Abhängigkeiten installieren
npm install        # oder pnpm install | yarn install | bun install

# Agents installieren
cp agents/*.md ~/.claude/agents/

# Skills installieren (primäre Workflow-Oberfläche)
mkdir -p ~/.claude/skills/ecc
cp -r .agents/skills/* ~/.claude/skills/ecc/

# Rules installieren
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
cp -r rules/typescript ~/.claude/rules/ecc/

# Commands installieren (optional, ECC migriert zu Skills)
mkdir -p ~/.claude/commands
cp commands/*.md ~/.claude/commands/
```

## Installation verifizieren

```bash
# Prüfen ob Plugin erfolgreich installiert wurde
/plugin list ecc@ecc

# Verfügbare Befehle prüfen
/ecc:plan
/ecc:code-review
```

## Basiskonfiguration

### Paketmanager einstellen

ECC erkennt automatisch deinen bevorzugten Paketmanager, du kannst ihn auch manuell einstellen:

```bash
# Umgebungsvariable
export CLAUDE_PACKAGE_MANAGER=pnpm

# oder mit Befehl
/setup-pm
```

Priorität: Umgebungsvariable > Projektkonfiguration > package.json > lock-Datei > globale Konfiguration

### Hook-Laufzeitsteuerung

```bash
# Hook-Strenge (Standard: standard)
export ECC_HOOK_PROFILE=standard

# Bestimmte Hooks deaktivieren
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# SessionStart-Kontextlimit (Standard: 8000 Zeichen)
export ECC_SESSION_START_MAX_CHARS=4000
```

### Modelleinstellungen

Empfohlen in `~/.claude/settings.json` einstellen:

```json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "50"
  }
}
```

## Nächste Schritte

- [Grundbefehle](./03-Grundbefehle.md) - Häufige Befehle lernen
- [Zurück zum Einsteiger-Verzeichnis](../README.md)