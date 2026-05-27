# Build-Reparatur-Befehle

Dieses Dokument beschreibt die ECC-Befehle zum Beheben von Build-Fehlern in verschiedenen Sprachen.

---

## /go-build

**Zweck**: schrittweise Go Build-Fehler, go vet-Warnungen und Linter-Probleme beheben.

**Verwendung**:
```
/go-build
```

**Anwendungsszenarien**:
- `go build ./...` schlaegt fehl
- `go vet ./...` meldet Probleme
- `golangci-lint run` zeigt Warnungen
- Modulabhaengigkeiten kaputt
- Build schlaegt fehl nach dem Pullen von Aenderungen

**Workflow**:
1. **Diagnose ausfuehren** - `go build`, `go vet`, `staticcheck` ausfuehren
2. **Fehler analysieren** - Nach Datei gruppieren und nach Schweregrad sortieren
3. **Schrittweise beheben** - Immer nur einen Fehler
4. **Reparatur verifizieren** - Nach jeder Aenderung Build erneut ausfuehren
5. **Zusammenfassung berichten** - Behobene und verbleibende Probleme anzeigen

**Haeufige Fehlerreparaturen**:

| Fehler | Typische Reparatur |
|--------|-------------------|
| `undefined: X` | Import hinzufuegen oder Tippfehler korrigieren |
| `cannot use X as Y` | Typkonvertierung oder Zuweisung korrigieren |
| `missing return` | return-Anweisung hinzufuegen |
| `X does not implement Y` | Fehlende Methode hinzufuegen |
| `import cycle` | Paketstruktur refaktorieren |
| `declared but not used` | Variable entfernen oder verwenden |
| `cannot find package` | `go get` oder `go mod tidy` |

**Diagnosebefehle**:
```bash
go build ./...                # Haupt-Build-Pruefung
go vet ./...                  # Statische Analyse
staticcheck ./...             # Erweiterter Lint (falls verfuegbar)
golangci-lint run             # Linting
go mod verify                # Modulverifizierung
go mod tidy -v               # Abhaengigkeiten bereinigen
```

---

## /kotlin-build

**Zweck**: schrittweise Kotlin/Gradle Build-Fehler, Compiler-Warnungen und Abhaengigkeitsprobleme beheben.

**Verwendung**:
```
/kotlin-build
```

**Anwendungsszenarien**:
- `./gradlew build` schlaegt fehl
- Kotlin-Compiler meldet Fehler
- `./gradlew detekt` meldet Verstoesze
- Gradle-Abhaengigkeitsaufloesung fehlgeschlagen
- Build schlaegt fehl nach dem Pullen von Aenderungen

**Haeufige Fehlerreparaturen**:

| Fehler | Typische Reparatur |
|--------|-------------------|
| `Unresolved reference: X` | Import oder Abhaengigkeit hinzufuegen |
| `Type mismatch` | Typkonvertierung oder Zuweisung korrigieren |
| `'when' must be exhaustive` | Fehlenden sealed-class-Zweig hinzufuegen |
| `Suspend function can only be called from coroutine` | `suspend`-Modifizierer hinzufuegen |
| `Smart cast impossible` | Lokale `val` oder `let` verwenden |
| `Could not resolve dependency` | Version oder Repository korrigieren |

**Diagnosebefehle**:
```bash
./gradlew build 2>&1                      # Haupt-Build-Pruefung
./gradlew detekt 2>&1                      # Statische Analyse (falls konfiguriert)
./gradlew ktlintCheck 2>&1                # Formatpruefung (falls konfiguriert)
./gradlew dependencies --configuration runtimeClasspath | head -100  # Abhaengigkeitsprobleme
./gradlew build --refresh-dependencies     # Tiefenrefresh (bei verdachtigem Cache oder Abhaengigkeitsmetadaten)
```

---

## /rust-build

**Zweck**: schrittweise Rust Build-Fehler, Borrow-Checker-Probleme und Abhaengigkeitsprobleme beheben.

**Verwendung**:
```
/rust-build
```

**Anwendungsszenarien**:
- `cargo build` oder `cargo check` schlaegt fehl
- `cargo clippy` meldet Warnungen
- Borrow-Checker- oder Lebensdauerfehler verhindern Kompilierung
- Cargo-Abhaengigkeitsaufloesung fehlgeschlagen
- Build schlaegt fehl nach dem Pullen von Aenderungen

**Haeufige Fehlerreparaturen**:

| Fehler | Typische Reparatur |
|--------|-------------------|
| `cannot borrow as mutable` | Refaktorieren damit unveraenderlicher Borrow beendet wird bevor auf mutable zugegriffen wird |
| `does not live long enough` | Owned-Typen verwenden oder Lebensdauern annotieren |
| `cannot move out of` | Refaktorieren um Ownership zu erhalten; nur als letzten Ausweg klonen |
| `mismatched types` | `.into()`, `as` oder explizite Konvertierung hinzufuegen |
| `trait X not implemented` | `#[derive(Trait)]` oder manuelle Implementierung hinzufuegen |
| `unresolved import` | Zu Cargo.toml hinzufuegen oder `use`-Pfad korrigieren |
| `cannot find value` | Import hinzufuegen oder Pfad korrigieren |

**Diagnosebefehle**:
```bash
cargo check 2>&1                               # Haupt-Build-Pruefung
cargo clippy -- -D warnings 2>&1               # Lints
cargo fmt --check 2>&1                         # Formatpruefung
cargo tree --duplicates                        # Doppelte Abhaengigkeiten
cargo audit                                   # Sicherheitsaudit (falls verfuegbar)
```

---

## /cpp-build

**Zweck**: schrittweise C++ Build-Fehler, CMake-Probleme und Linker-Probleme beheben.

**Verwendung**:
```
/cpp-build
```

**Anwendungsszenarien**:
- `cmake --build build` schlaegt fehl
- Linker-Fehler (undefinierte Referenzen, mehrfache Definitionen)
- Template-Instanziierungsfehler
- Include/Abhaengigkeitsprobleme
- Build schlaegt fehl nach dem Pullen von Aenderungen

**Haeufige Fehlerreparaturen**:

| Fehler | Typische Reparatur |
|--------|-------------------|
| `undeclared identifier` | `#include` hinzufuegen oder Tippfehler korrigieren |
| `no matching function` | Parametertypen korrigieren oder Ueberladung hinzufuegen |
| `undefined reference` | Bibliothek linken oder Implementierung hinzufuegen |
| `multiple definition` | `inline` verwenden oder in .cpp verschieben |
| `incomplete type` | `#include` statt Forward-Declaration verwenden |
| `no member named X` | Membername korrigieren oder include hinzufuegen |
| `cannot convert X to Y` | Entsprechende Konvertierung hinzufuegen |
| `CMake Error` | CMakeLists.txt-Konfiguration korrigieren |

**Diagnosebefehle**:
```bash
cmake -B build -S .                        # CMake-Konfiguration
cmake --build build 2>&1 | head -100       # Build
clang-tidy src/*.cpp -- -std=c++17         # Statische Analyse (falls verfuegbar)
cppcheck --enable=all src/                 # Zusaetzliche Analyse (falls verfuegbar)
```

---

## /gradle-build

**Zweck**: Gradle Build-Fehler fuer Android und Kotlin Multiplatform (KMP) Projekte beheben.

**Verwendung**:
```
/gradle-build
```

**Anwendungsszenarien**:
- Android-Projekt-Build fehlgeschlagen
- KMP-Projekt-Kompilierungsfehler
- Gradle-Synchronisierung fehlgeschlagen
- Abhaengigkeitskonflikte

**Projekttyp-Erkennung**:

| Indikator | Build-Befehl |
|-----------|--------------|
| `build.gradle.kts` + `composeApp/` (KMP) | `./gradlew composeApp:compileKotlinMetadata` |
| `build.gradle.kts` + `app/` (Android) | `./gradlew app:compileDebugKotlin` |
| `settings.gradle.kts` mit Modulen | `./gradlew assemble` |
| detekt konfiguriert | `./gradlew detekt` |

**Haeufige Fehlerreparaturen**:

| Fehler | Reparatur |
|--------|-----------|
| Unaufgeloeste Referenz in `commonMain` | Pruefen ob Abhaengigkeit in `commonMain.dependencies {}` ist |
| Expect-Declaration ohne actual | `actual`-Implementierung in jeder Plattform-Quellgruppe hinzufuegen |
| Compose-Compiler-Versionskonflikt | Kotlin und Compose-Compiler-Versionen in `libs.versions.toml` ausrichten |
| Doppelte Klassen | Konfliktuale Abhaengigkeiten mit `./gradlew dependencies` pruefen |
| KSP-Fehler | `./gradlew kspCommonMainKotlinMetadata` ausfuehren um zu regenieren |
| Konfigurationscache-Probleme | Non-serialisierbare Task-Inputs pruefen |

---

## /flutter-build

**Zweck**: schrittweise Dart-Analysator-Fehler und Flutter-Build-Fehler beheben.

**Verwendung**:
```
/flutter-build
```

**Anwendungsszenarien**:
- `flutter analyze` meldet Fehler
- `flutter build` auf einer beliebigen Plattform fehlgeschlagen
- `flutter pub get` Versionskonflikte
- `build_runner` Codegenerierung fehlgeschlagen
- Build schlaegt fehl nach dem Pullen von Aenderungen

**Haeufige Fehlerreparaturen**:

| Fehler | Typische Reparatur |
|--------|-------------------|
| `A value of type 'X?' can't be assigned to 'X'` | `?? default` oder Null-Schutz hinzufuegen |
| `The name 'X' isn't defined` | Import hinzufuegen oder Tippfehler korrigieren |
| `Non-nullable instance field must be initialized` | Initialisierer oder `late` hinzufuegen |
| `Version solving failed` | Versionsbeschraenkungen in pubspec.yaml anpassen |
| `Missing concrete implementation of 'X'` | Fehlende Interface-Methoden implementieren |
| `build_runner: Part of X expected` | Veraltete `.g.dart` loeschen und neu bauen |

**Diagnosebefehle**:
```bash
flutter analyze 2>&1                  # Analyse
flutter pub get 2>&1                  # Abhaengigkeiten
dart run build_runner build --delete-conflicting-outputs 2>&1  # Codegenerierung (falls build_runner verwendet)
flutter build apk 2>&1                # Plattform-Build
flutter build web 2>&1                # Web-Build
```

---

## Build-Reparatur-Befehlsvergleichstabelle

| Befehl | Sprache/Plattform | Haupttools | Haeufige Probleme |
|--------|-------------------|------------|-------------------|
| `/go-build` | Go | go build, go vet | Typfehler, Import-Zyklen |
| `/kotlin-build` | Kotlin | Gradle, detekt | Typ-Mismatch, when nicht exhaustiv |
| `/rust-build` | Rust | cargo check, clippy | Borrow-Fehler, Lebensdauer |
| `/cpp-build` | C++ | CMake, clang-tidy | Linker-Fehler, Template-Probleme |
| `/gradle-build` | Android/KMP | Gradle | Abhaengigkeitskonflikte, Konfigurationsfehler |
| `/flutter-build` | Flutter/Dart | Flutter analyze | Null-Safety, Analysefehler |

---

## Allgemeine Reparaturstrategien

Alle Build-Reparatur-Befehle folgen derselben grundlegenden Strategie:

1. **Zuerst Build-Fehler beheben** - Code muss kompilieren
2. **Dann Lint-Warnungen beheben** - Verdachtige Konstrukte reparieren
3. **Dann Format-Warnungen beheben** - Stil und Best Practices
4. **Immer nur eines auf einmal beheben** - Jede Aenderung verifizieren
5. **Minimale Aenderungen** - Nicht refaktorieren, nur reparieren

**Stoppbedingungen**:
Der Agent stoppt und berichtet wenn:
- Derselbe Fehler nach 3 Versuchen noch besteht
- Reparatur mehr Fehler einfuehrt
- Architekturaenderungen erforderlich sind
- Externe Abhaengigkeiten fehlen