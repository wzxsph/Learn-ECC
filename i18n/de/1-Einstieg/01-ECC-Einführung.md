# 01 - [ECC](../Referenzdokumente/README.md) Einführung

## Was ist [ECC](../Referenzdokumente/README.md)?

[ECC](../Referenzdokumente/README.md) (Everything Claude Code) ist ein Claude Code Plugin-System, das produktionsreife [Agents](../Referenzdokumente/agents/1.%20代码审查类.md), [Skills](../Referenzdokumente/skills/最佳实践.md), Hooks, Rules, [Commands](../Referenzdokumente/commands/01-核心工作流.md) und MCP-Konfigurationslösungen bietet. Es stammt aus einem preisgekrönten Anthropic Hackathon-Projekt und wurde über 10+ Monate im täglichen Gebrauch verfeinert.

## Was kann [ECC](../Referenzdokumente/README.md)?

### 1. [Agent](../Referenzdokumente/agents/1.%20代码审查类.md) System
[ECC](../Referenzdokumente/README.md) bietet 60+ professionelle [Agents](../Referenzdokumente/agents/1.%20代码审查类.md), die mehrsprachiges Review, Build-Fixes, Architekturdesign und mehr abdecken:

| [Agent](../Referenzdokumente/agents/1.%20代码审查类.md) | Verwendung |
|-------|------|
| `planner` | Funktionsimplementierungsplanung |
| `code-reviewer` | Codequalität und Sicherheitsreview |
| `security-reviewer` | Sicherheitslstellenanalyse |
| `build-error-resolver` | Build-Fehlerbehebung |
| `architect` | Systemarchitekturdesign |
| `tdd-guide` | Testgetriebene Entwicklung |

### 2. [Skills](../Referenzdokumente/skills/最佳实践.md) Workflows
[Skills](../Referenzdokumente/skills/最佳实践.md) sind die primäre Workflow-Oberfläche, [ECC](../Referenzdokumente/README.md) bietet 232+ [Skills](../Referenzdokumente/skills/最佳实践.md):

- `tdd-workflow` - Testgetriebene Entwicklung
- `e2e-testing` - End-to-End-Tests
- `security-review` - Sicherheitsreview-Checkliste
- `continuous-learning-v2` - Instinct-basiertes kontinuierliches Lernen
- `eval-harness` - Verifizierungsschleifen-Evaluation

### 3. [Commands](../Referenzdokumente/commands/01-核心工作流.md)
Beibehaltung der traditionellen Schrägstrich-Befehlskompatibilität (75+ Befehle):

| Befehl | Funktion |
|------|------|
| `/plan` | Implementierungsplan erstellen |
| `/code-review` | Codereview |
| `/build-fix` | Build-Fehler beheben |
| `/learn` | Muster aus Sitzung extrahieren |
| `/skill-create` | Skill aus Git-Verlauf generieren |
| `/sessions` | Sitzungsverlaufsverwaltung |

### 4. Hooks-Automatisierung
Flexibles Hook-System (8 Ereignistypen):
- `SessionStart` - Kontext beim Sitzungsstart laden
- `SessionEnd` - Status beim Sitzungsende speichern
- `PreToolUse` - Validierung vor Werkzeugausführung
- `PostToolUse` - Formatierung nach Werkzeugausführung

### 5. Rules-System
Umfassende Entwicklungsrichtlinien (common + sprachspezifisch):
- **common/** - Allgemeine Prinzipien (Sicherheit, Codestil, Tests, Performance)
- **typescript/** - TypeScript/JavaScript
- **python/** - Python
- **golang/** - Go
- **swift/** - Swift
- **php/** - PHP
- **arkts/** - HarmonyOS/ArkTS

### 6. MCP-Services
Unterstützung für 14+ MCP-Server-Konfigurationen: GitHub, Supabase, Vercel, Railway, Context7, Exa, Playwright und mehr.

## Plattformübergreifende Unterstützung

[ECC](../Referenzdokumente/README.md) unterstützt verschiedene AI-Programmierwerkzeuge:

| Werkzeug | Agents | Commands | Skills | Hooks |
|------|--------|----------|--------|-------|
| Claude Code | 60 | 75 | 232 | 8 types |
| Cursor | 48 | geteilt | geteilt | 15 types |
| Codex | geteilt | instruktionsbasiert | 32 | - |
| OpenCode | 12 | 35 | 37 | 11 types |
| GitHub Copilot | - | 6 prompts | instruktionsbasiert | - |

## Kernkonzepte

### [Agent](../Referenzdokumente/agents/1.%20代码审查类.md)
Wiederverwendbare Aufgaben-Subausführende, jeder [Agent](../Referenzdokumente/agents/1.%20代码审查类.md) hat klar definierte Verantwortlichkeiten. Komplexe Workflows durch Kombination mehrerer [Agents](../Referenzdokumente/agents/1.%20代码审查类.md) aufbauen.

### [Skill](../Referenzdokumente/skills/最佳实践.md)
Definiert den Workflow zur Erfüllung bestimmter Aufgaben, ist die primäre Workflow-Oberfläche von [ECC](../Referenzdokumente/README.md).

### Hook
Skripte, die zu bestimmten Zeitpunkten automatisch ausgelöst werden, unterstützen Workflow-Automatisierung.

### Rule
Einschränkungen, denen Claude Code folgen muss, um die Ausgabequalität zu gewährleisten.


- [Installation](./02-Installation.md) - Beginne mit der Nutzung von [ECC](../Referenzdokumente/README.md)
- [Zurück zum Einsteiger-Verzeichnis](../README.md)