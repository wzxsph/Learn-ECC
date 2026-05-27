# Utility-Funktionsbibliothek

Dieses Dokument beschreibt die Kern-Utility-Funktionsbibliothek im `scripts/lib/` Verzeichnis.

---

## utils.js

**Pfad**: `scripts/lib/utils.js`

Plattformuebergreifende Utility-Funktionsbibliothek, anwendbar auf Windows, macOS und Linux.

### Plattformerkennung

```javascript
const { isWindows, isMacOS, isLinux } = require('./utils');

// isWindows: true unter Windows
// isMacOS: true unter macOS
// isLinux: true unter Linux
```

### Verzeichnisoperationen

```javascript
const {
  getHomeDir,      // Benutzer-Home-Verzeichnis abrufen
  getClaudeDir,    // ~/.claude Verzeichnis abrufen
  getSessionsDir,  // ~/.claude/session-data Verzeichnis abrufen
  getTempDir,      // Temp-Verzeichnis abrufen
  ensureDir,       // Verzeichnis sicherstellen, wenn nicht vorhanden erstellen
  findFiles        // Dateien nach Muster finden
} = require('./utils');

// Verzeichnis erstellen falls nicht vorhanden
ensureDir('/path/to/dir');

// Dateien finden
const files = findFiles('/path', '*.js', { recursive: true });
```

### Datum und Zeit

```javascript
const {
  getDateString,      // Gibt Format YYYY-MM-DD zurueck
  getTimeString,      // Gibt Format HH:MM zurueck
  getDateTimeString   // Gibt Format YYYY-MM-DD HH:MM:SS zurueck
} = require('./utils');
```

### Sitzungen und Projekte

```javascript
const {
  sanitizeSessionId,    // Sitzungs-ID bereinigen, ungueltige Zeichen entfernen
  getSessionIdShort,    // Kurze Sitzungs-ID abrufen
  getGitRepoName,       // Git-Repository-Name abrufen
  getProjectName        // Projektname abrufen
} = require('./utils');

// Sitzungs-ID bereinigen
const cleanId = sanitizeSessionId('my-session@123'); // 'my-session-123'

// Projektname abrufen
const project = getProjectName();
```

### Dateioperationen

```javascript
const {
  readFile,        // Datei sicher lesen, bei Fehler null zurueckgeben
  writeFile,       // In Datei schreiben
  appendFile,      // An Datei anhaengen
  replaceInFile,   // Dateiinhalt ersetzen
  countInFile,     // Muster-Vorkommen in Datei zaehlen
  grepFile         // Muster in Datei suchen
} = require('./utils');

// Datei lesen
const content = readFile('/path/to/file.txt');

// Dateiinhalt ersetzen (erstes Match)
replaceInFile('/path', 'old', 'new');

// Alle Matches ersetzen
replaceInFile('/path', 'old', 'new', { all: true });

// Vorkommen zaehlen
const count = countInFile('/path', /pattern/g);

// Suchen und passende Zeilen zurueckgeben
const matches = grepFile('/path', /regex/);
// Gibt zurueck: [{ lineNumber: 1, content: '...' }]
```

### Stringverarbeitung

```javascript
const { stripAnsi } = require('./utils');

// ANSI-Escape-Sequenzen entfernen (Farben usw.)
const clean = stripAnsi('\x1b[32mHello\x1b[0m'); // 'Hello'
```

### Hook I/O

```javascript
const { readStdinJson, log, output } = require('./utils');

// JSON von stdin lesen ( fuer Hooks )
const input = await readStdinJson({ timeoutMs: 5000 });

// An stderr ausgeben (dem Benutzer sichtbar)
log('Debug message');

// An stdout ausgeben (an Claude zurueckgegeben)
output({ result: 'data' });
```

### Systembefehle

```javascript
const { commandExists, runCommand, isGitRepo, getGitModifiedFiles } = require('./utils');

// Pruefen ob Befehl existiert
if (commandExists('git')) { /* ... */ }

// Befehl ausfuehren (nur Whitelist-Befehle)
const result = runCommand('git rev-parse --show-toplevel');
if (result.success) {
  console.log(result.output);
}

// Pruefen ob Git-Repository
if (isGitRepo()) { /* ... */ }

// Geaenderte Dateien abrufen
const files = getGitModifiedFiles(['\\.js$', '\\.md$']);
```

---

## package-manager.js

**Pfad**: `scripts/lib/package-manager.js`

Paketmanager-Erkennungs- und Auswahlwerkzeug.

### Unterstuetzte Paketmanager

- npm
- pnpm
- yarn
- bun

### Erkennungsprioeritaet

1. Umgebungsvariable `CLAUDE_PACKAGE_MANAGER`
2. Projektkonfiguration `.claude/package-manager.json`
3. `packageManager`-Feld in `package.json`
4. Lock-Datei-Erkennung
5. Globale Benutzerpraeferenz `~/.claude/package-manager.json`
6. Standardwert npm

### Kern-API

```javascript
const {
  getPackageManager,           // Paketmanager des aktuellen Projekts abrufen
  setPreferredPackageManager,  // Globale Praeferenz setzen
  setProjectPackageManager,    // Projektpraeferenz setzen
  getAvailablePackageManagers, // Auf dem System verfuegbare Paketmanager abrufen
  detectFromLockFile,          // Aus Lock-Datei erkennen
  detectFromPackageJson,       // Aus package.json erkennen
  getRunCommand,               // Befehl zum Ausfuehren von Skripten abrufen
  getExecCommand,              // Befehl zum Ausfuehren von Binaries abrufen
  getSelectionPrompt,          // Interaktive Auswahlaufforderung abrufen
  getCommandPattern            // Befehlsuebereinstimmungregex generieren
} = require('./package-manager');

// Aktuellen Paketmanager abrufen
const { name, config, source } = getPackageManager();
// name: 'pnpm', 'npm', 'yarn', oder 'bun'
// config: { installCmd, runCmd, execCmd, testCmd, ... }
// source: 'environment', 'project-config', 'lock-file', ...

// Globale Praeferenz setzen
setPreferredPackageManager('pnpm');

// Projektpraeferenz setzen
setProjectPackageManager('yarn', '/path/to/project');

// Ausfuehrungsbefehl abrufen
const testCmd = getRunCommand('test'); // 'pnpm test' oder 'npm test' usw.
const buildCmd = getRunCommand('build');

// Ausfuehrungsbefehl abrufen
const eslintCmd = getExecCommand('eslint', '--fix .');
```

### Sicherheitspruefungen

`getRunCommand` und `getExecCommand` validieren Eingaben:
- Skriptnamen erlauben nur Buchstaben, Zahlen, Bindestrich, Unterstrich, Punkt, Schraegstrich, @
- Parameter erlauben nur Buchstaben, Zahlen, Leerzeichen, Bindestrich, Punkte, Schraegstriche, Gleichheitszeichen, Doppelpunkte, Kommas, Anfuehrungszeichen, @

---

## session-manager.js

**Pfad**: `scripts/lib/session-manager.js`

Sitzungsverwaltungsbibliothek, bietet CRUD-Operationen fuer Sitzungen.

### Sitzungsspeicher

- Neues Format: `~/.claude/session-data/YYYY-MM-DD-[session-id]-session.tmp`
- Legacy-Format-Kompatibilitaet: `~/.claude/sessions/YYYY-MM-DD-session.tmp`

### Kern-API

```javascript
const {
  parseSessionFilename,      // Sitzungsdateiname parsen
  getSessionPath,            // Vollstaendigen Sitzungsdateipfad abrufen
  getSessionContent,         // Sitzungsinhalt lesen
  parseSessionMetadata,      // Sitzungsmetadaten parsen
  getSessionStats,           // Sitzungsstatistiken abrufen
  getSessionTitle,           // Sitzungstitel abrufen
  getSessionSize,            // Sitzungsgroesse abrufen (humanlesbar)
  getAllSessions,            // Alle Sitzungen abrufen (mit Paginierung und Filterung)
  getSessionById,            // Einzelne Sitzung nach ID abrufen
  writeSessionContent,       // Sitzungsinhalt schreiben
  appendSessionContent,      // Sitzungsinhalt anhaengen
  deleteSession,            // Sitzung loeschen
  sessionExists              // Pruefen ob Sitzung existiert
} = require('./session-manager');

// Alle Sitzungen abrufen
const { sessions, total, offset, limit, hasMore } = getAllSessions({
  limit: 50,
  offset: 0,
  date: '2026-05-26',
  search: 'abc123'
});

// Einzelne Sitzung abrufen
const session = getSessionById('abc123', true);
// session: { filename, shortId, date, sessionPath, content, metadata, stats, ... }

// Sitzungsmetadaten parsen
const metadata = parseSessionMetadata(content);
// metadata: { title, date, started, lastUpdated, project, branch, worktree, completed, inProgress, notes, context }

// Sitzungsstatistiken abrufen
const stats = getSessionStats(sessionPathOrContent);
// stats: { totalItems, completedItems, inProgressItems, lineCount, hasNotes, hasContext }

// Sitzung schreiben
writeSessionContent(sessionPath, '# New Session\n\nContent...');

// Inhalt anhaengen
appendSessionContent(sessionPath, '\n\nMore content...');

// Sitzung loeschen
deleteSession(sessionPath);
```

### Sitzungsmetadaten-Format

Metadaten im Sitzungsinhalt:

```markdown
# Session Title

**Date:** 2026-05-26
**Started:** 14:30
**Last Updated:** 15:45
**Project:** my-project
**Branch:** main
**Worktree:** frontend

### Completed
- [x] Task 1
- [x] Task 2

### In Progress
- [ ] Task 3

### Notes for Next Session
Remember to check the config file.

### Context to Load
```
previous-context-content
```
```

---

## install-executor.js

**Pfad**: `scripts/lib/install-executor.js`

ECC-Installations-Ausfuehrer, verantwortlich fuer tatsaechliche Installationsoperationen.

### Hauptfunktionen

- Installationsplan erstellen
- Dateikopieroperationen ausfuehren
- Installationsstatus aufzeichnen

### Kern-API

```javascript
const {
  applyInstallPlan,       // Installationsplan anwenden
  listAvailableLanguages  // Verfuegbare Sprachen auflisten
} = require('./install-executor');

// Verfuegbare Sprachen auflisten
const languages = listAvailableLanguages();
// ['typescript', 'javascript', 'python', ...]
```

---

## install-lifecycle.js

**Pfad**: `scripts/lib/install-lifecycle.js`

Installationslebenszyklus-Management, behandelt die verschiedenen Phasen der Installation.

---

## install-manifests.js

**Pfad**: `scripts/lib/install-manifests.js`

Installationsmanifest-Definitionen, einschliesslich:
- Unterstuetzte Installationsziele
- Verfuegbare Sprachenlisten
- Unterstuetzte Lokalisierungen

---

## install-state.js

**Pfad**: `scripts/lib/install-state.js`

Installationsstatus-Management, zeichnet auf welche Dateien wohin installiert wurden.

---

## project-detect.js

**Pfad**: `scripts/lib/project-detect.js`

Projekttyp-Erkennung, erkennt den Projekttyp des aktuellen Verzeichnisses.

---

## observer-sessions.js

**Pfad**: `scripts/lib/observer-sessions.js`

Sitzungs-Beobachter, zur Ueberwachung von Sitzungsaktivitaeten.

---

## shell-substitution.js

**Pfad**: `scripts/lib/shell-substitution.js`

Shell-Variablen-Substitutionswerkzeug, behandelt Umgebungsvariablen und Shell-Ausdruecke.

---

## tmux-worktree-orchestrator.js

**Pfad**: `scripts/lib/tmux-worktree-orchestrator.js`

Tmux + Git Worktree-Orchestrator, verwaltet komplexe Entwicklungsumgebungen.

---

## mcp-config.js

**Pfad**: `scripts/lib/mcp-config.js`

MCP (Model Context Protocol) Konfigurationsmanagement.

---

## Sonstige Werkzeuge

| Datei | Beschreibung |
|------|--------------|
| `agent-compress.js` | Agent-Kontextkomprimierung |
| `cost-estimate.js` | Kostenveranschlagung |
| `github-discussions.js` | GitHub-Diskussionen-Integration |
| `hook-flags.js` | Hook-Flag-Management |
| `inspection.js` | Inspektionswerkzeug |
| `orchestration-session.js` | Orchestrierungs-Sitzungsverwaltung |
| `resolve-ecc-root.js` | ECC-Root-Verzeichnis-Aufloesung |
| `resolve-formatter.js` | Formatierer-Aufloesung |
| `session-aliases.js` | Sitzungsalias-Management |
| `session-bridge.js` | Sitzungsbruecke |
| `shell-split.js` | Shell-Befehl-Aufspaltung |
| `state-store/` | Status-Speicher-Submodul |
| `install/` | Installations-bezogenes Submodul |
| `skill-evolution/` | Faehigkeits-Evolutions-Submodul |
| `skill-improvement/` | Faehigkeits-Verbesserungs-Submodul |