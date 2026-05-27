# Werkzeug-Skripte

Dieses Dokument beschreibt die Haupt-Werkzeug-Skripte im ECC (Everything Claude Code) Projekt.

## Inhaltsverzeichnis

- [ecc.js](#eccjs) - Haupt-CLI-Eingang
- [install-apply.js](#install-applyjs) - Installations-Ausfuehrer
- [claw.js](#clawjs) - NanoClaw REPL
- [harness-audit.js](#harness-auditjs) - Toolchain-Audit
- [catalog.js](#catalogjs) - Komponenten-Katalog-Betrachter
- [build-opencode.js](#build-opencodejs) - OpenCode-Build

---

## ecc.js

**Pfad**: `scripts/ecc.js`

ECC CLI-Haupteingang fuer selektive Installation, dispatched einheitlich alle Unterbefehle.

### Befehlsliste

| Befehl | Skript | Beschreibung |
|------|------|------|
| `install` | install-apply.js | ECC-Inhalt an Ziel installieren |
| `plan` | install-plan.js | Installationsmanifest und -plan pruefen |
| `catalog` | catalog.js | Installationskonfigurationsdateien und Komponenten-IDs entdecken |
| `consult` | consult.js | Komponenten nach natuerlicher Sprachabfrage empfehlen |
| `list-installed` | list-installed.js | Installationsstatus des aktuellen Kontexts pruefen |
| `doctor` | doctor.js | Fehlende oder driftende ECC-Verwaltungsdateien diagnostizieren |
| `repair` | repair.js | Verdahte oder fehlende ECC-Verwaltungsdateien wiederherstellen |
| `auto-update` | auto-update.js | Neueste ECC-Aenderungen pullen und neu installieren |
| `status` | status.js | SQLite-Status-Speicher-Zusammenfassung abfragen |
| `platform-audit` | platform-audit.js | GitHub-Warteschlangen, Diskussionen, Roadmap usw. auditieren |
| `security-ioc-scan` | ci/scan-supply-chain-iocs.js | Supply-Chain-IOCs in Abhaengigkeiten und AI-Tool-Persistenz scannen |
| `sessions` | sessions-cli.js | Sitzungen auflisten oder SQLite-Status-Speicher pruefen |
| `work-items` | work-items.js | Verknuepfte Linear/GitHub-Arbeitselemente verfolgen |
| `session-inspect` | session-inspect.js | Normale Sitzungssnapshots von dmux oder Claude-History-Zielen ausgeben |
| `loop-status` | loop-status.js | Veraltete Loop-Wakeups und ausstehende Werkzeugergebnisse in Claude-Skripten pruefen |
| `uninstall` | uninstall.js | ECC-Verwaltungsdateien loeschen die in install-state aufgezeichnet sind |

### Verwendungsbeispiele

```bash
# Direkte Sprachinstallation
ecc typescript

# Installation mit Konfigurationsdatei
ecc install --profile developer --target claude

# Installationsplan anzeigen
ecc plan --profile core --target cursor

# Verfuegbare Komponenten auflisten
ecc catalog profiles
ecc catalog components --family language

# Komponentendetails anzeigen
ecc catalog show framework:nextjs

# Empfehlungen consultieren
ecc consult "security reviews"

# Installierte pruefen
ecc list-installed --json

# Probleme diagnostizieren
ecc doctor --target cursor
ecc repair --dry-run

# Automatisch aktualisieren
ecc auto-update --dry-run

# Status abfragen
ecc status --json
ecc status --exit-code
ecc status --markdown --write status.md

# Plattform-Audit
ecc platform-audit --json --allow-untracked docs/drafts/

# Sicherheitsscan
ecc security-ioc-scan --home

# Sitzungsverwaltung
ecc sessions
ecc sessions session-active --json

# Arbeitselement-Verfolgung
ecc work-items upsert linear-ecc-20 --source linear --source-id ECC-20 --title "Review control-plane contract" --status blocked
ecc work-items sync-github --repo affaan-m/ECC

# Loop-Status pruefen
ecc loop-status --json

# Deinstallieren
ecc uninstall --target antigravity --dry-run
```

---

## install-apply.js

**Pfad**: `scripts/install-apply.js`

ECC-Installationsruntime, haelt traditionelle Spracheingangspunkte intakt waehrend ziel-spezifische Aenderungslogik in testbaren Node-Code verschoben wird.

### Unterstuetzte Ziele

| Ziel | Beschreibung |
|------|--------------|
| `claude` | In ~/.claude/ installieren, Regeln/Faehigkeiten in rules/ecc und skills/ecc |
| `claude-project` | In ./.claude/ installieren (projektweise) |
| `cursor` | Regeln, Hooks, Cursor-Konfiguration in ./.cursor/ installieren |
| `antigravity` | Regeln, Workflows, Faehigkeiten, Agents in ./.agent/ installieren |
| `codex` | Gemeinsame Agents/Konfiguration in ~/.codex/ installieren |
| `gemini` | Projektweise Gemini-Konfiguration in ./.gemini/ installieren |
| `opencode` | Gemeinsame Befehle/Hooks/Konfiguration in ~/.opencode/ installieren |
| `codebuddy` | Befehle, Agents, Faehigkeiten in ./.codebuddy/ installieren |
| `joycode` | Befehle, Agents, Faehigkeiten in ./.joycode/ installieren |
| `qwen` | In ~/.qwen/ installieren |
| `zed` | In ./.zed/ installieren |

### Installationsmodi

1. **Traditioneller Sprachmodus**: `install.sh <language>` (z.B. `ecc-install typescript`)
2. **Profile-Modus**: `install.sh --profile <name> [--with <component>]... [--without <component>]...`
3. **Modul-Modus**: `install.sh --modules <id,id,...>`
4. **Faehigkeiten-Modus**: `install.sh --skills <skill-id[,skill-id...]>`
5. **Lokalisierungs-Modus**: `install.sh --target claude|claude-project --locale <locale-code>`

### Optionen

- `--dry-run` - Installationsplan anzeigen aber keine Dateien kopieren
- `--json` - Maschinenlesbares JSON-Format ausgeben
- `--config <path>` - Installationsabsicht aus Datei laden

---

## claw.js

**Pfad**: `scripts/claw.js`

NanoClaw v2 - Zero-externe-Abhaengigkeiten, sitzungsbewusster REPL fuer Everything Claude Code, gebaut auf `claude -p`.

### Funktionen

- Sitzungshistorie-Persistenz gespeichert in `~/.claude/claw/`
- Dynamisches Laden von Faehigkeiten als Kontext
- Sitzungskomprimierung zur Verwaltung der Kontextlaenge
- Sitzungs-Verzweigung und Export
- Cross-Session-Suche

### REPL-Befehle

| Befehl | Beschreibung |
|------|------|
| `/help` | Hilfe anzeigen |
| `/clear` | Aktuelle Sitzungshistorie loeschen |
| `/history` | Vollstaendigen Konversationsverlauf drucken |
| `/sessions` | Gespeicherte Sitzungen auflisten |
| `/model [name]` | Modell anzeigen/setzen |
| `/load <skill-name>` | Faehigkeit in aktiven Kontext laden |
| `/branch <session-name>` | Aktuelle Sitzung zu neuer Sitzung verzweigen |
| `/search <query>` | Cross-Session suchen |
| `/compact` | Aktuelle Turns behalten, alten Kontext komprimieren |
| `/export <md\|json\|txt> [path]` | Sitzung exportieren |
| `/metrics` | Sitzungsmetriken anzeigen |
| `exit` | REPL beenden |

### Umgebungsvariablen

| Variable | Standardwert | Beschreibung |
|------|--------|------|
| `CLAW_SESSION` | `default` | Anfangs-Sitzungsname |
| `CLAW_MODEL` | `sonnet` | Standardmodell |
| `CLAW_SKILLS` | leer | Geladene Faehigkeitenliste (kommagetrennt) |

### Verwendungsbeispiele

```bash
# Mit Standard-Sitzung starten
node scripts/claw.js

# Mit angegebener Sitzung starten
CLAW_SESSION=my-session node scripts/claw.js

# Mit geladenen Faehigkeiten starten
CLAW_SKILLS="javascript-patterns,python-testing" node scripts/claw.js
```

---

## harness-audit.js

**Pfad**: `scripts/harness-audit.js`

Deterministisches Toolchain-Audit-Werkzeug basierend auf expliziter Datei-/Regelpruefung. Automatisch ECC-Repo-Modus vs. Verbraucher-Projekt-Modus erkennen.

### Audit-Kategorien

| Kategorie | Beschreibung |
|------|------|
| Tool Coverage | Werkzeugabdeckungs-Vollstaendigkeit |
| Context Efficiency | Kontexteffizienz |
| Quality Gates | Qualitaets-Gates |
| Memory Persistence | Erinnerungs-Persistenz |
| Eval Coverage | Eval-Abdeckung |
| Security Guardrails | Sicherheits-Schutzrails |
| Cost Efficiency | Kosten-Effizienz |
| GitHub Integration | GitHub-Integration |
| Vercel/Netlify/Cloudflare/Fly Integration | Plattform-Integrationen |

### Verwendungsbeispiele

```bash
# Aktuelles Verzeichnis auditieren
node scripts/harness-audit.js

# Angegebenen Bereich
node scripts/harness-audit.js --scope repo
node scripts/harness-audit.js --scope hooks
node scripts/harness-audit.js --scope skills

# Angegebenes Root-Verzeichnis
node scripts/harness-audit.js --root /path/to/project

# JSON-Format-Ausgabe
node scripts/harness-audit.js --format json

# Kombiniert
node scripts/harness-audit.js repo --format json --root .
```

---

## catalog.js

**Pfad**: `scripts/catalog.js`

ECC-Installationskomponenten und Konfigurationsdateien-Entdeckungswerkzeug.

### Befehle

#### profiles

Liste aller Installationsprofile.

```bash
node scripts/catalog.js profiles
node scripts/catalog.js profiles --json
```

#### components

Installationskomponenten auflisten, nach Familie und Ziel filterbar.

```bash
node scripts/catalog.js components
node scripts/catalog.js components --family language
node scripts/catalog.js components --family framework
node scripts/catalog.js components --target claude
node scripts/catalog.js components --json
```

#### show

Details einer bestimmten Komponente anzeigen.

```bash
node scripts/catalog.js show <component-id>
node scripts/catalog.js show framework:nextjs --json
```

### Komponenten-Familien

- `baseline` - Basiskomponenten
- `language` - Sprachkomponenten (javascript, typescript, python, usw.)
- `framework` - Frameworkkomponenten (nextjs, react, vue, usw.)
- `capability` - Faehigkeitskomponenten
- `agent` - Agentkomponenten
- `skill` - Faehigkeitskomponenten

---

## build-opencode.js

**Pfad**: `scripts/build-opencode.js`

Baut TypeScript-Code des OpenCode-Plugins.

### Funktionen

1. `dist`-Verzeichnis bereinigen
2. TypeScript-Code im `.opencode`-Verzeichnis mit TypeScript-Compiler kompilieren
3. Ausgabe nach `.opencode/dist`

### Voraussetzungen

Erfordert installierte Development-Abhaengigkeiten im Root, TypeScript-Compiler muss verfuegbar sein.

### Verwendung

```bash
node scripts/build-opencode.js
```