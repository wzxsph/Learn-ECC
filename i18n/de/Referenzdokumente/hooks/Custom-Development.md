# Benutzerdefinierte Entwicklung

## Ueberblick

Dieses Handbuch beschreibt wie man benutzerdefinierte Hooks fuer das ECC Hooks-System entwickelt, einschliesslich Grundstruktur, Best Practices und exit 0-Anforderungen.

## Grundstruktur

### Minimaler Hook-Template

```javascript
// my-custom-hook.js
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Werkzeuginformationen zugreifen
  const toolName = input.tool_name;        // "Edit", "Bash", "Write" usw.
  const toolInput = input.tool_input;      // Werkzeugspezifische Parameter
  const toolOutput = input.tool_output;    // Nur bei PostToolUse verfuegbar

  // Warnung (non-blocking): an stderr schreiben
  console.error('[Hook] Warnmeldung');

  // Blockieren (nur PreToolUse): exit code 2
  // process.exit(2);

  // Immer die originalen Daten an stdout ausgeben
  console.log(data);
});
```

---

## Exit-Code-Anforderungen

### Schlüsselregel: exit 0 bei nicht-kritischen Fehlern

**Wichtig**: Alle Hooks muessen bei nicht-kritischen Fehlern exit 0 verwenden, um unerwartetes Blockieren der Werkzeugausfuehrung zu vermeiden.

| Exit-Code | Bedeutung | Verwendungszweck |
|--------|------|----------|
| 0 | Erfolg | Fortfahren, oder nicht-kritische Warnung |
| 2 | Blockieren | Nur fuer kritisches Blockieren bei PreToolUse |
| Andere nicht-null | Fehler | Nur loggen, **nicht verwenden** |

### Warum nicht-null Exit-Codes schlecht sind?

Wenn ein Hook mit nicht-null Exit-Code beendet wird (ausser 2), markiert Claude Code die gesamte Werkzeugausfuehrung als fehlgeschlagen, was zu Folgendem fuehren kann:
- Werkzeugausfuehrung unerwartet unterbrochen
- Beeintraechtigte Benutzererfahrung
- Schwierig zu debuggende Probleme

### Korrekte Fehlerbehandlung

```javascript
// Falsches Beispiel - Nicht tun
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Verarbeitungslogik
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    process.exit(1);  // Fehler: Blockiert Werkzeug
  }
  console.log(data);
});

// Richtig - So tun
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // Verarbeitungslogik
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    // Nicht-kritischer Fehler, exit 0 fortfahren
    console.log(data);
    return;
  }
  console.log(data);
});
```

---

## Hook-Eingabemuster

### HookInput Interface

```typescript
interface HookInput {
  tool_name: string;          // Werkzeugname
  tool_input: {               // Werkzeugeingabeparameter
    command?: string;         // Bash: Befehl
    file_path?: string;        // Edit/Write/Read: Dateipfad
    old_string?: string;       // Edit: Ersetzter Text
    new_string?: string;       // Edit: Ersetzungstext
    content?: string;          // Write: Dateiinhalt
  };
  tool_output?: {             // Nur PostToolUse
    output?: string;          // Befehl/Werkzeugausgabe
  };
}
```

### Auf Werkzeuginformationen zugreifen

```javascript
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // Werkzeugname erhalten
  console.log('Tool:', input.tool_name);

  // Nach Werkzeugtyp bearbeiten
  if (input.tool_name === 'Bash') {
    console.log('Command:', input.tool_input.command);
  }

  if (input.tool_name === 'Edit' || input.tool_name === 'Write') {
    console.log('File:', input.tool_input.file_path);
  }

  // PostToolUse kann Ausgabe zugreifen
  if (input.tool_output) {
    console.log('Output:', input.tool_output.output);
  }
});
```

---

## Haeufige Hook-Rezepte

### TODO/FIXME-Kommentare warnen

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "Warn when TODO/FIXME comments added"
}
```

### Erstellen von zu grossen Dateien blockieren

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');console.error('[Hook] Split into smaller, focused modules');process.exit(2)}console.log(d)})\""
  }],
  "description": "Block creating files over 800 lines"
}
```

### Python-Dateien nach dem Bearbeiten automatisch mit ruff formatieren

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/\\.py$/.test(p)){const{execFileSync}=require('child_process');try{execFileSync('ruff',['format',p],{stdio:'pipe'})}catch(e){}}console.log(d)})\""
  }],
  "description": "Auto-format Python files with ruff after editing"
}
```

### Neue Quelldateien erfordern Tests

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p);console.error('[Hook] Expected: '+testPath);console.error('[Hook] Consider writing tests first (/tdd)')}}console.log(d)})\""
  }],
  "description": "Remind to create tests when adding new source files"
}
```

---

## Asynchrone Hooks

Fuer Hooks die den Hauptfluss nicht blockieren sollten (z.B. Hintergrundanalysen):

```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node my-slow-hook.js",
    "async": true,
    "timeout": 30
  }]
}
```

### Asynchrone Hook-Hinweise

- Asynchrone Hooks laufen im Hintergrund
- Koennen Werkzeugausfuehrung nicht blockieren
- Sollten innerhalb von 30 Sekunden abschliessen
- Geeignet fuer: Logging, Analysen, Telemetrie

---

## Blockierende Hooks

Fuer Faelle die Werkzeugausfuehrung blockieren muessen (nur PreToolUse):

```javascript
// Bei PreToolUse blockieren
process.exit(2);  // Exit-Code 2 bedeutet blockieren
```

### Wann Blockieren verwenden

- Sicherheitspruefung fehlgeschlagen (z.B. Schluessel erkannt)
- Verstoss gegen harte Richtlinien
- Moegliche Datenverlustoperationen

### Wann nicht blockieren

- Code-Stil-Probleme (Warnung stattdessen verwenden)
- Nicht-kritische Pruefungen
- Vorschlagsartige Pruefungen

---

## Laufzeitkonfiguration

### run-with-flags.js verwenden

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const p=require('path');const r=(()=>{var e=process.env.CLAUDE_PLUGIN_ROOT;if(e&&e.trim())return e.trim();var p=require('path'),f=require('fs'),h=require('os').homedir(),d=p.join(h,'.claude'),q=p.join('scripts','lib','utils.js');if(f.existsSync(p.join(d,q)))return d;for(var s of [['ecc'],['ecc@ecc'],['marketplaces','ecc'],['everything-claude-code'],['everything-claude-code@everything-claude-code'],['marketplaces','everything-claude-code']]){var l=p.join(d,'plugins',...s);if(f.existsSync(p.join(l,q)))return l}try{for(var g of ['ecc','everything-claude-code']){var b=p.join(d,'plugins','cache',g);for(var o of f.readdirSync(b,{withFileTypes:true})){if(!o.isDirectory())continue;for(var v of f.readdirSync(p.join(b,o.name),{withFileTypes:true})){if(!v.isDirectory())continue;var c=p.join(b,o.name,v.name);if(f.existsSync(p.join(c,q)))return c}}}}catch(x){}return d})();const s=p.join(r,'scripts/hooks/plugin-hook-bootstrap.js');process.env.CLAUDE_PLUGIN_ROOT=r;process.argv.splice(1,0,s);require(s)\" node scripts/hooks/run-with-flags.js my-hook scripts/hooks/my-hook.js standard,strict"
  }]
}
```

### Vereinfachte Version

Direkt Skriptpfad verwenden (wenn Plugin-Root bekannt):

```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/my-hook.js"
  }]
}
```

---

## Plattformuebergreifende Hinweise

### Pfadbehandlung

```javascript
const path = require('path');
const os = require('os');

// path.join statt hartcodierter Pfadtrennzeichen verwenden
const configPath = path.join(os.homedir(), '.claude', 'config.json');

// Plattform erkennen
if (process.platform === 'win32') {
  // Windows-spezifische Behandlung
}
```

### Prozessausgabe

```javascript
// console.error fuer Warnungen verwenden (dem Benutzer angezeigt)
console.error('[Hook] Warning: some issue detected');

// console.log fuer originale Daten verwenden (erforderlich)
console.log(data);

// Niemals andere Textnachrichten an stdout ausgeben (zerstoert Datenstrom)
```

---

## Performance-Best-Practices

### Schnell bleiben

- PreToolUse-Hooks: <200ms
- PostToolUse synchrone Hooks: <1 Sekunde
- Asynchrone Hooks: <30 Sekunden

### Blockierende Operationen vermeiden

```javascript
// Schlecht: Synchrone Dateilesung
const content = fs.readFileSync('large-file.txt', 'utf8');

// Gut: Asynchron oder Lazy-Loading
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // Verarbeiten
});
```

### Ergebnisse cachen

```javascript
// Teure Operationen cachen
let cachedResult = null;
let cacheTime = 0;
const CACHE_TTL = 60000; // 1 Minute

function getCachedResult() {
  const now = Date.now();
  if (!cachedResult || now - cacheTime > CACHE_TTL) {
    cachedResult = expensiveOperation();
    cacheTime = now;
  }
  return cachedResult;
}
```

---

## Hooks testen

### Manuell testen

```bash
# PreToolUse-Hook testen
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"}}' | node scripts/hooks/my-hook.js

# PostToolUse-Hook testen
echo '{"tool_name":"Bash","tool_input":{"command":"echo test"},"tool_output":{"output":"test\n"}}' | node scripts/hooks/my-hook.js
```

### Testausgabeformat

```javascript
// Korrekte stdout-Ausgabe (JSON)
console.log(data);  // Originale Eingabedaten

// Korrekte stderr-Ausgabe (Warnungen)
console.error('[Hook] Warning message');

// Nicht korrekt - Keine weiteren Inhalte an stdout ausgeben
console.log('Some message');  // Dies zerstoert Datenstrom
```

---

## Hooks debuggen

### Debug-Ausgabe hinzufuegen

```javascript
const DEBUG = process.env.DEBUG_HOOKS === '1';

process.stdin.on('end', () => {
  if (DEBUG) {
    console.error('[DEBUG] Received input:', data);
  }
  // Verarbeitungslogik
  if (DEBUG) {
    console.error('[DEBUG] Sending output');
  }
  console.log(data);
});
```

### Haeufige Probleme

| Problem | Ursache | Loesung |
|------|------|----------|
| Werkzeug unerwartet blockiert | Hook exit mit nicht-null (ausser 2) | exit 0 und stderr-Warnungen verwenden |
| Haengt | Hook endet nicht | Sicherstellen dass immer an stdout ausgegeben wird |
| Datenbeschaedigung | stdout-Ausgabe ist nicht JSON | Nur originale data ausgeben |
| Performance-Probleme | Hook zu langsam | Optimieren oder async verwenden |

---

## In hooks.json integrieren

### Neuen Hook hinzufuegen

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "node scripts/hooks/my-new-hook.js"
          }
        ],
        "description": "My new hook description",
        "id": "pre:custom:my-new-hook"
      }
    ]
  }
}
```

### Eingebaute Hooks deaktivieren

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [],
        "description": "Documentation file warning disabled"
      }
    ]
  }
}
```

---

## Best-Practices-Zusammenfassung

1. **Immer exit 0 bei nicht-kritischen Fehlern** - Keine nicht-null Exit-Codes verwenden (ausser 2)
2. **console.error fuer Warnungen verwenden** - Nicht console.log
3. **Schnell bleiben** - PreToolUse <200ms
4. **Immer originale data an stdout ausgeben** - Datenstrom nicht veraendern
5. **Asynchrone Verarbeitung fuer laengere Operationen verwenden** - async: true setzen
6. **Plattformuebergreifende Pfadbehandlung** - path.join verwenden
7. **Ausgabeformat testen** - Sicherstellen stdout ist originales JSON
8. **Hooks dokumentieren** - Klare description hinzufuegen