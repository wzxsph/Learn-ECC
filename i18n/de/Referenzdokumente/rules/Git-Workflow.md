# Git-Workflow

## Commit-Nachrichtenformat

```
<type>: <Beschreibung>

<optional body>
```

**Typ (type)**:
- `feat`: Neues Feature
- `fix`: Bug-Fix
- `refactor`: Refactoring
- `docs`: Dokumentation
- `test`: Tests
- `chore`: Sonstiges
- `perf`: Performance-Optimierung
- `ci`: CI/CD

> Hinweis: Attribution ist ueber `~/.claude/settings.json` global deaktiviert

## Pull-Request-Workflow

Beim Erstellen eines PRs:
1. Vollstaendige Commit-Historie analysieren (nicht nur den neuesten Commit)
2. `git diff [base-branch]...HEAD` verwenden um alle Aenderungen zu sehen
3. Umfassende PR-Zusammenfassung erstellen
4. Testplan mit TODOs einschliessen
5. Bei neuem Branch mit `-u` Flag pushen


## Branch-Benennung

Standard git flow Branch-Benennung verwenden:
- `feature/`: Neue Features
- `fix/`: Fixes
- `refactor/`: Refactoring
- `docs/`: Dokumentation

## Entwicklungsprozess

Vollstaendiger Entwicklungsprozess:
1. **Recherche & Wiederverwendung** - GitHub-Suche, Bibliotheksdocs, Paket-Registries
2. **Planung** - planner Agent verwenden um Implementierungsplan zu erstellen
3. **TDD-Ansatz** - Tests zuerst schreiben, dann implementieren
4. **Code-Review** - code-reviewer Agent verwenden
5. **Commit & Push** - Detaillierte Commit-Nachrichten, Conventional Commits befolgen