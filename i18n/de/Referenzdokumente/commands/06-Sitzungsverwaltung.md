# Sitzungsverwaltungs-Befehle

## Ueberblick

Sitzungsverwaltungs-Befehle werden verwendet um Claude Code-Sitzungsstaende zu speichern, wiederherzustellen und zu verwalten. Diese Befehle sind entscheidend fuer das Beibehalten von Kontext in langen Sitzungen, fuer Uebergabe von Arbeit zwischen Sitzungen und fuer Fortschrittsverfolgung.

## Befehlsliste

### /save-session

**Zweck**: Sitzungszustand speichern - Den vollstaendigen Kontext der aktuellen Sitzung in eine Datumsdatei schreiben

**Beschreibung**: Speichert den gesamten Kontext der aktuellen Sitzung in eine Datumsdatei im Verzeichnis `~/.claude/session-data/`, einschliesslich: was gebaut wurde, was funktioniert hat, was fehlgeschlagen ist, verbleibende Arbeit, Dateizustand, getroffene Entscheidungen und offene Blocker.

**Sitzungsdatei-Benennungskonvention**:
- Format: `YYYY-MM-DD-<short-id>-session.tmp`
- Gueltige Zeichen: Buchstaben `a-z`/`A-Z`, Ziffern `0-9`, Bindestrich `-`, Unterstrich `_`
- Minimale Laenge: 1 Zeichen (aber nicht empfehlenswert zu kurz)
- Empfohlen: 8+ Zeichen mit Kleinbuchstaben/Ziffern/Bindestrich-Kombination um Konflikte am selben Tag zu vermeiden

**Verwendungszeitpunkte**:
- Bei Arbeitsende (vor Sitzungsende)
- Bevor Kontextlimit erreicht wird (zuerst speichern, dann neue Sitzung starten)
- Nach dem Loesen komplexer Probleme (erfolgreiche Muster speichern)
- Wenn an zukuenftige Sitzungen uebergeben werden muss

**Workflow**:
1. **Kontext sammeln** - Alle modifizierten Dateien lesen, Diskussion ueberpruefen
2. **Verzeichnis erstellen** - `mkdir -p ~/.claude/session-data`
3. **Datei schreiben** - Sitzungsdatei mit Zeitstempel erstellen
4. **Dem Benutzer anzeigen** - Inhalt anzeigen und Bestaetigung anfordern

**Sitzungsdateiformat**:

```markdown
# Session: YYYY-MM-DD

**Started:** [approximate time if known]
**Last Updated:** [current time]
**Project:** [project name or path]
**Topic:** [one-line summary of what this session was about]

---

## What We Are Building
[1-3 paragraphs describing the feature, bug fix, or task]

## What WORKED (with evidence)
- **[thing that works]** — confirmed by: [specific evidence]

## What Did NOT Work (and why)
- **[approach tried]** — failed because: [exact reason / error message]

## What Has NOT Been Tried Yet
- [approach / idea]

## Current State of Files
| File              | Status         | Notes                      |
| ----------------- | -------------- | -------------------------- |
| `path/to/file.ts` | PASS: Complete | [what it does]             |

## Decisions Made
- **[decision]** — reason: [why this was chosen over alternatives]

## Blockers & Open Questions
- [blocker / open question]

## Exact Next Step
[The single most important thing to do when resuming]
```

**Schluesselprinzipien**:
- Der "What Did NOT Work"-Abschnitt ist der WICHTIGSTE - zukuenftige Sitzungen werden fehlgeschlagene Ansätze blind erneut versuchen
- Jede Sitzung ist eine eigenständige Datei - niemals an vorherige Sitzungen anhängen
- Dateien werden fuer die naechste Sitzung via `/resume-session` gelesen

**Best Practices**:
- Bei Zwischen-Speicherung (nicht bei Ende), laufende Projekte klar markieren
- Spezifische Fehlerinformationen und Fehlergruende einschliessen ("threw X error because Y")
- Der naechste konkrete Schritt sollte praezise genug sein um ohne Nachdenken zu wissen wo zu beginnen

---

### /resume-session

**Zweck**: Gespeicherte Sitzung wiederherstellen

**Beschreibung**: Laedt und stellt einen zuvor gespeicherten Sitzungskontext aus `~/.claude/session-data/` wieder her.

**Anwendungsszenarien**:
- Beim Start einer neuen Sitzung
- Wenn frühere Arbeit fortgesetzt werden muss
- Wenn fruehere Problemlösungsprozesse reviewed werden müssen

---

### /sessions

**Zweck**: Durchsuchen+Suchen+Verwalten der Sitzungshistorie

**Beschreibung**: Durchsucht, sucht und verwaltet alle gespeicherten Sitzungshistorien.

**Funktionen**:
- Alle Sitzungen auflisten
- Nach Datum suchen
- Sitzungsaliasse verwalten
- Alte Sitzungen bereinigen

---

### /checkpoint

**Zweck**: Workflow-Prüfpunkte erstellen/validieren

**Beschreibung**: Erstellt Prüfpunkte an kritischen Workflow-Knotenpunkten um Fortschritt nachvollziehbar zu machen.

**Anwendungsszenarien**:
- Nach Abschluss wichtiger Schritte
- Wenn kritische Meilensteine validiert werden müssen
- Bei mehrstufigen Aufgaben

---

### /aside

**Zweck**: Seitenfrage beantworten ohne Kontext zu verlieren

**Beschreibung**: Wechselt in einen seitlichen Dialogmodus, bearbeitet Nebenfragen und kehrt nahtlos zur Hauptarbeit zurück.

**Anwendungsszenarien**:
- Wenn eine schnelle Frage gestellt werden muss
- Wenn Dokumentation nachgeschlagen werden muss
- Wenn Nebenaufgaben ausgeführt werden müssen

---

## Zugehoerige Befehle

- `/save-session` - Aktuelle Sitzung speichern
- `/resume-session` - Sitzung wiederherstellen
- `/sessions` - Sitzungshistorie verwalten