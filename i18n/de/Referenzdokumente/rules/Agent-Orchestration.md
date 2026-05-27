# Agenten-Aufgaben

## Verfuegbare Agenten

In `~/.claude/agents/`:

| Agent | Zweck | Wann verwenden |
|-------|------|----------------|
| planner | Implementierungsplanung | Komplexe Features, Refactoring |
| architect | Systemdesign | Architekturentscheidungen |
| tdd-guide | Testgetriebene Entwicklung | Neue Features, Bug-Fixes |
| code-reviewer | Code-Review | Nach dem Schreiben von Code |
| security-reviewer | Sicherheitsanalyse | Vor dem Committen |
| build-error-resolver | Build-Fehler beheben | Wenn Build fehlschlaegt |
| e2e-runner | End-to-End-Tests | Kritische Benutzerflows |
| refactor-cleaner | Toten Code bereinigen | Code-Wartung |
| doc-updater | Dokumentation aktualisieren | Docs aktualisieren |
| rust-reviewer | Rust Code-Review | Rust-Projekte |
| harmonyos-app-resolver | HarmonyOS App-Entwicklung | HarmonyOS/ArkTS-Projekte |

## Sofortige Agenten-Verwendung

Keine Benutzeraufforderung erforderlich:
1. Komplexe Feature-Anfragen → **planner** Agent verwenden
2. Code gerade geschrieben/geaendert → **code-reviewer** Agent verwenden
3. Bug-Fix oder neues Feature → **tdd-guide** Agent verwenden
4. Architekturentscheidung → **architect** Agent verwenden

## Parallele Task-Ausfuehrung

Fuer unabhaengige Operationen IMMER parallele Task-Ausfuehrung verwenden:

```markdown
# GUT: Parallele Ausfuehrung
3 Agenten parallel starten:
1. Agent 1: Sicherheitsanalyse des Auth-Moduls
2. Agent 2: Performance-Review des Cache-Systems
3. Agent 3: Typpruefung der Utilities

# SCHLECHT: Unnoetige sequentielle Ausfuehrung
Erst Agent 1, dann Agent 2, dann Agent 3
```

## Multi-Perspektiven-Analyse

Fuer komplexe Probleme Split-Role Sub-Agents verwenden:
- Fakten-Pruefer
- Senior-Ingenieur
- Sicherheitsexperte
- Konsistenz-Pruefer
- Redundanz-Checker