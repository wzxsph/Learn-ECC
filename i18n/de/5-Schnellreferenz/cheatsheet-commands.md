# Befehls-Cheatsheet

## Slash-Commands

| Befehl | Funktion | Verwendungsszenario |
|--------|----------|---------------------|
| `/tdd` | Testgetriebener Entwicklungsworkflow | Neue Funktionsentwicklung |
| `/plan` | Implementierungsplan erstellen | Komplexe Aufgabenplanung |
| `/code-review` | [Code-Review](../Referenzdokumente/commands/01-核心工作流.md) | Code-Qualitätsprüfung |
| `/build-fix` | [Build-Reparatur](../Referenzdokumente/commands/04-构建修复.md) | Bei Build-Fehlern |
| `/learn` | Muster aus Sitzung extrahieren | Wissens沉淀 |
| `/skill-create` | Skill aus Git generieren | Erfahrungswiederverwendung |
| `/test` | [Testbezogene Befehle](../Referenzdokumente/commands/02-测试相关.md) | Testbezogen |

## Skriptbefehle

| Skript | Funktion |
|--------|----------|
| `node scripts/hooks/run-with-flags.js` | Hooks mit Flags ausführen |
| `node tests/run-all.js` | Alle Tests ausführen |
| `node scripts/lib/utils.js` | Utility-Funktionen |

## Agent-Befehle

| Agent | Funktion |
|-------|----------|
| `/planner` | Aufgabenplanung |
| `/code-reviewer` | Code-Review |
| `/tdd-guide` | TDD-Anleitung |
| `/security-reviewer` | Sicherheitsreview |
| `/build-error-resolver` | Build-Reparatur |

## Globale Parameter

| Parameter | Beschreibung |
|-----------|--------------|
| `--model` | Modell angeben |
| `--no-input` | Non-Interaktiv-Modus |
| `--output` | Ausgabeformat |

## Hook-Umgebungsvariablen

| Variable | Beschreibung |
|----------|--------------|
| `ECC_HOOK_PROFILE` | Hook-Konfiguration |
| `ECC_DISABLED_HOOKS` | Deaktivierte Hook-Liste |

---

[Zurück zum Schnellreferenz-Verzeichnis](./README.md)