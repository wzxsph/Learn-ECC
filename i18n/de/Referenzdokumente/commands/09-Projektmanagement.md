# Projektmanagement-Befehle

## Ueberblick

Projektmanagement-Befehle werden fuer Projektkonfiguration, Infrastruktureinrichtung und Toolchain-Management verwendet.

## Befehlsliste

### /projects

**Zweck**: Bekannte Projekte auflisten + Instinct-Statistiken

**Beschreibung**: Listet alle bekannten Projekte des Benutzers mit zugehoerigen Instinct-Statistiken auf.

---

### /harness-audit

**Zweck**: Agent harness-Konfiguration auditieren

**Beschreibung**: Prueft und auditiert die Claude Code harness-Konfiguration.

**Prueft**:
- Toolchain-Konfiguration
- Agent-Einstellungen
- MCP-Server-Status
- Regelkonfiguration

---

### /model-route

**Zweck**: Aufgaben an geeignetes Modell weiterleiten

**Beschreibung**: Waehlt das am besten geeignete Modell basierend auf Aufgabentyp (Haiku/Sonnet/Opus).

**Modellauswahl-Leitfaden**:
- **Haiku**: Leichtgewichtige Aufgaben, Codegenerierung, haeufige Aufrufe
- **Sonnet**: Hauptentwicklungsarbeit, komplexe Coding-Aufgaben
- **Opus**: Architekturentscheidungen, tiefes Reasoning, Recherche und Analyse

---

### /pm2

**Zweck**: PM2-Prozessmanager-Initialisierung

**Beschreibung**: Initialisiert PM2-Prozessmanager-Konfiguration.

---

### /setup-pm

**Zweck**: Paketmanager konfigurieren

**Beschreibung**: Konfiguriert den vom Projekt verwendeten Paketmanager.

**Unterstuetzte Paketmanager**:
- npm
- pnpm
- yarn
- bun

**Kann ueber** `CLAUDE_PACKAGE_MANAGER` **Umgebungsvariable konfiguriert werden**

---

### /project-init

**Zweck**: Projektinitialisierung

**Beschreibung**: Initialisiert Standardstruktur und -konfiguration fuer ein neues Projekt.

---

## Zugehoerige Befehle

- `/projects` - Projekte auflisten
- `/harness-audit` - Konfiguration auditieren
- `/model-route` - Modell-Routing