# Eingebaute Hooks

## Ueberblick

ECC Plugins stellen umfangreiche eingebaute Hook-Implementierungen bereit, located in `ECC/scripts/hooks/` directory.

## PreToolUse Eingebaute Hooks

### pre:bash:dispatcher

**Typ**: PreToolUse
**matcher**: Bash
**Beschreibung**: Bash pre-check Dispatcher, kombiniert Qualitaetspruefungen, tmux-Erinnerungen, git-push-Erinnerungen und GateGuard-Checks

**Funktionen**:
- Dev-Server-Blocker - Verhindert das Ausfuehren von `npm run dev` ausserhalb von tmux
- Tmux-Erinnerung - Empfehlung fuer tmux bei laenger laufenden Befehlen (npm test, cargo build, docker)
- Git-Push-Erinnerung - Erinnert daran Aenderungen vor `git push` zu reviewen
- Pre-commit-Qualitaetspruefung - Qualitaetspruefungen ausfuehren: lint gestagerte Dateien, Commit-Message-Format verifizieren, console.log/debugger/Schluessel erkennen

**Exit-Codes**:
- 0: Warnung aber fortsetzen
- 2: Ausfuehrung blockieren

---

### pre:write:doc-file-warning

**Typ**: PreToolUse
**matcher**: Write
**Beschreibung**: Warnung beim Erstellen nicht-standardmaessiger Dokumentationsdateien

**Funktionen**:
- Standarddateien erlauben: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- Verzeichnisse erlauben: docs/, skills/
- Bei anderen .md/.txt-Dateien warnen
- Plattformuebergreifende Pfadbehandlung

**Exit-Code**: 0 (nur Warnung)

---

### pre:edit-write:suggest-compact

**Typ**: PreToolUse
**matcher**: Edit|Write
**Beschreibung**: Empfehlung fuer manuelles `/compact` in logischen Intervallen (ca. alle 50 Tool-Aufrufe)

**Funktionen**:
- Tool-Aufruf-Anzahl verfolgen
- Bei angemessenen Intervallen an Context-Komprimierung erinnern
- Non-Blocking, nur Empfehlungen

**Exit-Code**: 0 (Empfehlung)

---

### pre:observe:continuous-learning

**Typ**: PreToolUse
**matcher**: *
**Beschreibung**: Werkzeugintention aufzeichnen um kontinuierliche Lernsignale zu unterstuetzen

**Funktionen**:
- Werkzeugnutzungsbeobachtungen erfassen
- Zur Musterextraktion und -analyse verwendet
- Asynchron ausgefuehrt, nicht blockierend

**Exit-Code**: 0

---

### pre:governance-capture

**Typ**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Beschreibung**: Governance-Events erfassen (Schluessel, Richtlinienverstoesze, Genehmigungsanfragen)

**Funktionen**:
- Secret/Schluessel-Leaks erkennen
- Richtlinienverstoesze erfassen
- Genehmigungsanfragen aufzeichnen

**Aktivierung**: `ECC_GOVERNANCE_CAPTURE=1`

**Exit-Code**: 0

---

### pre:config-protection

**Typ**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**Beschreibung**: Aenderungen an Linter/Formatter-Konfigurationsdateien blockieren

**Funktionen**:
- .eslintrc, .prettierrc usw. Aenderungen blockieren
- Auf Code-Reparatur statt auf Abschwaechung der Konfiguration hinweisen

**Exit-Code**: 0

---

### pre:mcp-health-check

**Typ**: PreToolUse
**matcher**: *
**Beschreibung**: MCP-Server-Gesundheitsstatus vor MCP-Tool-Ausfuehrung pruefen

**Funktionen**:
- MCP-Server-Status pruefen
- Aufrufe an ungesunde MCP-Server blockieren

**Exit-Code**: 0

---

### pre:edit-write:gateguard-fact-force

**Typ**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**Beschreibung**: Fact-Force GateGuard: Erste Bearbeitung/Schreiben jeder Datei blockieren, Untersuchung vor Erlaubnis erforderlich

**Funktionen**:
- Erste Bearbeitung blockieren
- Bestaetigung erforderlich machen: Import, Datenschema, Benutzeranweisung
- Sicherstellen dass Begruendung vorliegt bevor modifiziert wird

**Exit-Code**: 0

---

## PostToolUse Eingebaute Hooks

### post:bash:dispatcher

**Typ**: PostToolUse
**matcher**: Bash
**Beschreibung**: Bash post-check Dispatcher fuer Logging, PR- und Build-Benachrichtigungen

**Funktionen**:
- PR-Logging - PR-URL und Review-Befehl nach `gh pr create` protokollieren
- Build-Analyse - Hintergrundanalyse nach Build-Befehlen (asynchron, non-blocking)

**Exit-Code**: 0

---

### post:quality-gate

**Typ**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Beschreibung**: Schnelle Qualitaetspruefung nach Dateibearbeitung ausfuehren

**Funktionen**:
- Codequalitaetspruefungen
- Asynchron ausgefuehrt
- Bearbeitung nicht blockieren

**Exit-Code**: 0

---

### post:edit:design-quality-check

**Typ**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Beschreibung**: Warnung wenn Frontend-Bearbeitungen zu generischem Template-Look-UI abweichen

**Funktionen**:
- UI-Designqualitaet erkennen
- Zu generisches Template-Erscheinungsbild verhindern

**Exit-Code**: 0

---

### post:edit:accumulator

**Typ**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**Beschreibung**: Bearbeitete JS/TS-Dateipfade aufzeichnen fuer Stop-seitige Batch-Formatierung und Typpruefung

**Funktionen**:
- Liste bearbeiteter Dateien akkumulieren
- Fuer stop:format-typecheck verwendet

**Exit-Code**: 0

---

### post:edit:console-warn

**Typ**: PostToolUse
**matcher**: Edit
**Beschreibung**: console.log-Anweisungen nach Bearbeitung warnen

**Funktionen**:
- console.log im Code erkennen
- Entwickler warnen Debug-Code zu bereinigen

**Exit-Code**: 0

---

### post:governance-capture

**Typ**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**Beschreibung**: Governance-Events aus Werkzeugausgaben erfassen

**Funktionen**:
- Werkzeugausgaben analysieren
- Potenzielle Probleme erkennen

**Aktivierung**: `ECC_GOVERNANCE_CAPTURE=1`

**Exit-Code**: 0

---

### post:session-activity-tracker

**Typ**: PostToolUse
**matcher**: *
**Beschreibung**: Tool-Aufrufe und Dateiaktivitaeten pro Session aufzeichnen

**Funktionen**:
- Werkzeugnutzungsstatistik verfolgen
- Dateiaktivitaeten aufzeichnen
- Fuer ECC2-Metriken verwendet

**Exit-Code**: 0

---

### post:observe:continuous-learning

**Typ**: PostToolUse
**matcher**: *
**Beschreibung**: Werkzeugergebnisse aufzeichnen um kontinuierliche Lernsignale zu unterstuetzen

**Funktionen**:
- Werkzeugausfuehrungsergebnisse erfassen
- Musteranalyse unterstuetzen

**Exit-Code**: 0

---

### post:ecc-metrics-bridge

**Typ**: PostToolUse
**matcher**: *
**Beschreibung**: Laufende Session-Metrik-Aggregation pflegen

**Funktionen**:
- Token- und Kostenmetriken verfolgen
- Fuer Statusleiste und Kontextmonitor verwendet

**Exit-Code**: 0

---

### post:ecc-context-monitor

**Typ**: PostToolUse
**matcher**: *
**Beschreibung**: Proxy-Warnungen injizieren wenn Kontext erschöpft, hohe Kosten, Scope Creep oder Tool-Schleifen auftreten

**Funktionen**:
- Kontextnutzung monitoren
- Kostenwarnungen
- Scope Creep-Erkennung
- Tool-Schleifen-Erkennung

**Exit-Code**: 0

---

### post:mcp-health-check

**Typ**: PostToolUseFailure
**matcher**: *
**Beschreibung**: Fehlgeschlagene MCP-Tool-Aufrufe verfolgen, ungesunde Server markieren und Wiederverbindung versuchen

**Funktionen**:
- Fehlerverfolgung
- Gesundheitsstatus-Markierung
- Automatische Wiederverbindungsversuche

**Exit-Code**: 0

---

## Stop Eingebaute Hooks

### stop:format-typecheck

**Typ**: Stop
**matcher**: *
**Beschreibung**: Batch-Formatierung (Biome/Prettier) und Typpruefung (tsc) fuer alle JS/TS-Dateien die in dieser Antwort bearbeitet wurden

**Funktionen**:
- Prettier/Biome-Formatierung
- TypeScript-Typpruefung
- Bei Stop einmal ausfuehren, nicht nach jeder Bearbeitung

**Exit-Code**: 0

**Timeout**: 300 Sekunden

---

### stop:check-console-log

**Typ**: Stop
**matcher**: *
**Beschreibung**: Nach jeder Antwort console.log in modifizierten Dateien pruefen

**Funktionen**:
- Modifizierte Dateien scannen
- console.log-Verwendung melden

**Exit-Code**: 0

---

### stop:session-end

**Typ**: Stop
**matcher**: *
**Beschreibung**: Session-Status nach jeder Antwort persistent machen (Stop traegt transcript_path)

**Funktionen**:
- Session-Status speichern
- Kontextpersistenz

**Exit-Code**: 0

---

### stop:evaluate-session

**Typ**: Stop
**matcher**: *
**Beschreibung**: Session evaluieren um lernbare Muster zu extrahieren

**Funktionen**:
- Mustererkennung
- Kontinuierliche Lernunterstuetzung
- Asynchron ausgefuehrt

**Exit-Code**: 0

---

### stop:cost-tracker

**Typ**: Stop
**matcher**: *
**Beschreibung**: Token- und Kostenmetriken pro Session verfolgen

**Funktionen**:
- Token-Nutzungsstatistik
- Kostenabschaetzung
- Leichtgewichtige Runtime-Kostentelemetrie-Markierung

**Exit-Code**: 0

---

### stop:desktop-notify

**Typ**: Stop
**matcher**: *
**Beschreibung**: macOS/WSL-Desktop-Benachrichtigungen mit Aufgabenzusammenfassung beim Claude-Response senden

**Funktionen**:
- Desktop-Benachrichtigungen
- Aufgabenzusammenfassung anzeigen

**Exit-Code**: 0

---

## SessionStart Eingebaute Hooks

### session:start

**Typ**: SessionStart
**matcher**: *
**Beschreibung**: Vorherigen Kontext laden und Paketmanager neuer Sessions erkennen

**Funktionen**:
- Begrenzten vorherigen Kontext laden
- Projektstatus erkennen
- Paketmanager erkennen (npm/pnpm/yarn/bun)

**Umgebungsvariablen**:
- `ECC_SESSION_START_MAX_CHARS`: Steuerung der zusaetzlichen Kontextgroesse (standard 8000 Zeichen)
- `ECC_SESSION_START_CONTEXT=off`: Zusaetzlichen Kontext vollstaendig deaktivieren

**Exit-Code**: 0

---

## PreCompact Eingebaute Hooks

### pre:compact

**Typ**: PreCompact
**matcher**: *
**Beschreibung**: Status vor Kontextkomprimierung speichern

**Funktionen**:
- Session-Status-Persistierung
- Kontext fuer Komprimierung vorbereiten

**Exit-Code**: 0

---

## SessionEnd Eingebaute Hooks

### session:end:marker

**Typ**: SessionEnd
**matcher**: *
**Beschreibung**: Session-End-Lebenszyklusmarker

**Funktionen**:
- Lebenszyklus-Ereignismarkierung
- Cleanup-Logging

**Exit-Code**: 0

---

## Eingebaute Hooks Spickzettel

### PreToolUse Hooks

| ID | Matcher | Funktion | Blockierend |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | Qualitaet/tmux/push/GateGuard-Pruefungen | Kann blockieren |
| pre:write:doc-file-warning | Write | Dokumentationsdatei-Warnung | Nein |
| pre:edit-write:suggest-compact | Edit|Write | Komprimierung empfehlen | Nein |
| pre:observe:continuous-learning | * | Kontinuierliches Lernen beobachten | Nein |
| pre:governance-capture | Bash|Write|Edit|MultiEdit | Governance-Event-Erfassung | Nein |
| pre:config-protection | Write|Edit|MultiEdit | Konfigurationsschutz | Nein |
| pre:mcp-health-check | * | MCP-Gesundheitspruefung | Kann blockieren |
| pre:edit-write:gateguard-fact-force | Edit|Write|MultiEdit | Erste Bearbeitung GateGuard | Kann blockieren |

### PostToolUse Hooks

| ID | Matcher | Funktion | Async |
|----|---------|------|------|
| post:bash:dispatcher | Bash | PR-Logging/Build-Benachrichtigungen | Ja |
| post:quality-gate | Edit|Write|MultiEdit | Qualitaets-Gate-Pruefung | Ja |
| post:edit:design-quality-check | Edit|Write|MultiEdit | Design-Qualitaetspruefung | Nein |
| post:edit:accumulator | Edit|Write|MultiEdit | Bearbeitungs-Akkumulator | Nein |
| post:edit:console-warn | Edit | console.log-Warnung | Nein |
| post:governance-capture | Bash|Write|Edit|MultiEdit | Governance-Event-Erfassung | Nein |
| post:session-activity-tracker | * | Session-Aktivitaetsverfolgung | Nein |
| post:observe:continuous-learning | * | Kontinuierliches Lernen beobachten | Ja |
| post:ecc-metrics-bridge | * | Metrik-Bridge | Nein |
| post:ecc-context-monitor | * | Kontext-Monitor | Nein |
| post:mcp-health-check | * (PostToolUseFailure) | MCP-Gesundheitspruefung | Nein |

### Stop Hooks

| ID | Matcher | Funktion | Timeout |
|----|---------|------|------|
| stop:format-typecheck | * | Batch-Formatierung und Typpruefung | 300s |
| stop:check-console-log | * | console.log-Pruefung | 30s |
| stop:session-end | * | Session-Status-Persistierung | 10s |
| stop:evaluate-session | * | Session-Auswertung | 10s |
| stop:cost-tracker | * | Kostenverfolgung | 10s |
| stop:desktop-notify | * | Desktop-Benachrichtigung | 10s |

### Lebenszyklus-Hooks

| ID | Event | Funktion |
|----|-------|------|
| session:start | SessionStart | Kontext laden und Paketmanager erkennen |
| pre:compact | PreCompact | Status vor Komprimierung speichern |
| session:end:marker | SessionEnd | Session-Ende-Marker |