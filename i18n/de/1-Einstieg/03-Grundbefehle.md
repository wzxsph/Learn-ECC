# 03 - Grundbefehle

## Befehlstypen

ECC bietet drei Befehlstypen:

### 1. Schrägstrich-Befehle (Slash Commands)

Direkt eingeben und verwenden:

| Befehl | Funktion | Beispiel |
|------|------|------|
| `/ecc:plan` | Implementierungsplan erstellen | `/ecc:plan "Benutzerauthentifizierung hinzufügen"` |
| `/ecc:code-review` | Codereview | `/ecc:code-review` |
| `/ecc:build-fix` | Build-Fehler beheben | `/ecc:build-fix` |
| `/ecc:learn` | Muster aus Sitzung extrahieren | `/ecc:learn` |
| `/ecc:skill-create` | Skill aus Git-Verlauf generieren | `/ecc:skill-create` |
| `/ecc:sessions` | Sitzungsverlaufsverwaltung | `/ecc:sessions` |

Hinweis: Manuell installierte Versionen verwenden möglicherweise Befehle ohne Präfix (wie `[/plan](../Referenzdokumente/commands/01-核心工作流.md)`), Plugin-Installationen verwenden das `/ecc:` Präfix.

### 2. Skills (Primäre Workflow-Oberfläche)

Skills sind die primäre Workflow-Aufrufmethode von ECC:

| Skill | Funktion |
|-------|------|
| `[tdd-workflow](../Referenzdokumente/skills/测试.md)` | Testgetriebene Entwicklung |
| `[e2e-testing](../Referenzdokumente/skills/测试.md)` | End-to-End-Tests |
| `security-review` | Sicherheitsreview |
| `verification-loop` | Verifizierungsschleife |
| `search-first` | Research-First-Workflow |

Aufrufmethode:
```
Verwende [tdd-workflow](../Referenzdokumente/skills/测试.md) Skill
```

### 3. Agent-Befehle

Unteraufgaben durch Agents ausführen:

| Agent | Verwendung |
|-------|------|
| `planner` | Implementierungsplanung |
| `code-reviewer` | Codereview |
| `build-error-resolver` | Build-Fehlerbehebung |
| `security-reviewer` | Sicherheitsreview |
| `architect` | Architekturdesign |

Aufrufmethode:
```
@planner
```

## Häufige Workflows

### Neue Funktion entwickeln

```
/ecc:plan "Benutzerauthentifizierung hinzufügen"
→ planner erstellt Implementierungsplan
Verwende [tdd-workflow](../Referenzdokumente/skills/测试.md) Skill
→ tdd-guide erzwingt Test-zuerst
/ecc:code-review
→ code-reviewer prüft Code
```

### Bug beheben

```
Verwende [tdd-workflow](../Referenzdokumente/skills/测试.md) Skill
→ tdd-guide: Reproduktionstest schreiben
→ Fix implementieren, Test bestehen
/ecc:code-review
→ code-reviewer: Regression prüfen
```

### Für Veröffentlichung vorbereiten

```
/ecc:security-scan
→ security-reviewer: OWASP Top 10 Audit
Verwende [e2e-testing](../Referenzdokumente/skills/测试.md) Skill
→ e2e-runner: Kritische User-Flows testen
/ecc:test-coverage
→ 80%+ Abdeckung verifizieren
```

## MCP-Befehle

| Befehl | Funktion |
|------|------|
| `/mcp` | MCP-Server verwalten |
| `/ecc:status` | Status prüfen |

## Nächste Schritte

- [Erste Aufgabe](./04-Erste-Aufgabe.md) - Deine erste Aufgabe abschließen
- [Zurück zum Einsteiger-Verzeichnis](../README.md)