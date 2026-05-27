# Code-Review-Agenten

Code-Review-Agenten sind spezialisiert auf die Pruefung von Codequalitaet, Sicherheit und Wartbarkeit.

## Agent-Liste

| Agent-Name | Verwendung | Verwendetes Modell | Kerntools |
|------------|------------|---------------------|------------|
| code-reviewer | Allgemeiner Code-Review-Experte | sonnet | Read, Grep, Glob, Bash |
| python-reviewer | Python Code-Review (PEP 8, Typhinweise, Sicherheit) | sonnet | Read, Grep, Glob, Bash |
| go-reviewer | Go Code-Review (Nebenlaeufigkeit, Fehlerbehandlung, Leistung) | sonnet | Read, Grep, Glob, Bash |
| kotlin-reviewer | Kotlin/Android Code-Review (Koroutinen, Compose) | sonnet | Read, Grep, Glob, Bash |
| rust-reviewer | Rust Code-Review (Eigentum, Lebensdauer, Sicherheit) | sonnet | Read, Grep, Glob, Bash |
| cpp-reviewer | C++ Code-Review (Speichersicherheit, moderne C++-Idiome) | sonnet | Read, Grep, Glob, Bash |
| flutter-reviewer | Flutter/Dart Code-Review (Zustandsverwaltung, Leistung) | sonnet | Read, Grep, Glob, Bash |
| typescript-reviewer | TypeScript/JavaScript Code-Review (Typsicherheit) | sonnet | Read, Grep, Glob, Bash |
| swift-reviewer | Swift Code-Review (Protokolle, Nebenlaeufigkeit, ARC-Speicherverwaltung) | sonnet | Read, Grep, Glob, Bash |
| csharp-reviewer | C# Code-Review (.NET-Konventionen, asynchrone Muster) | sonnet | Read, Grep, Glob, Bash |
| fsharp-reviewer | F# Code-Review (Funktionale Idiome, Pattern Matching) | sonnet | Read, Grep, Glob, Bash |
| java-reviewer | Java Code-Review (Spring Boot/Quarkus-Frameworks) | sonnet | Read, Grep, Glob, Bash |

---

## code-reviewer

### Name und Verwendung
Allgemeiner Code-Review-Experte, der proaktiv die Codequalitaet, Sicherheit und Wartbarkeit prueft. Muss nach allen Codeaenderungen verwendet werden.

### Faehigkeiten
- Codequalitaetspruefung (Funktionsgroesse, Verschachtelungstiefe, Codeduplikation)
- Sicherheitsluecken erkennung (SQL-Injection, XSS, hartcodierte Anmeldedaten)
- React/Next.js-Pattern-Pruefung
- Node.js/Backend-Pattern-Pruefung
- Leistungsprobleme identifizieren
- Best-Practice-Empfehlungen

### Anwendungsgebiete
- Nach dem Schreiben oder Aendern von Code
- Pull-Request-Reviews
- Vor dem Zusammenfuehren auf gemeinsame Branches

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: Dateien finden
- Bash: git diff und Lint-Befehle ausfuehren

### Zusammenarbeit mit anderen Agenten
- Bei Sicherheitsproblemen Escalation an security-reviewer
- Bei Build-Fehlern Verwendung von build-error-resolver
- Bei Refactoring-Bedarf Verwendung von refactor-cleaner

---

## python-reviewer

### Name und Verwendung
Python Code-Review-Experte, spezialisiert auf PEP-8-Compliance, Pythonische Idiome, Typhinweise, Sicherheit und Leistung.

### Faehigkeiten
- PEP-8-Stilpruefung
- Pythonische Idiome foerdern (Listenkomprehension, Enums, with-Anweisungen)
- Typhinweis-Validierung
- SQL-Injection-Erkennung
- Koroutinen- und Async-Pattern-Pruefung
- Frameworkspezifische Pruefungen (Django, FastAPI, Flask)

### Anwendungsgebiete
- Alle Codeaenderungen in Python-Projekten
- Django/FastAPI/Flask-spezifische Projekte

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: Python-Dateien finden
- Bash: mypy, ruff, black, bandit, pytest ausfuehren

### Zusammenarbeit mit anderen Agenten
- Sicherheitspruefungsregeln werden mit security-reviewer geteilt
- tdd-guide wird verwendet, um Testabdeckung sicherzustellen

---

## go-reviewer

### Name und Verwendung
Go Code-Review-Experte, spezialisiert auf idiomatic Go, Nebenlaeufigkeitspattern, Fehlerbehandlung und Leistung.

### Faehigkeiten
- Go-Idiom-Pruefung (fruehe Rueckgaben, Fehlerverpackung)
- Nebenlaeufigkeitssicherheitspruefung (Goroutine-Leaks, Channel-Deadlocks)
- Best Practices fuer Fehlerbehandlung
- Leistungspattern-Erkennung
- Sicherheitsluecken-Erkennung

### Anwendungsgebiete
- Alle Codeaenderungen in Go-Projekten
- Projekte, die Go-Codierungsstandards einhalten muessen

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: Go-Dateien finden
- Bash: go vet, staticcheck, golangci-lint ausfuehren

### Zusammenarbeit mit anderen Agenten
- go-build-resolver behandelt Build-Fehler
- security-reviewer behandelt sicherheitsrelevante Probleme

---

## kotlin-reviewer

### Name und Verwendung
Kotlin- und Android/KMP-Code-Review-Experte, spezialisiert auf idiomati Pattern, Koroutinensicherheit, Compose-Best-Practices und Clean-Architecture-Verstösse.

### Faehigkeiten
- Kotlin-Idiom-Pruefung
- Koroutinen- und Flow-Anti-Pattern-Erkennung
- Compose-Leistungsprobleme-Identifizierung
- Clean-Architecture-Modulgrenzen-Durchsetzung
- Android-spezifische Pruefungen

### Anwendungsgebiete
- Android Native Entwicklung
- Kotlin Multiplatform (KMP) Projekte
- Jetpack-Compose-Projekte

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: Kotlin/KTS-Dateien finden
- Bash: Gradle-Checks ausfuehren

### Zusammenarbeit mit anderen Agenten
- kotlin-build-resolver behandelt Build-Probleme
- security-reviewer behandelt sicherheitskritische Probleme

---

## rust-reviewer

### Name und Verwendung
Rust Code-Review-Experte, spezialisiert auf Eigentum, Lebensdauersicherheit, Fehlerbehandlung, unsafe-Nutzung und idiomati Pattern.

### Faehigkeiten
- Eigentum und Lebensdauerpruefung
- Borrow-Checker-Fehlerdiagnose
- Unsafe-Code-Sicherheitsreview
- Fehlerbehandlungspattern-Validierung
- Nebenlaeufigkeitssicherheitspruefung

### Anwendungsgebiete
- Alle Codeaenderungen in Rust-Projekten
- Projekte, die Speichersicherheitsgarantien benoetigen

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: Rust-Dateien finden
- Bash: cargo check, clippy, fmt, test, audit ausfuehren

### Zusammenarbeit mit anderen Agenten
- rust-build-resolver behandelt Build-Fehler
- Sicherheitsreviewregeln werden mit security-reviewer geteilt

---

## cpp-reviewer

### Name und Verwendung
C++ Code-Review-Experte, spezialisiert auf Speichersicherheit, moderne C++-Idiome, Nebenlaeufigkeit und Leistung.

### Faehigkeiten
- Speichersicherheitspruefung (raw new/delete, Buffer Overflows)
- Foerderung moderner C++-Pattern (Smart Pointer, RAII)
- Nebenlaeufigkeitssicherheitspruefung
- Leistungspattern-Erkennung
- Sicherheitsluecken-Erkennung

### Anwendungsgebiete
- Alle Codeaenderungen in C++-Projekten
- Projekte, die moderne C++-Best-Practices benoetigen

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: C++-Dateien finden
- Bash: clang-tidy, cppcheck, cmake ausfuehren

### Zusammenarbeit mit anderen Agenten
- cpp-build-resolver behandelt Build-Probleme
- security-reviewer behandelt sicherheitskritische Probleme

---

## flutter-reviewer

### Name und Verwendung
Flutter- und Dart-Code-Review-Experte, der Widget-Best-Practices, Zustaandsveraltungsmuster, Dart-Idiome, Leistungsfallen, Barrierefreiheit und Clean-Architecture-Verstösse in Flutter-Code prueft.

### Faehigkeiten
- Zustaandsveraltungs-Anti-Pattern-Erkennung (Ignorieren von SetState)
- Widget-Build-Leistungsoptimierung
- Architekturgrenzen-Durchsetzung
- Ressourcenlebenszyklusverwaltung
- Barrierefreiheitspruefung

### Anwendungsgebiete
- Alle Codeaenderungen in Flutter-Projekten
- Cross-Platform-Mobile-App-Entwicklung

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: Dart-Dateien finden
- Bash: flutter analyze ausfuehren

### Zusammenarbeit mit anderen Agenten
- dart-build-resolver behandelt Build-Probleme
- security-reviewer behandelt sicherheitskritische Probleme
- e2e-runner fuehrt End-to-End-Tests durch

---

## typescript-reviewer

### Name und Verwendung
TypeScript/JavaScript Code-Review-Experte, spezialisiert auf Typsicherheit, Async-Korrektheit, Node/Web-Sicherheit und idiomati Pattern.

### Faehigkeiten
- Typsicherheitspruefung (any-Missbrauch, Non-Null-Assertion)
- Async-Korrektheitsvalidierung
- Sicherheitsluecken-Erkennung
- React/Next.js-spezifische Pruefungen
- Node.js-spezifische Pruefungen

### Anwendungsgebiete
- Alle Codeaenderungen in TypeScript/JavaScript-Projekten
- Next.js- und React-Projekte

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: TS/TSX/JS/JSX-Dateien finden
- Bash: tsc, eslint, prettier, npm audit ausfuehren

### Zusammenarbeit mit anderen Agenten
- build-error-resolver behandelt Build-Fehler
- security-reviewer behandelt sicherheitskritische Probleme

---

## swift-reviewer

### Name und Verwendung
Swift Code-Review-Experte, spezialisiert auf protokollorientiertes Design, Wertsemantik, ARC-Speicherverwaltung, Swift-Nebenlaeufigkeit und idiomati Pattern.

### Faehigkeiten
- Swift-Nebenlaeufigkeits-Sicherheitspruefung
- Speicherverwaltungspruefung (Strong-Reference-Cycles, Delegate-Referenzen)
- Protokollorientiertes Design-Validierung
- Best-Practices fuer Fehlerbehandlung
- Leistungspattern-Erkennung

### Anwendungsgebiete
- Alle Codeaenderungen in Swift-Projekten
- iOS/macOS-Anwendungsentwicklung

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: Swift-Dateien finden
- Bash: swift build, swiftlint, swift test ausfuehren

### Zusammenarbeit mit anderen Agenten
- swift-build-resolver behandelt Build-Probleme
- security-reviewer behandelt sicherheitskritische Probleme

---

## csharp-reviewer

### Name und Verwendung
C# Code-Review-Experte, spezialisiert auf .NET-Konventionen, asynchrone Muster, Sicherheit, Nullable-Referenztypen und Leistung.

### Faehigkeiten
- .NET-Idiom-Pruefung
- Asynchrones Pattern-Validierung
- Nullable-Typsicherheit
- Sicherheitsluecken-Erkennung
- EF-Core-spezifische Pruefungen

### Anwendungsgebiete
- Alle Codeaenderungen in C#-Projekten
- .NET/.NET-Core-Anwendungsentwicklung

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: C#-Dateien finden
- Bash: dotnet build, dotnet format, dotnet test ausfuehren

### Zusammenarbeit mit anderen Agenten
- build-error-resolver behandelt .NET-Build-Probleme
- security-reviewer behandelt sicherheitskritische Probleme

---

## java-reviewer

### Name und Verwendung
Java Code-Review-Experte fuer Spring-Boot- und Quarkus-Projekte. Automatische Frameworkerkennung und Anwendung geeigneter Review-Regeln.

### Faehigkeiten
- Automatische Frameworkerkennung (Spring Boot/Quarkus)
- Schichtenarchitekturpruefung
- JPA/Panache/MongoDB-Pruefungen
- Nebenlaeufigkeitssicherheitspruefung
- Sicherheitsluecken-Erkennung

### Anwendungsgebiete
- Spring-Boot-Projekte
- Quarkus-Projekte
- Java-11+-Projekte

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: Java-Dateien finden
- Bash: Maven/Gradle-Checks ausfuehren

### Zusammenarbeit mit anderen Agenten
- java-build-resolver behandelt Build-Probleme
- security-reviewer behandelt sicherheitskritische Probleme

---

## fsharp-reviewer

### Name und Verwendung
F# Code-Review-Experte, spezialisiert auf funktionale Idiome, Typsicherheit, Pattern Matching, Calculation Expressions und Leistung.

### Faehigkeiten
- Funktionale Idiom-Pruefung
- Pattern-Matching-Vollstaendigkeitsvalidierung
- Typsicherheitspruefung
- Best-Practices fuer Calculation Expressions
- Leistungspattern-Erkennung

### Anwendungsgebiete
- Alle Codeaenderungen in F#-Projekten
- Funktionale Programmierungsprojekte

### Verwendete Tools
- Read: Dateiinhalt lesen
- Grep: Codemuster suchen
- Glob: F#-Dateien finden
- Bash: dotnet build, fantomas, dotnet test ausfuehren

### Zusammenarbeit mit anderen Agenten
- Sicherheitsreviewregeln werden mit security-reviewer geteilt
- tdd-guide wird verwendet, um Testabdeckung sicherzustellen
[Zurueck zum Agent-Index](../README.md)