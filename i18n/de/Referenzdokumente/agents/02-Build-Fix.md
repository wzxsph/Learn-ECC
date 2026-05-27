# Build-Fix-Agenten

Build-Fix-Agenten sind spezialisiert auf die Diagnose und Behebung von Build-Fehlern, Compiler-Fehlern und Abhaengigkeitsproblemen in verschiedenen Programmiersprachen.

## Agent-Liste

| Agent-Name | Verwendung | Verwendetes Modell | Kerntools |
|------------|------------|---------------------|------------|
| build-error-resolver | TypeScript/Allgemeine Build-Fehlerbehebung | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| go-build-resolver | Go Build/Compiler-Fehlerbehebung | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| kotlin-build-resolver | Kotlin/Gradle Build-Fehlerbehebung | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| rust-build-resolver | Rust Build/Borrow-Checker-Fehlerbehebung | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| cpp-build-resolver | C++/CMake Build-Fehlerbehebung | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| java-build-resolver | Java/Maven/Gradle Build-Fehlerbehebung | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| swift-build-resolver | Swift/Xcode Build-Fehlerbehebung | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| dart-build-resolver | Dart/Flutter Build-Fehlerbehebung | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| django-build-resolver | Django/Python Build-Probleme | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| pytorch-build-resolver | PyTorch/CUDA Build-Probleme | sonnet | Read, Write, Edit, Bash, Grep, Glob |

---

## build-error-resolver

### Name und Verwendung
TypeScript-Build- und Typfehler-Loesungsexperte. Behebt Build-Fails oder Typfehler, mit Fokus auf das Beitreiben des Builds, ohne Architekturänderungen.

### Faehigkeiten
- TypeScript-Fehlerloesung
- Build-Fehlerbehebung
- Abhaengigkeitsproblemloesung
- Konfigurationsfehlerloesung
- Minimale Diff-Reparatur

### Anwendungsgebiete
- Wenn Build fehlschlaegt
- Wenn TypeScript-Typfehler auftreten
- Bei Modulaufloesungsproblemen

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: tsc, npm build, eslint ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- Kein Refactoring -> refactor-cleaner verwenden
- Keine Architekturänderungen -> architect verwenden
- Keine neuen Funktionen -> planner verwenden
- Keine Testfixes -> tdd-guide verwenden
- Keine Sicherheitsprobleme -> security-reviewer verwenden

### Grundprinzipien
- Nur Fehler beheben, nicht refaktorieren
- Minimale Aenderungen
- Nach jeder Reparatur Build-Verifizierung

---

## go-build-resolver

### Name und Verwendung
Go-Build-, Vet- und Compiler-Fehler-Loesungsexperte. Behebt Go-Build-Fehler, go-vet-Probleme und Linter-Warnungen.

### Faehigkeiten
- Go-Compiler-Fehlerdiagnose
- go-vet-Warnungsbehebung
- staticcheck/golangci-lint-Problemlosung
- Modulabhaengigkeitsprobleme
- Typfehler und Schnittstellen-Mismatches

### Anwendungsgebiete
- Wenn Go-Build fehlschlaegt
- Wenn go-vet Fehler meldet
- Bei Modulabhaengigkeitskonflikten

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: go build, go vet, staticcheck, golangci-lint ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- go-reviewer prueft Codequalitaet
- security-reviewer behandelt sicherheitsrelevante Probleme

### Haeufige Fehlerbehebungsmuster

| Fehlertyp | Ursache | Behebung |
|-----------|---------|----------|
| undefined: X | Fehlender Import, Tippfehler | Import hinzufuegen oder Gross/Kleinschreibung korrigieren |
| cannot use X as type Y | Typ-Mismatch | Typkonvertierung oder Dereferenzierung |
| X does not implement Y | Fehlende Methode | Methode mit korrektem Receiver implementieren |
| import cycle not allowed | Zirkulaerer Import | Gemeinsamen Typ in neues Paket extrahieren |
| cannot find package | Fehlende Abhaengigkeit | go get pkg@version oder go mod tidy |

---

## kotlin-build-resolver

### Name und Verwendung
Kotlin/Gradle-Build-, Compiler- und Abhaengigkeitsfehler-Loesungsexperte. Behebt Kotlin-Build-Fehler, Kotlin-Compiler-Fehler und Gradle-Probleme.

### Faehigkeiten
- Kotlin-Compiler-Fehlerdiagnose
- Gradle-Build-Konfigurationsproblembehebung
- Abhaengigkeitskonflikte und Versionsinkonsistenzen loesen
- Kotlin-Compiler-Fehlerbehandlung
- detekt- und ktlint-Verstösse beheben

### Anwendungsgebiete
- Wenn Kotlin-Build fehlschlaegt
- Bei Gradle-Abhaengigkeitskonflikten
- Wenn Kotlin-Compiler Fehler meldet

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: ./gradlew build, detekt, ktlintCheck ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- kotlin-reviewer prueft Codequalitaet
- security-reviewer behandelt sicherheitsrelevante Probleme

### Haeufige Fehlerbehebungsmuster

| Fehlertyp | Ursache | Behebung |
|-----------|---------|----------|
| Unresolved reference: X | Fehlender Import, Tippfehler | Import oder Abhaengigkeit hinzufuegen |
| Type mismatch | Typfehler | Konvertierung hinzufuegen oder Typ korrigieren |
| Smart cast impossible | Veraenderliches Property oder nebenlaeufiger Zugriff | Lokale val-Kopie verwenden |
| when expression must be exhaustive | Fehlende sealed-class-Verzweigung | Fehlenden Zweig oder else hinzufuegen |
| Suspend function can only be called from coroutine | Fehlendes suspend oder Koroutinen-Scope | suspend-Modifizierer hinzufuegen |

---

## rust-build-resolver

### Name und Verwendung
Rust-Build-, Compiler- und Abhaengigkeitsfehler-Loesungsexperte. Behebt Cargo-Build-Fehler, Borrow-Checker-Probleme und Cargo.toml-Probleme.

### Faehigkeiten
- cargo build/check-Fehlerdiagnose
- Borrow-Checker- und Lebensdauer-Fehlerbehebung
- Trait-Implementierungs-Mismatches loesen
- Cargo-Abhaengigkeiten und Feature-Probleme
- cargo-clippy-Warnungsbehebung

### Anwendungsgebiete
- Wenn Rust-Build fehlschlaegt
- Wenn Borrow-Checker Fehler meldet
- Bei Cargo-Abhaengigkeitskonflikten

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: cargo check, clippy, fmt, tree ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- rust-reviewer prueft Codequalitaet
- security-reviewer behandelt sicherheitsrelevante Probleme

### Haeufige Fehlerbehebungsmuster

| Fehlertyp | Ursache | Behebung |
|-----------|---------|----------|
| cannot borrow as mutable | Unveraenderlicher Borrow noch aktiv | Refaktorieren damit unveraenderlicher Borrow beendet wird |
| does not live long enough | Wert wird verworfen waehrend er noch geliehen ist | Lebensdauer-Scope erweitern |
| cannot move out of | Verschiebung nach Referenzierung | .clone() verwenden oder refaktorieren |
| mismatched types | Typfehler | .into(), as oder explizite Konvertierung hinzufuegen |
| trait X is not implemented for Y | Fehlendes impl oder derive | #[derive(Trait)] hinzufuegen |

---

## cpp-build-resolver

### Name und Verwendung
C++-Build-, CMake- und Compiler-Fehler-Loesungsexperte. Behebt Build-Fehler, Linker-Probleme und Template-Fehler.

### Faehigkeiten
- C++-Compiler-Fehlerdiagnose
- CMake-Konfigurationsproblembehebung
- Linker-Fehlerloesung (undefinierte Referenzen, mehrfache Definitionen)
- Template-Instanziierungsfehlerbehandlung
- Include- und Abhaengigkeitsproblembehebung

### Anwendungsgebiete
- Wenn C++-Build fehlschlaegt
- Bei CMake-Konfigurationsfehlern
- Wenn Linker Fehler meldet

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: cmake --build, clang-tidy, cppcheck ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- cpp-reviewer prueft Codequalitaet
- security-reviewer behandelt sicherheitsrelevante Probleme

### Haeufige Fehlerbehebungsmuster

| Fehlertyp | Ursache | Behebung |
|-----------|---------|----------|
| undefined reference to X | Fehlende Implementierung oder Bibliothek | Quelldatei oder Bibliothek hinzufuegen |
| no matching function for call | Parametertypfehler | Typ korrigieren oder Ueberladung hinzufuegen |
| multiple definition of | Doppelte Symbole | inline verwenden oder in .cpp verschieben |
| cannot convert X to Y | Typ-Mismatch | Konvertierung hinzufuegen oder Typ korrigieren |
| template argument deduction failed | Template-Parameterfehler | Template-Parameter korrigieren |

---

## java-build-resolver

### Name und Verwendung
Java/Maven/Gradle-Build-, Compiler- und Abhaengigkeitsfehler-Loesungsexperte. Automatische Erkennung von Spring Boot oder Quarkus und Anwendung frameworkspezifischer Fixes.

### Faehigkeiten
- Java-Compiler-Fehlerdiagnose
- Maven- und Gradle-Build-Konfigurationsproblembehebung
- Abhaengigkeitskonflikte und Versionsinkonsistenzen loesen
- Annotation-Processor-Fehlerbehandlung (Lombok, MapStruct, Spring, Quarkus)
- Checkstyle- und SpotBugs-Verstösse beheben

### Anwendungsgebiete
- Wenn Java-Build fehlschlaegt
- Bei Maven/Gradle-Abhaengigkeitskonflikten
- Bei frameworkspezifischen Build-Problemen

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: ./mvnw compile, ./gradlew build, dependency:tree ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- java-reviewer prueft Codequalitaet
- security-reviewer behandelt sicherheitsrelevante Probleme

### Framework-Erkennung
Automatische Erkennung des Projektframeworks:
- Wenn es `quarkus` enthaelt -> [QUARKUS]-Regeln anwenden
- Wenn es `spring-boot` enthaelt -> [SPRING]-Regeln anwenden
- Beide vorhanden -> Beide Regelsets anwenden

### Haeufige Fehlerbehebungsmuster

| Fehlertyp | Ursache | Behebung |
|-----------|---------|----------|
| cannot find symbol | Fehlender Import, Tippfehler | Import oder Abhaengigkeit hinzufuegen |
| incompatible types | Typ-Mismatch | Explizite Konvertierung hinzufuegen |
| package X does not exist | Fehlende Abhaengigkeit | Zu pom.xml/build.gradle hinzufuegen |
| No qualifying bean of type X | Fehlendes @Component oder Komponentenscan | Annotation hinzufuegen oder Scan-Paket korrigieren |

---

## swift-build-resolver

### Name und Verwendung
Swift/Xcode-Build-, Compiler- und Abhaengigkeitsfehler-Loesungsexperte. Behebt Swift-Build-Fehler, Xcode-Build-Fails, SPM-Abhaengigkeitsprobleme und Code-Signing-Probleme.

### Faehigkeiten
- swift build/xcodebuild-Fehlerdiagnose
- Type-Checker- und Protokoll-Konformitaets-Fehlerbehebung
- Swift-Nebenlaeufigkeit und Sendable-Probleme loesen
- SPM-Abhaengigkeiten und Versionsauflösungsfehler
- Xcode-Projektkonfiguration und Code-Signing-Probleme beheben

### Anwendungsgebiete
- Wenn Swift-Build fehlschlaegt
- Wenn Xcode-Build fehlschlaegt
- Bei SPM-Abhaengigkeitskonflikten

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: swift build, xcodebuild, swiftlint ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- swift-reviewer prueft Codequalitaet
- security-reviewer behandelt sicherheitsrelevante Probleme

### Haeufige Fehlerbehebungsmuster

| Fehlertyp | Ursache | Behebung |
|-----------|---------|----------|
| cannot find type 'X' in scope | Fehlender Import oder Tippfehler | import hinzufuegen oder Name korrigieren |
| type 'X' does not conform to protocol 'Y' | Fehlende erforderliche Member | Fehlende Protokollanforderungen implementieren |
| non-sendable type passed | Sendable-Verstoss | Sendable-Konformitaet hinzufuegen oder refaktorieren |
| @MainActor function cannot be called | Main-Actor-Isolation | await hinzufuegen oder MainActor.run verwenden |

---

## dart-build-resolver

### Name und Verwendung
Dart/Flutter-Build-, Analyse- und Abhaengigkeitsfehler-Loesungsexperte. Behebt dart-analyze-Fehler, Flutter-Kompilierungs-Fails, pub-Abhaengigkeitskonflikte und build_runner-Probleme.

### Faehigkeiten
- dart analyze/flutter analyze-Fehlerdiagnose
- Dart-Typfehler, Null-Safety-Verstösse und fehlende Imports beheben
- pubspec.yaml-Abhaengigkeitskonflikte und Versionsbeschränkungen loesen
- build_runner-Code-Generierungs-Fails beheben
- Flutter-spezifische Build-Fehlerbehandlung (Android Gradle, iOS CocoaPods, web)

### Anwendungsgebiete
- Wenn Dart/Flutter-Build fehlschlaegt
- Bei pub-Abhaengigkeitskonflikten
- Wenn build_runner-Generierung fehlschlaegt

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: flutter analyze, dart analyze, flutter pub get, build_runner ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- flutter-reviewer prueft Codequalitaet
- security-reviewer behandelt sicherheitsrelevante Probleme

### Haeufige Fehlerbehebungsmuster

| Fehlertyp | Ursache | Behebung |
|-----------|---------|----------|
| The name 'X' isn't defined | Fehlender Import oder Tippfehler | Korrekten import hinzufuegen |
| A value of type 'X?' can't be assigned to type 'X' | Null-Safety - Nullable-Typ nicht behandelt | !, ?? default oder Null-Pruefung hinzufuegen |
| Because X depends on Y >=A and Z depends on Y <B | Pub-Versionskonflikt | Versionsbeschraenkungen anpassen oder dependency_overrides hinzufuegen |
| `build_runner: No actions were run` | Code-Generierung hat sich nicht geaendert | `--delete-conflicting-outputs` verwenden um Neubau zu erzwingen |

---

## django-build-resolver

### Name und Verwendung
Django/Python-Build- und Abhaengigkeitsproblem-Behebungsexperte. Behandelt Django-spezifische Build-Probleme und Python-Abhaengigkeitskonflikte.

### Faehigkeiten
- Django-Projektkonfigurationsproblembehebung
- Python-Abhaengigkeitskonfliktloesung
- Django-Migrationsprobleme
- requirements.txt/pipfile-Abhaengigkeitsverwaltung

### Anwendungsgebiete
- Wenn Django-Projekt-Build fehlschlaegt
- Bei Python-Abhaengigkeitskonflikten
- Bei Django-Migrationsproblemen

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: pip, pipenv, poetry, django-admin ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- python-reviewer prueft Codequalitaet
- security-reviewer behandelt sicherheitsrelevante Probleme

---

## pytorch-build-resolver

### Name und Verwendung
PyTorch/CUDA-Build-Problem-Behebungsexperte. Behandelt PyTorch-spezifische Build-Probleme, CUDA-Kompatibilitaetsprobleme und Tensor-Operations-Fehler.

### Faehigkeiten
- PyTorch-Build-Fehlerdiagnose
- CUDA-Kompatibilitaetsproblembehebung
- Tensor-Form- und Geraete-Platzierungs-Fehlerbehandlung
- DataLoader- und AMP-Fails beheben
- PyTorch-Umgebungskonfigurationsprobleme

### Anwendungsgebiete
- Wenn PyTorch-Build fehlschlaegt
- Bei CUDA-Kompatibilitaetsproblemen
- Bei Trainings/Inferenz-Fails

### Verwendete Tools
- Read: Dateiinhalt lesen
- Write: Fix schreiben
- Edit: Datei bearbeiten
- Bash: python, pip, nvcc, nvidia-smi ausfuehren
- Grep: Fehlermuster suchen
- Glob: Zugehoerige Dateien finden

### Zusammenarbeit mit anderen Agenten
- mle-reviewer prueft ML-Code
- security-reviewer behandelt sicherheitsrelevante Probleme
[Zurueck zum Agent-Index](../README.md)