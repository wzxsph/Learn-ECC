# Hook-Typen

## Ueberblick

Das ECC Hooks-System ist ein ereignisgesteuerter Automatisierungsmechanismus, der vor und nach der Claude Code-Werkzeugausfuehrung ausgeloest wird, um Codequalitaet zu erzwingen, Frueherkennung von Fehlern und Automatisierung wiederholender Pruefungen zu ermoeglichen.

```
Benutzeranfrage → Claude waehlt Werkzeug → PreToolUse-Hooks ausfuehren → Werkzeugausfuehrung → PostToolUse-Hooks ausfuehren
```

## Detaillierte Hook-Typen

### 1. PreToolUse-Hooks

#### Typbeschreibung
PreToolUse-Hooks werden VOR der Werkzeugausfuehrung ausgefuehrt und koennen blockieren (exit code 2) oder warnen (stderr-Ausgabe aber nicht blockieren).

#### Ausloesezeitpunkt
- Vor der Ausfuehrung eines beliebigen Werkzeugs (Edit, Write, Bash, Read usw.)
- Gilt fuer Pre-Checks aller Werkzeugoperationen

#### Parameterbeschreibung
```typescript
interface HookInput {
  tool_name: string;          // Werkzeugname: "Bash", "Edit", "Write", "Read" usw.
  tool_input: {
    command?: string;         // Bash: auszufuehrender Befehl
    file_path?: string;        // Edit/Write/Read: Zieldateipfad
    old_string?: string;       // Edit: Ersetzter Text
    new_string?: string;       // Edit: Ersetzungstext
    content?: string;          // Write: Dateiinhalt
  };
}
```

#### Exit-Codes
| Exit-Code | Bedeutung | Verhalten |
|--------|------|------|
| 0 | Erfolg | Fortfahren, keine Warnung ausgeben |
| 2 | Blockieren | Werkzeugausfuehrung blockieren |
| Andere nicht-null | Fehler | Loggen aber nicht blockieren |

#### Verwendungsbeispiele

**Dev-Server-Blocker (verhindert das Ausfuehren von npm run dev ausserhalb von tmux):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "Verhindert das Ausfuehren von Entwicklungsservern ausserhalb von tmux"
}
```

**Dokumentationsdatei-Warnung (warnt vor nicht-Standard-.md-Dateien):**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "Warnt vor Erstellung nicht-standardmaessiger Dokumentationsdateien"
}
```

#### Hinweise
- PreToolUse ist ein **blockierender** Hook, muss schnell ausgefuehrt werden (<200ms)
- Keine Netzwerkaufrufe in PreToolUse-Hooks durchfuehren
- Blockieroperationen (exit 2) sparsam verwenden, nur fuer kritische Sicherheitspruefungen

---

### 2. PostToolUse-Hooks

#### Typbeschreibung
PostToolUse-Hooks werden NACH der Werkzeugausfuehrung ausgefuehrt, koennen Ausgaben analysieren aber die Werkzeugausfuehrung nicht blockieren.

#### Ausloesezeitpunkt
- Nach Abschluss eines beliebigen Werkzeugs
- Kann auf Werkzeugausgabeergebnisse zugreifen

#### Parameterbeschreibung
```typescript
interface HookInput {
  tool_name: string;          // Werkzeugname
  tool_input: {               // Wie bei PreToolUse
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // Nur bei PostToolUse verfuegbar
    output?: string;          // Befehl/Werkzeugausgabe
  };
}
```

#### Exit-Codes
| Exit-Code | Bedeutung |
|--------|------|
| 0 | Erfolg, fortfahren |
| Nicht-null | Fehler, loggen aber Werkzeugausfuehrung nicht blockieren |

#### Verwendungsbeispiele

**PR-Logging (protokolliert PR-URL nach gh pr create):**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "URL und Review-Befehl nach PR-Erstellung protokollieren"
}
```

**Qualitaets-Gate-Pruefung (fuehrt Qualitaetspruefungen nach Bearbeitung aus):**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "Qualitaets-Gate-Pruefung nach Dateibearbeitung ausfuehren"
}
```

#### Hinweise
- PostToolUse-Hooks koennen Werkzeugausfuehrung NICHT blockieren
- Können als asynchron gesetzt werden (async: true) um nicht zu blockieren
- Asynchrone Hooks sollten innerhalb von 30 Sekunden abschliessen

---

### 3. Stop-Hooks

#### Typbeschreibung
Stop-Hooks werden nach jeder Claude-Antwort ausgefuehrt, fuer Batch-Verarbeitung, abschliessende Pruefungen und Sitzungsstatus-Persistierung.

#### Ausloesezeitpunkt
- Nach jedem Benutzeranfragenabschluss und Claude-Antwortgenerierung
- Tritt nach der letzten Antwort einer Sitzung auf

#### Parameterbeschreibung
Stop-Hooks empfangen dieselbe Eingabe wie PostToolUse, einschliesslich vollstaendiger Werkzeugaufruf-Historie.

#### Verwendungsbeispiele

**Console-Log-Pruefung (prueft console.log in modifizierten Dateien nach Antwort):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "Prueft ob console.log in modifizierten Dateien vorhanden ist"
}
```

**Batch-Formatierung und Typpruefung:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "Batch-Formatierung aller bearbeiteten JS/TS-Dateien und Typpruefung"
}
```

**Sitzungszusammenfassung-Persistierung:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "Sitzungszustand speichern"
}
```

#### Hinweise
- Stop-Hooks laufen nach jeder Antwort, geeignet fuer Batch-Operationen
- Können async-Modus fuer nicht-blockierende Operationen verwenden
- Formatierung/Typpruefung eignen sich fuer Stopzeit-Batch-Ausfuehrung

---

### 4. SessionStart-Hooks

#### Typbeschreibung
SessionStart-Hooks werden beim Start des Sitzungslebenszyklus ausgeloest, fuer das Laden vorheriger Kontexte und die Erkennung des Projektstatus.

#### Ausloesezeitpunkt
- Wenn neue Sitzung startet
- Wenn Claude Code startet oder neue Sitzung erstellt

#### Verwendungsbeispiele

**Vorherigen Kontext laden und Paketmanager erkennen:**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "Begrenzten vorherigen Kontext laden und Projektstatus bei Sitzungsstart erkennen",
  "blocking": false
}
```

#### Hinweise
- Lebenszyklus-Hooks, müssen schnell abgeschlossen werden (<30 Sekunden)
- Können ueber `ECC_SESSION_START_MAX_CHARS` Umgebungsvariable die zusätzliche Kontextgroesse steuern
- Können mit `ECC_SESSION_START_CONTEXT=off` vollstaendig deaktivieren

---

### 5. SessionEnd-Hooks

#### Typbeschreibung
SessionEnd-Hooks werden beim Ende einer Sitzung ausgeloest, fuer die Persistierung von Sitzungsende-Zusammenfassungen und Bereinigung.

#### Ausloesezeitpunkt
- Wenn Sitzung endet
- Wenn Sitzungs-transcript-Metadaten verfuegbar sind

#### Verwendungsbeispiele

**Sitzungsende-Marker:**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "Persistiert Sitzungsende-Zusammenfassung wenn Transcript-Metadaten verfuegbar sind",
  "blocking": false
}
```

#### Hinweise
- Hauptsaechlich asynchrone Ausfuehrung, sollte Sitzungsende nicht blockieren
- Lebenszyklus-Marker fuer指标收集和清理

---

### 6. PreCompact-Hooks

#### Typbeschreibung
PreCompact-Hooks werden VOR der Kontextkomprimierung ausgeloest, fuer die Zustandsspeicherung.

#### Ausloesezeitpunkt
- Vor der Kontextkomprimierung/-reduzierung
- Wenn Kontextfenster fast voll ist

#### Verwendungsbeispiele

**Zustand vor Kontextkomprimierung speichern:**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "Persistiert Sitzungszustand vor Kontextkomprimierung",
  "blocking": false
}
```

#### Hinweise
- Eignen sich zum Speichern des Zustands laengerer Aufgaben
- Non-blocking, Komprimierungsprozess nicht beeinflussen

---

### 7. PostToolUseFailure-Hooks

#### Typbeschreibung
PostToolUseFailure-Hooks werden nach einem Werkzeugausfuehrungsfehler ausgeloest.

#### Ausloesezeitpunkt
- Nach einem beliebigen fehlgeschlagenen Werkzeugaufruf

#### Verwendungsbeispiele

**MCP-Gesundheitspruefung (verfolgt fehlgeschlagene MCP-Aufrufe):**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "Verfolgt fehlgeschlagene MCP-Toolaufrufe, markiert ungesunde Server und versucht Wiederverbindung"
}
```

#### Hinweise
- Können für Wiederholungslogik oder Gesundheitsstatusverfolgung verwendet werden
- Ändert nicht den Status des Werkzeugfehlers

---

## Hook-Typ-Vergleichstabelle

| Typ | Ausloesezeitpunkt | Kann blockieren | Kann blockieren | Typische Verwendung |
|------|----------|----------|----------|----------|
| PreToolUse | Vor Werkzeugausfuehrung | Ja (exit 2) | Ja (synchron) | Qualitaetspruefungen, Sicherheitsvalidierung |
| PostToolUse | Nach Werkzeugausfuehrung | Nein | Optional (async) | Logging, Analysen |
| Stop | Nach jeder Antwort | Nein | Optional (async) | Batch-Formatierung, Sitzungszusammenfassung |
| SessionStart | Sitzungsstart | Nein | Nein | Kontext laden, Projekt erkennen |
| SessionEnd | Sitzungsende | Nein | Nein | Zusammenfassung persistent machen, bereinigen |
| PreCompact | Vor Komprimierung | Nein | Nein | Zustand speichern |
| PostToolUseFailure | Nach Werkzeugfehler | Nein | Nein | Gesundheitspruefungen, Wiederholung |

---

## Umgebungsvariablen-Steuerung

Hook-Verhalten kann ueber Umgebungsvariablen gesteuert werden:

```bash
# minimal | standard | strict (Standard: standard)
export ECC_HOOK_PROFILE=standard

# Bestimmte Hook-IDs deaktivieren (kommagetrennt)
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# GateGuard deaktivieren
export ECC_GATEGUARD=off

# Groesse der SessionStart-Zusatzkontexts steuern (Standard: 8000 Zeichen)
export ECC_SESSION_START_MAX_CHARS=4000

# SessionStart-Zusatzkontext vollstaendig deaktivieren
export ECC_SESSION_START_CONTEXT=off
```