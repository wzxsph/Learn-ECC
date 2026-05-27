# Konfigurationsformat

## hooks.json-Strukturuebersicht

Die ECC Hooks-System-Konfigurationsdatei verwendet JSON-Format mit folgender Hauptstruktur:

```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "hooks": {
    "PreToolUse": [...],
    "PostToolUse": [...],
    "Stop": [...],
    "SessionStart": [...],
    "SessionEnd": [...],
    "PreCompact": [...],
    "PostToolUseFailure": [...]
  }
}
```

## Top-Level-Struktur

| Feld | Typ | Beschreibung |
|------|------|------|
| `$schema` | string | JSON Schema URL zur Konfigurationsvalidierung |
| `hooks` | object | Objekt das alle Hook-Typen enthaelt |

---

## Hook-Array-Struktur

Jeder Event-Typ (PreToolUse, PostToolUse usw.) enthaelt ein Hook-Array, wobei jedes Hook-Objekt folgende Struktur hat:

```json
{
  "matcher": "Bash|Edit|Write",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/my-hook.js",
      "async": false,
      "timeout": 30
    }
  ],
  "description": "Hook-Beschreibung",
  "id": "unique-hook-id"
}
```

### matcher-Feld

#### Typbeschreibung
Das matcher-Feld gibt an fuer welche Werkzeugtypen der Hook ausgeloest wird, unterstuetzt einzelne oder mehrere Werkzeuge (mit `|` getrennt).

#### Verfuegbare Werte
| matcher-Wert | Zugehoerige Werkzeuge |
|-----------|-----------------|
| `*` | Alle Werkzeuge |
| `Bash` | Bash-Werkzeug |
| `Edit` | Edit-Werkzeug |
| `Write` | Write-Werkzeug |
| `Read` | Read-Werkzeug |
| `MultiEdit` | MultiEdit-Werkzeug |
| `Bash\|Edit\|Write` | Bash, Edit oder Write |
| `Edit\|Write\|MultiEdit` | Edit, Write oder MultiEdit |

#### Verwendungsbeispiele
```json
// Alle Werkzeuge matchen
"matcher": "*"

// Nur Bash-Werkzeug matchen
"matcher": "Bash"

// Mehrere Werkzeuge matchen
"matcher": "Edit|Write|MultiEdit"
```

---

## hooks-Array-Struktur

Das hooks-Array enthaelt ein oder mehrere Hook-Befehlsobjekte:

### type-Feld

| type-Wert | Beschreibung |
|--------|------|
| `command` | Node.js-Befehl ausfuehren |

### command-Feld

Der auszufuehrende Befehl, typischerweise ein Node.js-Skript-Pfad.

### async-Feld (optional)

```json
"async": true
```

- `true`: Asynchron ausfuehren, Werkzeugausfuehrung nicht blockieren
- `false` oder weggelassen: Synchron ausfuehren, Werkzeugausfuehrung blockieren

### timeout-Feld (optional)

```json
"timeout": 30
```

Maximale Ausfuehrungszeit (Sekunden). Empfohlen:
- Synchrone Hooks: <200ms
- Asynchrone Hooks: <=30 Sekunden

---

## Vollstaendige Konfigurationsbeispiele

### PreToolUse Hook-Beispiel

```json
{
  "matcher": "Bash",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/pre-bash-dispatcher.js"
    }
  ],
  "description": "Bash pre-check Dispatcher fuer Qualitaet, tmux, Push und GateGuard-Pruefungen",
  "id": "pre:bash:dispatcher"
}
```

### PostToolUse Hook-Beispiel

```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/run-with-flags.js post:quality-gate scripts/hooks/quality-gate.js standard,strict",
      "async": true,
      "timeout": 30
    }
  ],
  "description": "Qualitaets-Gate-Pruefung nach Dateibearbeitung ausfuehren",
  "id": "post:quality-gate"
}
```

### Stop Hook-Beispiel

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/run-with-flags.js stop:session-end scripts/hooks/session-end.js minimal,standard,strict",
      "async": true,
      "timeout": 10
    }
  ],
  "description": "Sitzungszustand nach jeder Antwort speichern",
  "id": "stop:session-end"
}
```

### SessionStart Hook-Beispiel

```json
{
  "matcher": "*",
  "hooks": [
    {
      "type": "command",
      "command": "node scripts/hooks/session-start-bootstrap.js"
    }
  ],
  "description": "Vorherigen Kontext laden und Paketmanager neuer Sitzungen erkennen",
  "id": "session:start"
}
```

---

## Lebenszyklus-Event-Konfigurationsformat

Fuer SessionStart, SessionEnd, PreCompact und aehnliche Lebenszyklus-Events eine andere Konfigurationsstruktur verwenden:

```json
{
  "description": "Lebenszyklus-Hook-Definition fuer Memory-Persistenz",
  "events": [
    {
      "event": "SessionStart",
      "id": "session:start",
      "script": "scripts/hooks/session-start-bootstrap.js",
      "purpose": "Begrenzten vorherigen Kontext laden und Projektstatus bei Sitzungsstart erkennen",
      "blocking": false
    },
    {
      "event": "PreCompact",
      "id": "pre:compact",
      "script": "scripts/hooks/pre-compact.js",
      "purpose": "Sitzungszustand vor Kontextkomprimierung persistent machen",
      "blocking": false
    }
  ]
}
```

### Lebenszyklus-Event-Felder

| Feld | Beschreibung |
|------|--------------|
| `event` | Event-Typ (SessionStart, PreCompact, SessionEnd usw.) |
| `id` | Eindeutige Kennung |
| `script` | Skriptpfad |
| `purpose` | Zweckbeschreibung |
| `blocking` | Ob blockierend (normalerweise false) |

---

## Hooks deaktivieren

### Durch Bearbeiten der Konfiguration deaktivieren

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Override: Erlaube alle .md-Dateierstellung"
      }
    ]
  }
}
```

### Durch Umgebungsvariablen deaktivieren

```bash
# Bestimmte Hooks deaktivieren (kommagetrennt)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# GateGuard deaktivieren
export ECC_GATEGUARD=off
```

---

## Hook-ID-Benennungskonventionen

ECC verwendet Doppelpunkt-getrennte Benennungskonvention:

| Praefix | Verwendungszweck | Beispiel |
|------|------|------|
| `pre:` | PreToolUse-Hooks | `pre:bash:dispatcher` |
| `post:` | PostToolUse-Hooks | `post:quality-gate` |
| `stop:` | Stop-Hooks | `stop:session-end` |
| `session:` | Sitzungslebenszyklus-Hooks | `session:start` |

---

## run-with-flags.js Wrapper

ECC verwendet den `run-with-flags.js` Wrapper um Hooks auszufuehren, mit Runtime-Gating aus Konfigurationsdateien:

```
node scripts/hooks/run-with-flags.js <hook-id> <script-path> <profiles>
```

### Parameterbeschreibung
| Parameter | Beschreibung |
|------|------|
| `hook-id` | Eindeutige Hook-Kennung |
| `script-path` | Tatsaechlicher auszufuehrender Skriptpfad |
| `profiles` | Kommagetrennte Profiliste (minimal, standard, strict) |

### Funktionsweise
1. Liest `ECC_HOOK_PROFILE` Umgebungsvariable (Standard: standard)
2. Prueft ob Hook-ID in `ECC_DISABLED_HOOKS` ist
3. Wenn erlaubt, Skript ausfuehren

---

## Plattformuebergreifende Pfadbehandlung

ECC Hook-Befehle verwenden Node.js-Skripte fuer plattformuebergreifendes Verhalten:

```javascript
const path = require('path');
const homedir = require('os').homedir();

// Plattformuebergreifende Pfadaufloesung
const configDir = path.join(homedir, '.claude');
const hookScript = path.join(configDir, 'scripts', 'hooks', 'my-hook.js');
```

---

## Konfigurationsvalidierung

JSON Schema-Validierung fuer Konfiguration empfohlen:
```json
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json"
}
```