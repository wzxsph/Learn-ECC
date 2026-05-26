# 02 - Agent-Zusammenarbeit

Lernen wie man Multi-Agent-Zusammenarbeitssysteme entwirft, implementiert und verwaltet.

## Übersicht

ECC bietet **66 professionelle [Agents](../../Referenzdokumente/agents/1.%20代码审查类.md)**, unterteilt in 6 Hauptkategorien. Diese Agents arbeiten durch standardisierte Workflows zusammen um komplexe Aufgaben von Code-Review bis Systemdesign zu erledigen.

---

## 1. Agent-Architektur

### 1.1 Single Agent vs. Multi Agent

| Architektur | Anwendungszenario | Eigenschaften |
|------|----------|-------|
| **Single Agent** | Einfache Aufgaben, unabhängige Operationen | Direkter Aufruf, schnelle Antwort |
| **Multi Agent** | Komplexe Aufgaben, Zusammenarbeit nötig | Aufgabenzerlegung, Ergebnisaggregation |

### 1.2 Verfügbare Agents-Liste

| Agent | Verwendung | Nutzungszeitpunkt |
|-------|------|----------|
| **planner** | Implementierungsplanung | Komplexe Funktionen, Refactoring |
| **architect** | Systemdesign | Architekturentscheidungen |
| **tdd-guide** | Testgetriebene Entwicklung | Neue Funktionen, Bug-Fixes |
| **code-reviewer** | Code-Review | Nach dem Schreiben von Code |
| **security-reviewer** | Sicherheitsanalyse | Vor dem Commit |
| **build-error-resolver** | Build-Fehler beheben | Wenn Build fehlschlägt |
| **e2e-runner** | End-to-End-Tests | Kritische User-Flows |
| **refactor-cleaner** | Toten Code bereinigen | Code-Wartung |
| **doc-updater** | Dokumentation aktualisieren | Bei Dokumentationsaktualisierung |
| **rust-reviewer** | Rust Code-Review | Rust-Projekte |
| **harmonyos-app-resolver** | HarmonyOS App-Entwicklung | HarmonyOS/ArkTS-Projekte |

### 1.3 Agent-Dateiformat

```yaml
---
name: agent-name
description: Agent-Verwendung
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

---

## 2. Agent-Kategorien im Detail

### 2.1 Code-Review-Kategorie (16)

Verantwortlich für Codequalität, Sicherheit und Wartbarkeits-Review.

| Agent | Spezialität |
|-------|------|
| code-reviewer | Generisches Code-Review |
| security-reviewer | Sicherheitslstellenanalyse |
| python-reviewer | Python PEP 8, Type-Hints |
| go-reviewer | Go-Idiome, Nebenläufigkeit |
| kotlin-reviewer | Kotlin Null-Safety, Koroutinen |
| rust-reviewer | Rust Ownership, Lebensdauer |
| cpp-reviewer | C++ Speichersicherheit |
| flutter-reviewer | Flutter/Dart-Muster |
| fastapi-reviewer | FastAPI-Architektur, async |
| typescript-reviewer | TypeScript Typsicherheit |
| java-reviewer | Java-Codierungsstandards |
| csharp-reviewer | C# Best Practices |
| ruby-reviewer | Ruby-Idiome |
| php-reviewer | PHP-Sicherheit und Performance |
| swift-reviewer | Swift-Nebenläufigkeit, Speicherverwaltung |
| go-security-reviewer | Go-Sicherheitsreview |

### 2.2 Build-Reparatur-Kategorie (10)

Fokus auf das Beheben von Build-Fehlern verschiedener Sprachen.

| Agent | Spezialität |
|-------|------|
| build-error-resolver | Generische Build-Fehlerbehebung |
| go-build | Go-Build + go vet |
| kotlin-build | Kotlin/Gradle-Kompilierung |
| rust-build | Rust Borrow-Checker |
| cpp-build | C++ CMake + Linker |
| gradle-build | Android/KMP Gradle |
| flutter-build | Flutter-Build |
| maven-build | Maven-Build |
| cmake-build | CMake-Konfiguration |
| ninja-build | Ninja-Build-System |

### 2.3 Planungs-Kategorie (5)

Verantwortlich für Anforderungsanalyse und Systemplanung.

| Agent | Spezialität |
|-------|------|
| planner | Implementierungsplanung |
| architect | Systemarchitekturdesign |
| prp-planner | PRP-Workflow-Planung |
| multi-plan | Multi-Modell-Kollaborationsplanung |
| product-manager | Produktenanforderungsanalyse |

### 2.4 Test-Kategorie (2)

Fokus auf testgetriebene Entwicklung und Coverage.

| Agent | Spezialität |
|-------|------|
| tdd-guide | TDD-Workflow-Anleitung |
| e2e-runner | Playwright E2E-Tests |

### 2.5 Sicherheits-Kategorie (5)

Verantwortlich für Sicherheitsreview und Lückenscans.

| Agent | Spezialität |
|-------|------|
| security-reviewer | Generisches Sicherheitsreview |
| owasp-reviewer | OWASP Top 10 |
| pentest-reviewer | Penetrationstests |
| vulnerability-scanner | Lückenscan |
| secrets-detector | Schlüssel und Anmeldedaten-Erkennung |

### 2.6 Architektur-Kategorie (5)

Verantwortlich für Systemarchitektur und technische Entscheidungen.

| Agent | Spezialität |
|-------|------|
| architect | Systemarchitekturdesign |
| microservices-architect | Microservices-Architektur |
| frontend-architect | Frontend-Architektur |
| backend-architect | Backend-Architektur |
| devops-architect | DevOps-Architektur |

---

## 3. Kollaborationsmodi

### 3.1 Master-Slave-Modus

Ein Master-Agent koordiniert mehrere Slave-Agents:

```
Benutzeranfrage → Master Agent (planner)
           ├── Sub-Agent 1 (code-reviewer)
           ├── Sub-Agent 2 (tdd-guide)
           └── Sub-Agent 3 (build-error-resolver)
```

### 3.2 Peer-to-Peer-Modus

Mehrere Agents arbeiten parallel, Ergebnisse werden aggregiert:

```
Benutzeranfrage → Agent 1 (Sicherheitsanalyse)
        → Agent 2 (Performance-Analyse)
        → Agent 3 (Codequalitätsanalyse)
           ↓ Ergebnisaggregation
        Finaler Bericht
```

### 3.3 Pipeline-Modus

Agents werden sequenziell verarbeitet, die Ausgabe jedes Agents ist die Eingabe des nächsten:

```
Eingabe → Agent 1 → Agent 2 → Agent 3 → Ausgabe
```

---

## 4. Aufgabenverteilungsstrategien

### 4.1 Aufgabenzerlegung

Komplexe Aufgaben in unabhängige Subaufgaben zerlegen:

```markdown
# Komplexe Aufgabenzerlegung-Beispiel
Originalaufgabe: Benutzerauthentifizierungssystem implementieren

Zerlegung:
1. Datenbankschema entwerfen → architect
2. API-Endpunkte implementieren → backend-developer
3. Tests schreiben → tdd-guide
4. Sicherheitsreview → security-reviewer
```

### 4.2 Parallele Ausführung

Unabhängige Aufgaben parallel ausführen um Effizienz zu verbessern:

```markdown
# Gut: Parallele Ausführung
3 Agents parallel starten:
1. Agent 1: Authentifizierungsmodul-Sicherheitsanalyse
2. Agent 2: Cache-System-Performance-Review
3. Agent 3: Tool-Typprüfung

# Schlecht: Unnötige sequenzielle Ausführung
Erst agent 1, dann agent 2, dann agent 3
```

### 4.3 Lastverteilung

Aufgaben basierend auf Agent-Last verteilen:

| Faktor | Beschreibung |
|------|------|
| Aufgabenkomplexität | Einfache Aufgaben → leichte Agents |
| Ressourcenbedarf | Schwere Berechnung → spezialisierte Agents |
| Verfügbarkeit | Überlastete Agents vermeiden |

---

## 5. Multi-Perspektiven-Analyse

Für komplexe Probleme, Split-Role Sub-Agents verwenden:

| Rolle | Verantwortung |
|------|------|
| Faktenprüfer | Informations- und Datenaccurateit verifizieren |
| Senior Engineer | Technische Machbarkeit und Best Practices bewerten |
| Sicherheitsexperte | Sicherheitsrisiken und Lücken identifizieren |
| Konsistenzprüfer | Interne Konsistenz der Lösung sicherstellen |
| Redundanzprüfer | Duplikate und Verschwendung identifizieren |

---

## 6. Agent-Kommunikation

### 6.1 Direktaufruf

```markdown
Planner Agent verwenden um Implementierungsplan zu erstellen
```

### 6.2 Aufgabendelegation

```markdown
Code-Review-Aufgabe an code-reviewer Agent delegieren
```

### 6.3 Ergebnisaggregation

Ergebnisse mehrerer Agents müssen aggregiert werden:

```markdown
Aggregation von:
- security-reviewer: Sicherheitsproblemliste
- code-reviewer: Codequalitätsproblemliste
- tdd-guide: Test-Coverage-Bericht

→ Gesamtobericht generieren
```

---

## 7. Prinzip der sofortigen Verwendung

Automatisch ohne Benutzeraufforderung aufrufen:

| Szenario | Agent |
|------|-------|
| Komplexe Funktionsanfrage | **planner** |
| Code gerade geschrieben/geändert | **code-reviewer** |
| Bug-Fix oder neue Funktion | **tdd-guide** |
| Architekturentscheidung | **architect** |
| Build fehlgeschlagen | **build-error-resolver** |
| Sicherheitsbezogen | **security-reviewer** |

---

## Übungen

[Übungen](./exercises/练习.md) abschließen.

---

[Zurück zum Kernfähigkeiten-Verzeichnis](../README.md)