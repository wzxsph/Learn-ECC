# Kern-Workflow-Befehle

Dieses Dokument beschreibt die ECC-Befehle fuer Kern-Entwicklungsworkflows.

---

## /plan

**Zweck**: Anforderungen umformulieren, Risiken bewerten und einen schrittweisen Implementierungsplan erstellen. **Auf Benutzerbestaetigung warten**, BEVOR Code beruehrt wird.

**Verwendung**:
```
/plan [Feature-Beschreibung | Pfad zur PRD-Datei]
```

**Anwendungsszenarien**:
- Neue Feature-Entwicklung starten
- Grosze Architektur-aenderungen vornehmen
- Komplexes Refactoring durchfuehren
- Wenn Anforderungen unklar oder vage sind

**Detaillierte Erlaeuterung**:
Dieser Befehl:
1. **Anforderungen umformulieren** - Klar ausdrucken was gebaut werden soll
2. **Risiken identifizieren** - Potenzielle Probleme und Blocker aufdecken
3. **Schrittweisen Plan erstellen** - Implementierung in Phasen aufteilen
4. **Auf Bestaetigung warten** - Muss explizite Benutzergenehmigung erhalten bevor fortgefahren wird

Unterstuetzt mehrere Eingabemodi:
| Eingabe | Modus | Verhalten |
|------|------|------|
| `path/to/name.prd.md` | PRD-Modus | PRD lesen, naechsten ausstehenden Liefer-Meilenstein auswaehlen |
| Andere Markdown-Pfade | Referenz-Modus | Datei als Kontext lesen und Inline-Plan generieren |
| Freiform-Text | Dialog-Modus | Inline-Plan generieren |
| Leere Eingabe | Klaerungs-Modus | Fragen was geplant werden soll |

---

## /code-review

**Zweck**: Umfassende Code-Reviews von lokalen uncommiteten Aenderungen oder GitHub PRs.

**Verwendung**:
```
/code-review                 # Lokaler Review-Modus
/code-review [PR-Nummer | PR-Link]  # PR-Review-Modus
```

**Anwendungsszenarien**:
- Umfassende Reviews vor dem Committen
- PR-Reviews um potenzielle Probleme zu finden
- Best Practices im Codebase lernen

**Lokaler Review-Modus**:
1. Geaenderte Dateien sammeln (`git diff --name-only HEAD`)
2. Sicherheitsprobleme pruefen (hartcodierte Credentials, SQL-Injection, XSS usw.)
3. Codequalitaetsprobleme pruefen (Funktionen >50 Zeilen, Dateien >800 Zeilen usw.)
4. Bericht mit Schweregrad-Kennzeichnung generieren

**PR-Review-Modus**:
1. PR-Metadaten und Diff abrufen
2. Relevante Projektstandards und Planungsdokumente pruefen
3. Vollstaendige Aenderungsdateien lesen
4. 7 Review-Kategorien anwenden (Korrektheit, Typsicherheit, Muster-Konformitaet, Sicherheit, Performance, Vollstaendigkeit, Wartbarkeit)
5. Verifizierungsbefehle ausfuehren (Typpruefung, Lint, Tests, Build)
6. Review-Kommentare auf GitHub veroeffentlichen

---

## /build-fix

**Zweck**: Build-System des Projekts erkennen und Build/Typ-Fehler schrittweise beheben.

**Verwendung**:
```
/build-fix
```

**Anwendungsszenarien**:
- Build schlaegt fehl
- TypeScript/JavaScript Typfehler
- Compiler-Fehler
- Abhaengigkeitsprobleme

**Workflow**:
1. **Build-System erkennen** - Passenden Build-Befehl basierend auf Dateitypen auswaehlen
2. **Fehler analysieren** - Nach Datei und Abhaengigkeitsreihenfolge gruppieren
3. **Schrittweise beheben** - Immer nur einen Fehler auf einmal beheben
4. **Verifizieren** - Nach jeder Reparatur den Build erneut ausfuehren

Unterstuetzte Build-Systeme:

| Indikator | Build-Befehl |
|----------|--------------|
| `package.json` + `build`-Skript | `npm run build` |
| `tsconfig.json` (TypeScript) | `npx tsc --noEmit` |
| `Cargo.toml` | `cargo build` |
| `pom.xml` | `mvn compile` |
| `build.gradle` | `./gradlew compileJava` |
| `go.mod` | `go build ./...` |
| `pyproject.toml` | `python -m compileall` oder `mypy` |

---

## /verify

**Zweck**: Verifizieren dass Codeaenderungen wie erwartet funktionieren. Zum Verifizieren von PRs, Bestaetigen von Fixes oder Testen von Aenderungen verwendet.

**Verwendung**:
```
/verify [quick]           # Schnelle Verifizierung (empfohlen)
/verify full             # Vollstaendige Verifizierung
```

**Anwendungsszenarien**:
- PR-Aenderungen verifizieren
- Bestaetigen dass Fix funktioniert
- Aenderungen manuell testen
- Lokal verifizieren bevor gepusht wird

**Verifizierungsschritte**:
1. Relevante Tests ausfuehren
2. Typsicherheit pruefen
3. Erfolgreichen Build verifizieren
4. Linting pruefen

---

## /quality-gate

**Zweck**: ECC-Qualitaets-Pipeline fuer Datei- oder Projektbereich ausfuehren und Reparaturschritte melden.

**Verwendung**:
```
/quality-gate [path|.] [--fix] [--strict]
```

**Anwendungsszenarien**:
- Bei Bedarf Qualitaetspruefungen ausfuehren
- In CI-Pipeline integrieren
- Sicherstellen dass Code Projektstandards entspricht

**Pipeline-Schritte**:
1. Zielsprache/Tool erkennen
2. Formatierungspruefung ausfuehren
3. Lint/Typpruefung ausfuehren
4. Kurze Reparaturliste generieren

---

## /tdd

**Zweck**: Testgetriebener Entwicklungsworkflow. Dem Rot-Gruen-Refaktor-Zyklus folgen.

**Verwendung**:
```
/tdd                      # TDD-Workflow starten
```

**Anwendungsszenarien**:
- Neue Features implementieren
- Bugs beheben (erst fehlschlagenden Test schreiben)
- Testabdeckung hinzufuegen
- TDD-Methodik lernen

**TDD-Zyklus**:
```
ROT     →  Einen fehlschlagenden Test schreiben
GRUEN   →  Minimalen Code schreiben damit Test durchlaeuft
REFACTOR →  Code verbessern, Tests bleiben gruen
REPEAT  →  Naechster Testfall
```

**Best Practices**:
- **Tests zuerst schreiben** - Vor jeder Implementierung
- **Tests nach jeder Aenderung ausfuehren** - Fortschritt verifizieren
- **Verhalten testen, nicht Implementierung** - Auf Schnittstellen fokussieren, nicht auf Details
- **Randfaelle einschliessen** - Null, leer, Maxima usw.

---

## Befehlsintegration

```
Unklare Anforderungen → /plan-prd (PRD erstellen)
     ↓
Klare Anforderungen → /plan (Implementierungsplan erstellen)
     ↓
/tdd (oder /feature-dev) implementieren
     ↓
/build-fix Build-Fehler beheben
     ↓
/code-review Code-Review
     ↓
/quality-gate Qualitaets-Gate
     ↓
/verify Verifizieren
     ↓
/pr PR erstellen
```