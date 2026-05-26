# Agent-Cheatsheet

## Integrierte Agents

### Planungsklasse
| Agent | Verwendungszweck | Merkmale |
|-------|------------------|----------|
| `planner` | [Implementierungsplanung](../Referenzdokumente/agents/6.%20架构类.md) | Aufgaben zerlegen, Schritte generieren |
| `architect` | [Systemdesign](../Referenzdokumente/agents/6.%20架构类.md) | Architekturentscheidungen, Technologieauswahl |

### Review-Klasse
| Agent | Verwendungszweck | Merkmale |
|-------|------------------|----------|
| `code-reviewer` | Code-Review | Qualitätskontrolle, Pattern-Prüfung |
| `security-reviewer` | Sicherheitsreview | Schwachstellenerkennung, OWASP |
| `rust-reviewer` | Rust-Review | Lebenszyklus, Borrow-Checking |

### Entwicklungsklasse
| Agent | Verwendungszweck | Merkmale |
|-------|------------------|----------|
| `tdd-guide` | [TDD-Anleitung](../Referenzdokumente/agents/1.%20代码审查类.md) | Test-First, Inkrementelles Refactoring |
| `build-error-resolver` | [Build-Reparatur](../Referenzdokumente/agents/2.%20构建修复类.md) | Fehlerlokalisierung, Reparaturoptionen |

### Operations-Klasse
| Agent | Verwendungszweck | Merkmale |
|-------|------------------|----------|
| `e2e-runner` | [E2E-Tests](../Referenzdokumente/agents/1.%20代码审查类.md) | Kritische Pfade, Automatisierung |
| `refactor-cleaner` | [Refactoring-Bereinigung](../Referenzdokumente/agents/1.%20代码审查类.md) | Tot-Code-Entfernung |

## Agent-Aufrufmethoden

```
/[agent-name]                    # Direktaufruf
/[agent-name] [parameters]       # Mit Parametern
/[agent-name] --option value    # Optionsparameter
```

## Benutzerdefinierte Agents

Agent-Dateiformat: `agents/*.md` + YAML Frontmatter

```yaml
---
name: my-agent
description: Mein benutzerdefinierter Agent
tools: [Read, Bash, Edit]
model: sonnet
---
```

---

[Zurück zum Schnellreferenz-Verzeichnis](./README.md)