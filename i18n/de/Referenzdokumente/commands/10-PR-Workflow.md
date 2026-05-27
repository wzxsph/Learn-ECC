# Pull-Request-Workflow-Befehle

## Ueberblick

PR-Workflow-Befehle werden verwendet um GitHub Pull Requests zu erstellen, zu verwalten und zu reviewen.

## Befehlsliste

### /pr

**Zweck**: GitHub PR aus aktuellem Branch erstellen, mit nicht gepushten Commits

**Beschreibung**: Entdeckt Templates, analysiert Aenderungen, pusht Branch und erstellt PR. Analysiert vollstaendige Commit-Historie, verwendet `git diff [base-branch]...HEAD` um alle Aenderungen zu sehen.

**Eingabe**: `$ARGUMENTS` - Optional, kann Basis-Branch-Namen und/oder Flags enthalten (wie `--draft`)

**Parsing-Regeln**:
- Alle erkannten Flags extrahieren (wie `--draft`)
- Restlichen Nicht-Flag-Text als Basis-Branch-Namen verwenden
- Wenn nicht angegeben, Standard ist `main`

**Workflow**:

| Phase | Beschreibung |
|---|---|
| **Phase 1: VALIDATE** | Vorbedingungen pruefen (nicht auf base-Branch, keine uncommittierten Aenderungen, Ahead-Commits vorhanden, kein existierender PR) |
| **Phase 2: DISCOVER** | PR-Template suchen, Commits analysieren, Dateiaenderungen pruefen, relevante Planungsprodukte pruefen |
| **Phase 3: PUSH** | Branch puschen (erst rebase wenn nötig) |
| **Phase 4: CREATE** | PR mit Template oder Standardformat erstellen |
| **Phase 5: VERIFY** | ERstellung des PR erfolgreich verifizieren |
| **Phase 6: OUTPUT** | PR-URL und naechste Schritte berichten |

**PR-Erstellungs-Verifizierungspruefungen**:

| Pruefung | Bedingung | Bei Fehler |
|---|---|---|
| Nicht auf base-Branch | Aktueller Branch ≠ base | Stopp: "Erst zu Feature-Branch wechseln" |
| Arbeitsbereich sauber | Keine uncommittierten Aenderungen | Warnung: "Erst committen oder stashen" |
| Ahead-Commits vorhanden | `git log origin/<base>..HEAD` nicht leer | Stopp: "Keine Ahead-Commits" |
| Kein existierender PR | `gh pr list` leer | Stopp: "PR existiert bereits" |

**Template-Suchreihenfolge**:
1. `.github/PULL_REQUEST_TEMPLATE/` Verzeichnis (falls vorhanden)
2. `.github/PULL_REQUEST_TEMPLATE.md`
3. `.github/pull_request_template.md`
4. `docs/pull_request_template.md`

**Standard PR-Format** (ohne Template):
```markdown
## Summary
<1-2 sentence description>

## Changes
<bulleted list grouped by area>

## Files Changed
<table of changed files>

## Testing
<how changes were tested, or "Needs testing">

## Related Issues
<linked issues or "None">
```

**Randfallbehandlung**:
- **Kein `gh` CLI**: Stopp und Installation vorschlagen
- **Nicht authentifiziert**: `gh auth login` vorschlagen
- **Branch abgezweigt**: `git fetch && git rebase` verwenden dann pushen
- **Grosser PR (>20 Dateien)**: Warnung und Aufteilung empfehlen

**Integration mit anderen Befehlen**:
- `/code-review <number>` zum Reviewen des PR verwenden
- `gh pr merge <number>` zum Zusammenfuehren fertiger PRs verwenden
- `/plan-prd` oder `/plan` zum Erstellen relevanter Planungsprodukte verwenden

---

### /review-pr

**Zweck**: GitHub PR oder lokale uncommittierte Aenderungen reviewen

**Beschreibung**: Reviewt Pull Request auf GitHub oder lokale uncommittierte Aenderungen. PR-Review-Modus adaptiert von PRPs-agentic-eng.

**Eingabe**: `$ARGUMENTS` - Kann PR-Nummer, PR-URL oder leer sein (lokaler Review)

---

#### Modus-Auswahl

| Eingabe | Aktion |
|---|---|
| PR-Nummer (wie `42`) | Als PR-Nummer verwenden |
| URL (wie `github.com/.../pull/42`) | PR-Nummer extrahieren |
| Branch-Name | PR ueber `gh pr list --head <branch>` suchen |
| Leer | Lokalen Review-Modus verwenden |

---

#### Lokaler Review-Modus

**Workflow**:
1. **GATHER** - Aenderungen sammeln: `git diff --name-only HEAD`
2. **REVIEW** - Jede Datei reviewen (Sicherheitsprobleme, Qualitaetsprobleme, Best Practices)
3. **REPORT** - Bericht mit Schweregrad und Reparaturempfehlungen generieren

**Review-Checkliste**:

| Kategorie | Pruefungen |
|---|---|
| **Sicherheitsprobleme (CRITICAL)** | Hartcodierte Credentials, SQL-Injection, XSS, fehlende Eingabevalidierung, unsichere Abhaengigkeiten, Path-Traversal |
| **Codequalitaet (HIGH)** | Funktionen>50 Zeilen, Dateien>800 Zeilen, Verschachtelung>4 Ebenen, fehlende Fehlerbehandlung, console.log, TODO/FIXME |
| **Best Practices (MEDIUM)** | Aenderungsmuster, Emoji-Verwendung, neue Code ohne Tests, Barrierefreiheitsprobleme |

**Entscheidungen**:

| Bedingung | Entscheidung |
|---|---|
| CRITICAL oder HIGH Probleme vorhanden | **BLOCK** - Zusammenfuehren verhindern |
| Nur MEDIUM/LOW Probleme | Durchlassen aber mit Kommentaren |
| Keine Probleme und Verifizierung bestanden | **APPROVE** |

---

#### PR-Review-Modus

**Workflow**:
1. **FETCH** - PR-Metadaten und Diff abrufen
2. **CONTEXT** - Review-Kontext aufbauen (Projektregeln, Planungsprodukte, Aenderungsdateien)
3. **REVIEW** - Vollstaendige Dateien lesen, 7 Review-Kategorien anwenden
4. **VALIDATE** - Verifizierungsbefehle fuer Projekttyp ausfuehren
5. **DECIDE** - Empfehlung basierend auf Erkenntnissen bilden
6. **REPORT** - Review-Produkt erstellen
7. **PUBLISH** - Review auf GitHub veroeffentlichen
8. **OUTPUT** - Dem Benutzer berichten

**7 Review-Kategorien**:

| Kategorie | Pruefungen |
|---|---|
| **Correctness** | Logikfehler, Grenzfaelle, Nullbehandlung, Race Conditions |
| **Type Safety** | Typ-Mismatches, unsichere Typkonvertierungen, `any`-Verwendung, fehlende Generics |
| **Pattern Compliance** | Projektkonventionen entsprechen (Namensgebung, Dateistruktur, Fehlerbehandlung, Imports) |
| **Security** | Injection, Authentifizierungsluecken, Secret-Exposition, SSRF, Path-Traversal, XSS |
| **Performance** | N+1-Abfragen, fehlende Indizes, unbegrenzte Schleifen, Speicherlecks, grosse Payloads |
| **Completeness** | Fehlende Tests, fehlende Fehlerbehandlung, unvollstaendige Migrationen, fehlende Dokumentation |
| **Maintainability** | Totcode, magische Zahlen, tiefe Verschachtelung, unklare Benennung, fehlende Typen |

**Verifizierungsbefehl-Erkennung**:

| Projekttyp | Erkennungsdatei | Verifizierungsbefehl |
|---|---|---|
| Node.js/TypeScript | `package.json` | `npm run typecheck`, `npm run lint`, `npm test`, `npm run build` |
| Rust | `Cargo.toml` | `cargo clippy`, `cargo test`, `cargo build` |
| Go | `go.mod` | `go vet ./...`, `go test ./...`, `go build ./...` |
| Python | `pyproject.toml` | `pytest` |

**Entscheidungsstufen**:

| Stufe | Bedeutung | Aktion |
|---|---|---|
| CRITICAL | Sicherheitsluecke oder Datenverlustrisiko | Muss behoben werden um zu mergen |
| HIGH | Bug oder Logikfehler der Probleme verursachen kann | Sollte vor dem Merge behoben werden |
| MEDIUM | Fehlende Codequalitaet oder Best Practices | Reparatur empfohlen |
| LOW | Stilproblem oder kleine Anmerkung | Optional |

**Review auf GitHub veroeffentlichen**:
```bash
# APPROVE
gh pr review <NUMBER> --approve --body "<summary>"

# REQUEST_CHANGES
gh pr review <NUMBER> --request-changes --body "<summary with required fixes>"

# COMMENT only (Draft-PR oder informativ)
gh pr review <NUMBER> --comment --body "<summary>"
```

---

### /multi-workflow (in Bearbeitung)

> (In Bearbeitung) Befehlsdetails folgen

**Zweck**: Multi-Modell-Kollaborationsentwicklung

**Beschreibung**: Koordiniert mehrere AI-Modelle fuer kollaborative Entwicklung.

---

### /multi-backend (in Bearbeitung)

> (In Bearbeitung) Befehlsdetails folgen

**Zweck**: Backend Multi-Modell-Entwicklung

**Beschreibung**: Multi-Modell-Kollaboration mit Fokus auf Backend-Entwicklung.

---

### /multi-frontend (in Bearbeitung)

> (In Bearbeitung) Befehlsdetails folgen

**Zweck**: Frontend Multi-Modell-Entwicklung

**Beschreibung**: Multi-Modell-Kollaboration mit Fokus auf Frontend-Entwicklung.

---

### /multi-execute (in Bearbeitung)

> (In Bearbeitung) Befehlsdetails folgen

**Zweck**: Multi-Modell-Kollaborationsausfuehrung

**Beschreibung**: Koordiniert mehrere Modelle fuer parallele Aufgabenausfuehrung.

---

## Zugehoerige Befehle

- `/pr` - PR erstellen
- `/review-pr` - PR reviewen
- `/multi-workflow` - Multi-Modell-Kollaboration