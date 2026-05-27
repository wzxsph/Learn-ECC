# Test-Agenten

Test-Agenten sind spezialisiert auf testgetriebene Entwicklung, End-to-End-Tests, Testabdeckungsanalyse und Qualitaetssicherung.

## Agent-Liste

| Agent-Name | Verwendung | Verwendetes Modell | Kerntools |
|------------|------------|---------------------|------------|
| tdd-guide | TDD testgetriebene Entwicklung | sonnet | Read, Write, Edit, Bash, Grep |
| e2e-runner | End-to-End-Test-Experte | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| mle-reviewer | ML-Engineering-Code-Review (Training/Inferenz/Monitoring) | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | PR-Testabdeckungsanalyse | sonnet | Read, Grep, Glob, Bash |

---

## tdd-guide

### Name und Verwendung
Testgetriebene Entwicklungsexperte, der die Test-first-Methodik erzwingt. Proaktiv verwendet beim Schreiben neuer Funktionen, Beheben von Bugs oder Refaktorieren von Code. Stellt 80%+ Testabdeckung sicher.

### Faehigkeiten
- Erzwingung der Test-first-Methodik
- Anleitung durch den Rot-Gruen-Refaktor-Zyklus
- Sicherstellung von 80%+ Testabdeckung
- Schreiben umfassender Test-Suiten (Unit, Integration, E2E)
- Erfassung von Randfaellen vor der Implementierung

### Anwendungsgebiete
- Beim Schreiben neuer Funktionen
- Beim Beheben von Bugs
- Beim Refaktorieren von Code
- Wenn TDD-Workflow benoetigt wird

### Verwendete Tools
- Read: Vorhandenen Code lesen
- Write: Tests schreiben
- Edit: Tests und Implementierung aendern
- Bash: Testbefehle ausfuehren
- Grep: Testmuster suchen

### Zusammenarbeit mit anderen Agenten
- code-reviewer prueft Code nach dem TDD-Zyklus
- build-error-resolver behebt Test-Build-Fehler
- e2e-runner fuehrt E2E-Tests fuer kritische Benutzerreisen durch

### TDD-Workflow

#### 1. Test zuerst schreiben (ROT)
Einen fehlschlagenden Test schreiben, der das erwartete Verhalten beschreibt.

#### 2. Test ausfuehren - Verifizieren dass er FAILTS
```bash
npm test
```

#### 3. Minimale Implementierung schreiben (GRUEN)
Nur genug Code schreiben damit der Test durchlaeuft.

#### 4. Test ausfuehren - Verifizieren dass er BESTANDEN hat

#### 5. Refaktorieren (VERBESSERN)
Duplikate entfernen, Namen verbessern, optimieren - Tests müssen gruen bleiben.

#### 6. Abdeckung verifizieren
```bash
npm run test:coverage
# Anforderung: 80%+ Branch-, Funktions-, Zeilen-, Anweisungsabdeckung
```

### Must-Test-Randfaelle

1. **Null/Undefined**-Eingabe
2. **Leere** Arrays/Strings
3. **Ungueltiger Typ** übergeben
4. **Grenzwerte** (Minimum/Maximum)
5. **Fehlerpfade** (Netzwerkfehler, Datenbankfehler)
6. **Race Conditions** (nebenlaeufige Operationen)
7. **Grosse Datenmengen** (10k+ Elemente Leistung)
8. **Sonderzeichen** (Unicode, Emojis, SQL-Zeichen)

### Anti-Pattern - Müssen vermieden werden

- Testen von Implementierungsdetails (interner Zustand) statt Verhalten
- Tests die voneinander abhängen (geteilter Zustand)
- Zu wenige Behauptungen (durchlassen aber nichts verifizieren)
- Externe Abhaengigkeiten nicht gemockt (Supabase, Redis, OpenAI usw.)

### Qualitaets-Pruefliste

- [ ] Alle öffentlichen Funktionen haben Unit-Tests
- [ ] Alle API-Endpunkte haben Integrationstests
- [ ] Kritische Benutzerreisen haben E2E-Tests
- [ ] Randfaelle abgedeckt (null, leer, ungueltig)
- [ ] Fehlerpfade getestet (nicht nur Happy Path)
- [ ] Externe Abhaengigkeiten gemockt
- [ ] Tests unabhaengig (kein geteilter Zustand)
- [ ] Behauptungen spezifisch und aussagekräftig
- [ ] Abdeckung 80%+

### Anhang: v1.8 Eval-Driven TDD

Eval-Driven Entwicklung in den TDD-Prozess integrieren:

1. Faehigkeiten + Regression-Evals vor der Implementierung definieren
2. Baseline ausfuehren und Fail-Merkmale erfassen
3. Minimale durchlaufende Änderung implementieren
4. Tests und Evals erneut ausfuehren; pass@1 und pass@3 berichten
5. Kritische Pfade sollten vor dem Merge pass^3-Stabilitaet erreichen

---

## e2e-runner

### Name und Verwendung
End-to-End-Test-Experte, der Vercel Agent Browser (bevorzugt) und Playwright-Fallback verwendet. Proaktiv verwendet zum Erstellen, Warten und Ausfuehren von E2E-Tests.

### Faehigkeiten
- Testreisen-Erstellung - Tests fuer Benutzerfluesse schreiben
- Testwartung - Tests synchron mit UI-Änderungen halten
- Instabile Testverwaltung - Instabile Tests identifizieren und isolieren
- Testberichte - HTML-Berichte und JUnit-XML generieren
- CI/CD-Integration - Sicherstellen dass Tests zuverlaessig in Pipelines laufen
- Screenshot/Video/Trace-Verwaltung

### Anwendungsgebiete
- Wenn kritische Benutzerreisen verifiziert werden müssen
- Wenn End-to-End-Verifizierung benoetigt wird
- Wenn E2E-Tests in CI/CD-Pipelines ausgefuehrt werden
- Wenn Integrationsprobleme gefunden werden

### Verwendete Tools
- Read: Seiteninhalt lesen
- Write: Tests schreiben
- Edit: Tests aendern
- Bash: Playwright/Agent-Browser-Befehle ausfuehren
- Grep: Tests suchen
- Glob: Testdateien finden

### Zusammenarbeit mit anderen Agenten
- tdd-guide schreibt Unit/Integrationstests
- code-reviewer prueft Testqualitaet
- mle-reviewer prueft ML-bezogene Tests

### Hauptwerkzeug: Agent Browser

**Bevorzugt gegenüber Raw Playwright verwenden** - Semantische Selektoren, KI-optimiert, automatisch wartend, basierend auf Playwright.

```bash
# Setup
npm install -g agent-browser && agent-browser install

# Kernworkflow
agent-browser open https://example.com
agent-browser snapshot -i          # Elemente mit refs erhalten [ref=e1]
agent-browser click @e1            # Per ref klicken
agent-browser fill @e2 "text"     # Per ref ausfuellen
agent-browser wait visible @e5     # Auf Element sichtbar warten
agent-browser screenshot result.png
```

### Fallback: Playwright

Wenn Agent Browser nicht verfuegbar ist, Playwright direkt verwenden.

```bash
npx playwright test                        # Alle E2E-Tests ausfuehren
npx playwright test tests/auth.spec.ts     # Bestimmte Datei ausfuehren
npx playwright test --headed               # Sichtbarer Browser
npx playwright test --debug                # Mit Inspector debuggen
npx playwright test --trace on             # Mit Trace ausfuehren
npx playwright show-report                 # HTML-Bericht anzeigen
```

### Workflow

#### 1. Planen
- Kritische Benutzerreisen identifizieren (Authentifizierung, Kernfunktionen, Zahlung, CRUD)
- Szenarien definieren: Happy Path, Randfaelle, Fehlerfaelle
- Nach Risiko priorisieren: HOCH (Finanzen, Auth), MITTEL (Suche, Navigation), NIEDRIG (UI-Optimierung)

#### 2. Erstellen
- Page Object Model (POM)-Pattern verwenden
- `data-testid`-Selektoren bevorzugen
- An wichtigen Schritten Behauptungen hinzufuegen
- An kritischen Punkten Screenshots erfassen
- Entsprechende Wartezeiten verwenden (niemals `waitForTimeout`)

#### 3. Ausfuehren
- Lokal 3-5 Mal ausfuehren um Instabilitaet zu pruefen
- Instabile Tests mit `test.fixme()` oder `test.skip()` isolieren
- Artifact in CI hochladen

### Kernprinzipien

- **Semantische Selektoren verwenden**: `[data-testid="..."]` > CSS-Selektoren > XPath
- **Auf Bedingungen warten statt Zeit**: `waitForResponse()` > `waitForTimeout()`
- **Automatisch warten eingebaut**: `page.locator().click()` wartet automatisch; Raw `page.click()` wartet nicht
- **Tests isolieren**: Jeder Test sollte unabhaengig sein; Kein geteilter Zustand
- **Schnell scheitern**: An jedem kritischen Schritt `expect()`-Behauptungen verwenden
- **Retry mit Trace**: Konfiguration `trace: 'on-first-retry'` fuer Debugging von Failures verwenden

### Instabile Testbehandlung

```typescript
// Isolieren
test('flaky: market search', async ({ page }) => {
  test.fixme(true, 'Flaky - Issue #123')
})

// Instabilitaet identifizieren
// npx playwright test --repeat-each=10
```

Haeufige Ursachen: Race Conditions (automatisch wartende Selektoren verwenden), Netzwerk-Timing (auf Antwort warten), Animation-Timing (auf `networkidle` warten).

### Erfolgsmetriken

- Alle kritischen Reisen bestanden (100%)
- Gesamt-Bestehensrate > 95%
- Instabilitaetsrate < 5%
- Testdauer < 10 Minuten
- Artifact hochgeladen und zugaenglich

---

## mle-reviewer

### Name und Verwendung
Produktions-Machine-Learning-Engineering-Reviewer, spezialisiert auf Datenvertraege, Feature-Pipelines, Trainingsreproduzierbarkeit, Offline/Online-Bewertung, Modell-Serving, Monitoring und Rollback.

### Faehigkeiten
- Problemdefinition und Entscheidungsqualitaets-Review
- Metriken, Schwellenwerte und Fehleranalyse-Review
- Datenvertraege und Leak-Pruefungen
- Trainingsreproduzierbarkeits-Validierung
- Bewertungs- und Foerderungsprozess-Review
- Serving- und Deployment-Sicherheitsreview
- Monitoring- und Vorfallreaktionsplanung

### Anwendungsgebiete
- Bei ML-, MLOps-, Modell-Trainings-Codeaenderungen
- Bei Inferenz-, Feature-Store-, Bewertungs-Codeaenderungen
- Bei Modellfoerderungsentscheidungen

### Verwendete Tools
- Read: ML-Code und Konfiguration lesen
- Grep: Muster suchen
- Glob: Dateien finden
- Bash: pytest, ruff, mypy ausfuehren

### Zusammenarbeit mit anderen Agenten
- python-reviewer behandelt Python-Stil, Typen, Fehlerbehandlung
- pytorch-build-resolver behandelt Tensor/CUDA/Training-Fails
- database-reviewer behandelt Feature-Tabellen, Label-Speicher
- security-reviewer behandelt Secrets, PII, Pickle-Sicherheit
- performance-optimizer behandelt Latenz, Speicher, GPU-Auslastung
- build-error-resolver behandelt CI, Abhaengigkeiten, Native-Extension-Fails
- pr-test-analyzer analysiert Testabdeckung
- silent-failure-hunter findet Pipeline-stille Fails
- e2e-runner fuehrt Produktfluss-E2E-Tests durch
- a11y-architect prueft Vorhersage-Barrierefreiheit
- doc-updater aktualisiert Dokumentation

### Wichtige Review-Bereiche

#### Problemdefinition und Entscheidungsqualitaet
- Ändert sich die Loesung von Benutzer- oder Systementscheidungen, nicht von Modellarchitekturpraeferenzen?
- Sind Stakeholder und Failure-Kosten klar?
- Folgt die Metrik-Auswahl dem Fehlerbudget?

#### Metriken, Schwellenwerte und Fehleranalyse
- Werden Baseline und aktuelles Produktionsverhalten verglichen?
- Sind Schwellenwerte und Konfiguration als Produktentscheidungen?
- Werden False Positives und False Negatives direkt geprueft?

#### Datenvertraege und Leaks
- Sind Entity-Granularitaet, Primaerschluessel, Zeitstempel klar?
- Halten Splits Zeit-, Benutzer/Entity-Gruppierungen ein?
- Sind Feature-Joins zeitpunkt-korrekt?
- Sind sensible Attribute ausgeschlossen oder rechtmaessig behandelt?

#### Trainingsreproduzierbarkeit
- Kann das Training aus Code, Konfiguration, Datensatzversion, Seed ausgefuehrt werden?
- Sind Hyperparameter, Vorverarbeitung, Abhaengigkeitsversionen dokumentiert?
- Sind Zufallsaerung und GPU-Nichtdeterminismus absichtlich behandelt?

#### Bewertung und Foerderung
- Werden Metriken mit Baseline und aktuellem Produktionsmodell verglichen?
- Sind Foerderungstore vor Auswahl deklariert?
- Decken Slice-Metriken wichtige Warteschlangen ab?

#### Serving und Deployment
- Werden Trainings- und Serving-Transformationen geteilt oder äquivalent getestet?
- Lehnt das Eingabeschema veraltete, fehlende, ungueltige Features ab?
- Haben Inferenzpfade Timeout, Ressourcenlimit, Fallback-Logik?

#### Monitoring und Vorfallreaktion
- Decken Metriken Service-Gesundheit, Feature-Drift, Vorhersage-Drift ab?
- Enthalten Logs ausreichend Identifikatoren?
- Benennt Rollback vorherige Artifact, Konfiguration, Datenabhaengigkeiten?

### Haeufige Blocker

- Zufaellige Train/Test-Splits auf zeitbezogenen oder benutzerbezogenen Daten
- Feature-Generierung verwendet Felder die zum Vorhersagezeitpunkt nicht verfuegbar sind
- Offline-Metriken verbessern sich während kritische Slices zurueckgehen
- Trainingsvorverarbeitung wird manuell in Serving-Code dupliziert
- Modellversion nicht in Vorhersage-Logs

### Genehmigungsstandards

- **APPROVE**: Keine kritischen/hohen MLE-Risiken, relevante Tests oder Bewertungstore bestanden
- **APPROVE WITH WARNINGS**: Nur mittlere Probleme mit klarer Folgemaßnahme
- **BLOCK**: Jegliches mögliches Leak, nicht reproduzierbare Foerderung, unsicheres Serving-Verhalten, fehlender Produktions-Deployment-Rollback, Sensible-Daten-Exposure oder kritische Bewertungsluecken

---

## pr-test-analyzer

### Name und Verwendung
Prueft die Qualitaet und Vollstaendigkeit der Testabdeckung von Pull Requests, mit Betonung auf Verhaltensabdeckung und echter Bug-Praevention.

### Faehigkeiten
- Änderungscode-Zuordnung identifizieren
- Verhaltensabdeckung validieren
- Testqualitaet bewerten
- Abdeckungsluecken finden

### Anwendungsgebiete
- Bei PR-Reviews
- Bei Bewertung der Testabdeckung
- Bei Verifizierung der Testsufficient

### Verwendete Tools
- Read: Geaenderten Code und Tests lesen
- Grep: Tests suchen
- Glob: Testdateien finden
- Bash: Testbefehle ausfuehren

### Zusammenarbeit mit anderen Agenten
- tdd-guide verbessert Tests
- e2e-runner fuehrt End-to-End-Abdeckung durch
- code-reviewer fuehrt Codequalitaetsreview durch

### Analyseprozess

#### 1. Änderungscode identifizieren
- Geaenderte Funktionen, Klassen und Module abbilden
- Zugehoerige Tests lokalisieren
- Neue ungetestete Codepfade identifizieren

#### 2. Verhaltensabdeckung
- Prüfen ob jede Funktion Tests hat
- Randfaelle und Fehlerpfade verifizieren
- Sicherstellen dass wichtige Integrationen abgedeckt sind

#### 3. Testqualitaet
- Bevorzugen aussagekraeftige Behauptungen statt keine-Throwing-Checks
- Instabile Pattern markieren
- Isolation und Testnamenklarheit pruefen

#### 4. Abdeckungsluecken
Nach Auswirkungsbewertung:
- critical (kritisch)
- important (wichtig)
- nice-to-have (nice-to-have)

### Ausgabeformat

1. Abdeckungszusammenfassung
2. Kritische Luecken
3. Verbesserungsvorschlaege
4. Positive Beobachtungen
[Zurueck zum Agent-Index](../README.md)