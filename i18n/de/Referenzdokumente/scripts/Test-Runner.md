# Test-Runner

Dieses Dokument beschreibt die Teststruktur und -ausfuehrung des ECC-Projekts.

## Test-Eingang

**Pfad**: `tests/run-all.js`

Zentralisierter Test-Runner, automatisch Testdateien entdecken und ausfuehren.

### Verwendung

```bash
# Alle Tests ausfuehren
node tests/run-all.js

# Einzelne Testdatei ausfuehren
node tests/lib/utils.test.js
node tests/hooks/hooks.test.js
```

### Testdateientdeckung

Test-Runner entdeckt Testdateien automatisch ueber Glob-Muster `tests/**/*.test.js`.

Regeln:
- Dateien müssen sich im `tests/`-Verzeichnis befinden
- Dateinamen müssen auf `.test.js` enden
- Verzeichnisstruktur bleibt fuer Anzeige erhalten

### Ausgabeformat

```
╔══════════════════════════════════════════════════════════╗
║           Everything Claude Code - Test Suite             ║
╚══════════════════════════════════════════════════════════╝

━━━ Running lib/utils.test.js ━━━
(Testausgabe...)
━━━ Running hooks/hooks.test.js ━━━
(Testausgabe...)

╔══════════════════════════════════════════════════════════╗
║                     Final Results                        ║
╠══════════════════════════════════════════════════════════╣
║  Total Tests:    42                                      ║
║  Passed:         42  ✓                                   ║
║  Failed:         0                                        ║
╚══════════════════════════════════════════════════════════╝
```

### Teststatistiken

Test-Runner parst Ausgabe jeder Testdatei um Statistiken zusammenzufassen:
- `Passed: <Anzahl>`
- `Failed: <Anzahl>`

---

## Teststruktur

### Verzeichnislayout

```
tests/
├── run-all.js              # Haupt-Test-Runner
├── lib/                    # Bibliotheks-Utils-Tests
│   ├── utils.test.js
│   ├── package-manager.test.js
│   └── ...
├── hooks/                  # Hook-Integrationstests
│   └── hooks.test.js
├── integration/            # Integrationstests
├── scripts/                # Skript-Tests
├── commands/               # Befehls-Tests
├── docs/                   # Dokumentations-Tests
├── opencode-config.test.js
├── plugin-manifest.test.js
├── test_*.py               # Python-Tests
└── conftest.py             # pytest-Konfiguration
```

### Testtypen

1. **Unit-Tests** (`*.test.js`)
   - Testen eigenstaendige Funktionen und Utils
   - Verwenden Node.js natives assert oder Jest-Style

2. **Integrationstests** (`tests/integration/`)
   - Testen Zusammenarbeit mehrerer Komponenten
   - Testen vollstaendige Hook-Workflows

3. **Python-Tests** (`test_*.py`)
   - pytest-Format fuer Python-Tests
   - Verwenden `conftest.py` fuer Konfiguration

---

## Testkonfiguration

### Node.js-Tests

Testdateien verwenden standardmaessiges Node.js assert-Modul:

```javascript
const assert = require('assert');

test('example test', () => {
  assert.strictEqual(actual, expected, 'Optional message');
});
```

### Python-Tests (pytest)

Position: `tests/conftest.py`

pytest-Konfigurationsdatei, bietet geteilte Fixtures und Konfiguration.

```bash
# Python-Tests ausfuehren
python -m pytest tests/

# Bestimmten Test ausfuehren
python -m pytest tests/test_builder.py
```

---

## Test-Best-Practices

### AAA-Muster

```
test('Testbeschreibung', () => {
  // Arrange - Testdaten vorbereiten
  const input = 'test data';

  // Act - Zu testende Operation ausfuehren
  const result = myFunction(input);

  // Assert - Ergebnis verifizieren
  assert.strictEqual(result, expected);
});
```

### Testbenennung

Beschreibende Namen verwenden die das Testverhalten klar erklaeren:

```javascript
test('gibt leeres Array zurueck wenn keine Maerkte Abfrage entsprechen', () => {});
test('wirft Fehler wenn API-Schluessel fehlt', () => {});
test('fallt auf Substring-Suche zurueck wenn Redis nicht verfuegbar ist', () => {});
```

### Testisolation

Jeder Test sollte:
- Unabhaengig ausgefuehrt werden, ohne von anderen Tests abzuhängen
- Eigene Testdaten bereinigen
- Externe Abhaengigkeiten mit Mocks isolieren

---

## CI-Integration

Testskripte in `package.json` konfigurieren:

```json
{
  "scripts": {
    "test": "validate-commands.js && tests/run-all.js"
  }
}
```

Test-Runner ist fuer CI-Umgebungen ausgelegt:
- Non-zero Exit-Code zeigt Fehler an
- JSON-Ausgabe fuer Automatisierungsparsing
- Klare Fehlermeldungen fuer Debugging