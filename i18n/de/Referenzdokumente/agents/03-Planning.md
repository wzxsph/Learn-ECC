# Planungs-Agenten

Planungs-Agenten sind spezialisiert auf Funktionsplanung, Technologie-Architekturdesign und Implementierungsschritt-Zerlegung.

## Agent-Liste

| Agent-Name | Verwendung | Verwendetes Modell | Kerntools |
|------------|------------|---------------------|------------|
| planner | Komplexer Funktionsimplementierungs-Planungsexperte | opus | Read, Grep, Glob |
| code-architect | Funktionsarchitektur-Designexperte | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | PR-Testabdeckungsanalyse | sonnet | Read, Grep, Glob, Bash |
| type-design-analyzer | Typdesignanalyse | sonnet | Read, Grep, Glob |
| comment-analyzer | Code-Kommentar-Analyse | sonnet | Read, Grep, Glob |

---

## planner

### Name und Verwendung
Planungsexperte fuer komplexe Funktionsimplementierungen und Refactorings. Wird proaktiv verwendet wenn der Benutzer eine Funktionsimplementierung, Architekturänderung oder komplexes Refactoring anfordert.

### Faehigkeiten
- Anforderungsanalyse und detaillierte Implementierungsplanerstellung
- Zerlegung komplexer Funktionen in handhabbare Schritte
- Identifizierung von Abhaengigkeiten und potenziellen Risiken
- Empfehlung der optimalen Implementierungsreihenfolge
- Beruecksichtigung von Randfaellen und Fehlerszenarien

### Anwendungsgebiete
- Planung neuer Funktionsimplementierungen
- Architekturänderungsplanung
- Komplexe Refactoring-Planung
- Technische Schuldenbereinigungs-Planung

### Verwendete Tools
- Read: Vorhandenen Code und Dokumentation lesen
- Grep: Relevante Muster und vorhandene Implementierungen suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- Generierte Plaene werden von code-architect weiter verfeinert
- Generierte Plaene werden von tdd-guide zum Schreiben von Tests verwendet
- Generierte Plaene werden von code-reviewer zum Pruefen verwendet

### Planungsprozess

1. **Anforderungsanalyse**
   - Funktionelle Anforderung vollstaendig verstehen
   - Klaerungsfragen stellen wenn noetig
   - Erfolgsstandards identifizieren
   - Annahmen und Einschraenkungen auflisten

2. **Architektur-Review**
   - Bestehende Codebasisstruktur analysieren
   - Betroffene Komponenten identifizieren
   - Aehnliche Implementierungen ueberpruefen
   - Wiederverwendbare Muster beruecksichtigen

3. **Schritt-Zerlegung**
   - Detaillierte Schritte erstellen, einschliesslich:
     - Klare, spezifische Aktionen
     - Dateipfade und Positionen
     - Abhaengigkeiten zwischen Schritten
     - Geschaetzte Komplexitaet
     - Potenzielle Risiken

4. **Implementierungsreihenfolge**
   - Nach Abhaengigkeiten sortieren
   - Zusammengehoerige Aenderungen gruppieren
   - Kontextwechsel minimieren
   - Inkrementelles Testen unterstuetzen

### Planungsausgabeformat

```markdown
# Implementierungsplan: [Funktionsname]

## Uebersicht
[2-3 Saetze zusammenfassen]

## Anforderungen
- [Anforderung 1]
- [Anforderung 2]

## Architekturänderungen
- [Änderung 1: Dateipfad und Beschreibung]
- [Änderung 2: Dateipfad und Beschreibung]

## Implementierungsschritte

### Phase 1: [Phasenname]
1. **[Schrittname]** (Datei: path/to/file.ts)
   - Aktion: Spezifische auszufuehrende Aktion
   - Grund: Warum dieser Schritt notwendig ist
   - Abhaengigkeit: Keine / Erfordert Schritt X
   - Risiko: Niedrig/Mittel/Hoch

2. **[Schrittname]** (Datei: path/to/file.ts)
   ...

### Phase 2: [Phasenname]
...

## Teststrategie
- Unit-Tests: [Zu testende Dateien]
- Integrationstests: [Zu testende Flows]
- E2E-Tests: [Zu testende Benutzerreisen]

## Risiken und Mitigation
- **Risiko**: [Beschreibung]
  - Mitigation: [Wie damit umgehen]

## Erfolgsstandards
- [ ] Standard 1
- [ ] Standard 2
```

---

## code-architect

### Name und Verwendung
Designt Funktionsarchitektur basierend auf tiefem Verstaendnis der vorhandenen Codebasis. Bietet Implementierungsblaupausen mit konkreten Dateien, Schnittstellen, Datenfluesssen und Build-Reihenfolgen.

### Faehigkeiten
- Erforschung vorhandener Codeorganisation und Namenskonventionen
- Identifizierung vorhandener Architekturpattern
- Dokumentation von Testpattern und vorhandenen Grenzen
- Verstehen der Abhaengigkeitsgraphen bevor neue Abstraktionen vorgeschlagen werden
- Design von Funktionsarchitektur die zum aktuellen Muster passt

### Anwendungsgebiete
- Design neuer Funktionsarchitektur
- Design fuer Refactoring vorhandener Funktionen
- Codeorganisationsoptimierung

### Verwendete Tools
- Read: Vorhandenen Code lesen
- Grep: Muster und Konventionen suchen
- Glob: Zugehoerige Dateien finden
- Bash: Diagnosebefehle ausfuehren

### Zusammenarbeit mit anderen Agenten
- Architekturdesign basierend auf dem Plan des planners
- Blaupausen fuer Codeimplementierung bereitstellen
- Zusammenarbeit mit build-error-resolver zur Behebung von Build-Problemen

### Designprozess

1. **Musteranalyse**
   - Erforschung vorhandener Codeorganisation und Namenskonventionen
   - Identifizierung vorhandener Architekturpattern
   - Beachtung von Testpattern und vorhandenen Grenzen
   - Abhaengigkeitsgraphen verstehen bevor neue Abstraktionen vorgeschlagen werden

2. **Architekturdesign**
   - Design von Funktionen die natuerlich zum aktuellen Muster passen
   - Waehlen der einfachsten Architektur die die Anforderungen erfuellt
   - Vermeidung von spekulativer Abstraktion (ausser Codebasis verwendet sie bereits)

3. **Implementierungsblaupause**
   Fuer jede wichtige Komponente bereitstellen:
   - Dateipfad
   - Zweck
   - Wichtige Schnittstellen
   - Abhaengigkeiten
   - Datenfluss-Rolle

4. **Build-Reihenfolge**
   Implementierung nach Abhaengigkeiten sortiert:
   1. Typen und Schnittstellen
   2. Kernlogik
   3. Integrationsschicht
   4. UI
   5. Tests
   6. Dokumentation

### Ausgabeformat

```markdown
## Architektur: [Funktionsname]

### Designentscheidungen
- Entscheidung 1: [Begruendung]
- Entscheidung 2: [Begruendung]

### Zu erstellende Dateien
| Datei | Zweck | Prioritaet |
|------|------|------------|
| | | |

### Zu aendernde Dateien
| Datei | Änderung | Prioritaet |
|------|---------|------------|
| | | |

### Datenfluss
[Beschreibung]

### Build-Reihenfolge
1. Schritt 1
2. Schritt 2
```

---

## pr-test-analyzer
(Siehe [4. Testklasse.md](./4.%20Testklasse.md))
---

## type-design-analyzer

### Name und Verwendung
Analysiert ob Typdesign es erschwert oder unmöglich macht, illegale Zustaende darzustellen.

### Faehigkeiten
- Kapselungsbewertung
- Invarianz-Ausdrucksanalyse
- Invarianz-Nützlichkeitsbewertung
- Durchsetzungsverifikation

### Anwendungsgebiete
- Typystem-Design-Reviews
- API-Design-Reviews
- Domänenmodellierungs-Reviews

### Verwendete Tools
- Read: Typdefinitionen lesen
- Grep: Typverwendung suchen
- Glob: Typdateien finden

### Zusammenarbeit mit anderen Agenten
- Zusammenarbeit mit code-reviewer fuer Code-Reviews
- Zusammenarbeit mit code-architect fuer Architekturdesign

### Bewertungskriterien

1. **Kapselung**
   - Sind interne Details verborgen?
   - Können Invarianten von aussen verletzt werden?

2. **Invarianz-Ausdruck**
   - Kodiert der Typ Geschäftsregeln?
   - Werden unmögliche Zustaende auf Typebene verhindert?

3. **Invarianz-Nützlichkeit**
   - Verhindern diese Invarianten echte Bugs?
   - Sind sie mit der Domäne ausgerichtet?

4. **Durchsetzung**
   - Werden Invarianten vom Typsystem erzwungen?
   - Gibt es einfache Escape-Hatches?

### Ausgabeformat

Fuer jeden reviewed Typ:
- Typname und Position
- Bewertung ueber vier Dimensionen
- Gesamtbewertung
- Konkrete Verbesserungsvorschlaege

---

## comment-analyzer

### Name und Verwendung
Analysiert die Genauigkeit, Vollstaendigkeit, Wartbarkeit und Kommentar-Verfallsrisiken von Code-Kommentaren.

### Faehigkeiten
- Faktengenauigkeits-Verifikation
- Vollstaendigkeitspruefung
- Langzeitwertbewertung
- Irrefuehrende Element-Identifizierung

### Anwendungsgebiete
- Bei Code-Reviews
- Bei Dokumentationsqualitaetsbewertung
- Bei Identifizierung von Kommentar-Technikschulden

### Verwendete Tools
- Read: Code und Kommentare lesen
- Grep: Kommentare und Code suchen
- Glob: Quelldateien finden

### Zusammenarbeit mit anderen Agenten
- Zusammenarbeit mit code-reviewer fuer Code-Reviews
- Zusammenarbeit mit doc-updater zur Dokumentationsaktualisierung

### Analyse-Framework

1. **Faktengenauigkeit**
   - Behauptungen gegen Code verifizieren
   - Parameter- und Rückgabebeschreibungen gegen Implementierung pruefen
   - Veraltete Referenzen markieren

2. **Vollstaendigkeit**
   - Prüfen ob komplexe Logik ausreichend erklärt ist
   - Verifizieren ob wichtige Nebeneffekte und Randfaelle dokumentiert sind
   - Sicherstellen dass öffentliche APIs ausreichend kommentiert sind

3. **Langzeitwert**
   - Kommentare markieren die nur Code neu formulieren
   - Zerbrechliche Kommentare identifizieren die schnell verfallen
   - TODO/FIXME/HACK-Schulden finden

4. **Irrefuehrende Elemente**
   - Kommentare die im Widerspruch zum Code stehen
   - Veraltete Referenzen auf entferntes Verhalten
   - Übermässige Versprechen oder unzureichend beschriebenes Verhalten

### Ausgabeformat

Nach Schweregrad gruppierte Beratungsbefunde:
- Inaccurate (Ungenau)
- Stale (Veraltet)
- Incomplete (Unvollstaendig)
- Low-value (Geringer Wert)
[Zurueck zum Agent-Index](../README.md)