# Projekt 1: CLI-Tool

Erstelle ein Kommandozeilen-Tool mit ECC.

## Projektziele

**Geschätzte Dauer**: 2-3 Stunden

Erstelle ein `ecc-gen` Tool zur Generierung von ECC-Projektkomponenten (Agent, Skill, Hook).

## Funktionsanforderungen

### 1. Kernfunktionen
- `ecc-gen agent <name>` - Agent-Datei generieren
- `ecc-gen skill <name>` - Skill-Datei generieren
- `ecc-gen hook <name>` - Hook-Konfiguration generieren

### 2. Interaktive Funktionen
- Interaktive Frage-und-Antwort-Generierung
- Vorlagenauswahl
- Ausgabepfad festlegen

### 3. Erweiterte Funktionen
- Stapelgenerierung
- Benutzerdefinierte Vorlagen
- Konfigurationsdatei-Unterstützung

## Technischer Ansatz

### Verzeichnisstruktur

```
ecc-gen/
├── src/
│   ├── commands/
│   │   ├── agent.js
│   │   ├── skill.js
│   │   └── hook.js
│   ├── generators/
│   │   └── templates/
│   └── index.js
├── package.json
└── README.md
```

### Verwendete Bibliotheken
- `commander` - CLI-Parameter-Parsing
- `inquirer` - Interaktive Fragen
- `ejs` - Template-Engine

## Abnahmekriterien

- [ ] Alle drei Unterbefehle funktionieren korrekt
- [ ] Generierte Vorlagen haben das richtige Format
- [ ] `--output` zur Ausgabepfadangabe wird unterstützt
- [ ] Unit-Tests enthalten
- [ ] Dokumentation vollständig

## Lernschwerpunkte

- CLI-Tool-Entwicklung
- Vorlagensystem-Design
- Interaktive Eingabeverarbeitung
- Modulare Architektur

> Referenzbefehle: [/build-fix](../Referenzdokumente/commands/04-构建修复.md) | [/test](../Referenzdokumente/commands/02-测试相关.md)

---

[Zurück zum Verzeichnis der praktischen Projekte](./README.md)