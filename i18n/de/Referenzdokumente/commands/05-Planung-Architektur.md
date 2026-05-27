# Planungs- und Architektur-Befehle

## Ueberblick

Planungs- und Architektur-Befehle werden fuer Produktplanung, Funktionsdesign und Systemarchitektur-Entscheidungen verwendet. Diese Befehle stellen sicher dass klare Plaene vorhanden sind BEVOR Code geschrieben wird, reduzieren Nacharbeit und verbessern die Codequalitaet.

## Befehlsliste

### /plan

**Zweck**: Universelle Implementierungsplanung - Anforderungen umformulieren, Risiken bewerten, schrittweisen Implementierungsplan erstellen

**Beschreibung**: Erstellt umfassenden Implementierungsplan und wartet auf Benutzerbestaetigung BEVOR Code beruehrt wird. Akzeptiert Freiform-Anforderungen oder PRD-Markdown-Dateien.

**Eingabemodi**:

| Eingabe | Modus | Verhalten |
|---|---|---|
| `path/to/name.prd.md` | PRD artifact-Modus | PRD lesen, naechsten ausstehenden Liefer-Meilenstein auswaehlen, `.claude/plans/{name}.plan.md` generieren |
| Andere Markdown-Pfade | Referenz-Modus | Datei als Kontext lesen, Inline-Plan generieren |
| Freiform-Text | Dialog-Modus | Inline-Plan generieren |
| Leere Eingabe | Klaerungs-Modus | Fragen was geplant werden soll |

**Schluesselparameter**:
- `$ARGUMENTS`: `[feature description | path/to/*.prd.md]` - Funktionsbeschreibung oder PRD-Dateipfad

**Workflow**:
1. **Anforderungen umformulieren** - Klarstellen was gebaut werden muss
2. **Risiken identifizieren** - Potenzielle Probleme und Blocker auflisten
3. **Schrittweisen Plan erstellen** - In phasenweise Implementierung aufteilen
4. **Auf Bestaetigung warten** - Muss Benutzerfreigabe erhalten bevor fortgefahren wird

**Ausgabeformat** (PRD artifact-Modus):
```markdown
# Plan: {Feature Name}

**Source PRD**: {path}
**Selected Milestone**: {milestone or phase name}
**Complexity**: {Small | Medium | Large}

## Summary
{2-3 sentences}

## Patterns to Mirror
| Category | Source | Pattern |
|---|---|---|
| Naming | `path:line` | {short description} |
| Errors | `path:line` | {short description} |
| Tests | `path:line` | {short description} |

## Files to Change
| File | Action | Why |
|---|---|---|
| `path` | CREATE / UPDATE / DELETE | {reason} |

## Tasks
### Task 1: {name}
- **Action**: {what to do}
- **Mirror**: {pattern to follow}
- **Validate**: {command that proves correctness}

## Validation
```bash
{project-specific validation commands}
```

## Risks
| Risk | Likelihood | Mitigation |
|---|---|---|
```

**Best Practices**:
- Recherche vorhandene Muster im Codebase VOR Verwendung
- Im PRD artifact-Modus `.claude/plans/` Verzeichnis erstellen falls nicht vorhanden
- Wenn PRD eine `Delivery Milestones`-Tabelle enthaelt, nur ausgewaehlte Zeilenstatus aktualisieren

**Haeufige Fallstricke**:
- Keine Klaerung erfragen wenn Eingabe leer ist
- Nicht auf Benutzerbestaetigung warten bevor Code geschrieben wird
- Pattern-Grounding-Schritt ueberspringen

**Integration mit anderen Befehlen**:
- Nach Planung `tdd-workflow` skill fuer testgetriebene Entwicklung verwenden
- `/build-fix` zum Beheben von Build-Fehlern verwenden
- `/code-review` zum Review der fertigen Implementierung verwenden
- `/pr` oder `/prp-pr` zum Oeffnen eines Pull Request verwenden

---

### /plan-prd

**Zweck**: Interaktiver PRD-Generator

**Beschreibung**: Generiert problemzentriertes Produkt-Anforderungsdokument mit Problemdefinition, Benutzer-Personas, Erfolgsmetriken und MVP-Umfang.

**Anwendungsszenarien**:
- Zu bauende Funktion bestimmen
- Produktanforderungen muessen geklaert werden
- Planung eines Features von Grund auf

**Workflow**:
1. FRAME - Problem und Benutzer verstehen
2. GROUND - Belege sammeln
3. DECIDE - Umfang und Annahmen bestimmen
4. GENERATE - PRD-Datei generieren

---

### /prp-plan

**Zweck**: Umfassende Funktionsplanung

**Beschreibung**: Vollstaendige Projektplanung mit Codebase-Analyse, Risikobewertung und schrittweisem Implementierungsplan.

**Anwendungsszenarien**:
- Detaillierte Planung fuer komplexe Funktionen
- Codebase-Kontextanalyse erforderlich
- Mehrphasen-Implementierungsprojekte

---

### /prp-prd

**Zweck**: PRP-Workflow PRD-Generator

**Beschreibung**: Generiert Produkt-Anforderungsdokumente unter Verwendung des PRP (Plan-Record-Produce) Workflows.

---

### /prp-implement

**Zweck**: PRP-Plan-Validierungs-Schleife ausfuehren

**Beschreibung**:Fuehrt den Plan im PRP-Workflow aus mit kontinuierlicher Validierung und Pruefung.

---

### /prp-pr

**Zweck**: PR aus PRP-Workflow erstellen

**Beschreibung**: Erstellt GitHub Pull Request aus den Ergebnissen des PRP-Workflows.

---

### /prp-commit

**Zweck**: PRP-Validierungs-Commit

**Beschreibung**: Fuehrt Validierung aus und committed Code im PRP-Workflow.

---

### /multi-plan

**Zweck**: Multi-Modell-Kollaborationsplanung

**Beschreibung**: Kombiniert Codex- und Gemini-Doppelmodell-Analyse um umfassenden Implementierungsplan zu generieren.

**Anwendungsszenarien**:
- Mehrperspektivenanalyse erforderlich
- Komplexe domaenenuebergreifende Projekte
- Balance zwischen Frontend- und Backend-Ueberlegungen noetig

---

## Zugehoerige Befehle

- `/plan` - Universelle Implementierungsplanung
- `/code-review` - Code-Review
- `/build-fix` - Build-Reparatur