# Sprachpruefungs-Befehle

Dieses Dokument beschreibt die ECC-Befehle fuer Code-Reviews in verschiedenen Programmiersprachen.

---

## /python-review

**Zweck**: Umfassendes Python Code-Review. PEP 8-Konformitaet, Typ-Hints, Sicherheit und Python-Idiome pruefen.

**Verwendung**:
```
/python-review
```

**Anwendungsszenarien**:
- Nach dem Schreiben oder Aendern von Python-Code
- Vor dem Committen von Python-Aenderungen
- Beim Review von PRs die Python-Code enthalten
- Python-Idiome und Best Practices lernen

**Review-Kategorien**:

### CRITICAL (MUSS behoben werden)
- SQL/Befehls-Injection-Schwachstellen
- Unsichere eval/exec-Verwendung
- Unsichere Pickle-Deserialisierung
- Hartcodierte Credentials
- Unsichere YAML-Ladung
- Nackte except-Clauses die Fehler verstecken

### HIGH (SOLLTE behoben werden)
- Oeffentliche Funktionen ohne Typ-Hints
- Veraenderliche Standardargumente
- Still schluckende Exceptions
- Ressourcenverwaltung ohne Context-Manager
- C-Style Schleifen statt Comprehensions
- type() statt isinstance() verwenden

### MEDIUM (Empfohlen zu beruecksichtigen)
- PEP 8 Formatverstoesse
- Oeffentliche Funktionen ohne Docstrings
- Print-Anweisungen statt Logging
- Ineffiziente String-Operationen
- Magische Zahlen ohne benannte Konstanten
- f-strings nicht verwendet

**Automatische Pruefbefehle**:
```bash
mypy .                              # Typpruefung
ruff check .                        # Linting
black --check .                     # Formatpruefung
isort --check-only .                # Import-Sortierung
bandit -r .                         # Sicherheitsscan
pip-audit                           # Abhaengigkeitspruefung
pytest --cov=app --cov-report=term-missing  # Testabdeckung
```

**Haeufige Problemreparaturen**:

```python
# SQL Injection Reparatur
# FALSCH
query = f"SELECT * FROM users WHERE id = {user_id}"

# RICHTIG - Parameterisierte Abfrage
query = "SELECT * FROM users WHERE id = %s"
cursor.execute(query, (user_id,))

# Veraenderliche Standardargumente Reparatur
# FALSCH
def process_items(items=[]):
    items.append("new")
    return items

# RICHTIG
def process_items(items=None):
    if items is None:
        items = []
    items.append("new")
    return items

# Context-Manager verwenden
# FALSCH
f = open("config.json")
data = f.read()
f.close()

# RICHTIG
with open("config.json") as f:
    data = f.read()
```

---

## /go-review

**Zweck**: Umfassendes Go Code-Review. Idiomatische Muster, Concurrency-Safety, Fehlerbehandlung und Sicherheit pruefen.

**Verwendung**:
```
/go-review
```

**Anwendungsszenarien**:
- Nach dem Schreiben oder Aendern von Go-Code
- Vor dem Committen von Go-Aenderungen
- Beim Review von PRs die Go-Code enthalten
- Go-idiomatische Muster lernen

**Review-Kategorien**:

### CRITICAL (MUSS behoben werden)
- SQL/Befehls-Injection-Schwachstellen
- Race Conditions ohne Synchronisation
- Goroutine-Leaks
- Hartcodierte Credentials
- Unsichere Zeigerverwendung
- Fehler in kritischen Pfaden ignoriert

### HIGH (SOLLTE behoben werden)
- Fehlender Fehlerkontext-Wrapper
- Panic statt Fehlerreturn
- Context nicht propagiert
- Ungepufferte Channels die Deadlocks verursachen
- Interface nicht implementiert Fehler
- Fehlender Mutex-Schutz

### MEDIUM (Empfohlen zu beruecksichtigen)
- Nicht-idiomatischer Codestil
- Exportierte Funktionen ohne godoc-Kommentare
- Ineffiziente Stringverkettung
- Slices ohne Vorausallokation
- Keine Table-driven Tests verwendet

**Automatische Pruefbefehle**:
```bash
go vet ./...                         # Statische Analyse
staticcheck ./...                    # Erweiterte Pruefung (falls installiert)
golangci-lint run                   # Linting
go build -race ./...                # Race-Detektion
govulncheck ./...                   # Sicherheitsschwachstellen
```

---

## /kotlin-review

**Zweck**: Umfassendes Kotlin Code-Review. Idiomatische Muster, Null-Safety, Koroutinen-Safety und Sicherheit pruefen.

**Verwendung**:
```
/kotlin-review
```

**Anwendungsszenarien**:
- Nach dem Schreiben oder Aendern von Kotlin-Code
- Vor dem Committen von Kotlin-Aenderungen
- Beim Review von Android/KMP-Projekt-PRs
- Kotlin-idiomatische Muster lernen

**Review-Kategorien**:

### CRITICAL (MUSS behoben werden)
- SQL/Befehls-Injection-Schwachstellen
- Force-Unwrap `!!` ohne guten Grund
- Platform-Typ Null-Safety-Verstoesze
- GlobalScope Verwendung (strukturierte Concurrency-Verstoss)
- Hartcodierte Credentials
- Unsichere Deserialisierung

### HIGH (SOLLTE behoben werden)
- Veraenderlicher Zustand (wenn Immutable moeglich)
- Blockierende Aufrufe im Koroutinen-Kontext
- Fehlende Abbruchpruefung in langen Schleifen
- Sealed class `when` nicht exhaustiv
- Grosse Funktionen (>50 Zeilen)
- Tiefe Verschachtelung (>4 Ebenen)

### MEDIUM (Empfohlen zu beruecksichtigen)
- Nicht-idiomatisches Kotlin (Java-Style-Muster)
- Fehlende nachfolgende Kommas
- Scope-Funktion-Missbrauch oder -Verschachtelung
- Grosse Collection-Chains ohne Sequence
- Redundante explizite Typen

**Automatische Pruefbefehle**:
```bash
./gradlew build                      # Build-Pruefung
./gradlew detekt                     # Statische Analyse
./gradlew ktlintCheck                # Formatpruefung
./gradlew test                       # Tests
```

---

## /rust-review

**Zweck**: Umfassendes Rust Code-Review. Ownership-Lebensdauer, Fehlerbehandlung, Unsafe-Nutzung und idiomatische Muster pruefen.

**Verwendung**:
```
/rust-review
```

**Anwendungsszenarien**:
- Nach dem Schreiben oder Aendern von Rust-Code
- Vor dem Committen von Rust-Aenderungen
- Beim Review von Rust-Projekt-PRs
- Rust-idiomatische Muster lernen

**Review-Kategorien**:

### CRITICAL (MUSS behoben werden)
- Ungeprueftes `unwrap()`/`expect()` in Produktionscodepfaden
- `unsafe` ohne `// SAFETY:` Kommentar
- SQL-Injection durch Stringinterpolation
- Ungepruefte Eingabe durch Befehlsinjektion
- Hartcodierte Credentials
- Use-after-free bei rohen Zeigern

### HIGH (SOLLTE behoben werden)
- Unnoetiges `.clone()` um Borrow-Checker zu erfuellen
- `String`-Parameter sollte `&str` oder `impl AsRef<str>` sein
- Blockieren im Async-Kontext (`std::thread::sleep`)
- `Send`/`Sync`-Constraints bei Shared-Typen fehlen
- Wildcard `_ =>` Match bei kritischen Enums
- Grosse Funktionen (>50 Zeilen)

### MEDIUM (Empfohlen zu beruecksichtigen)
- Unnoetige Allokationen im Hot Path
- Fehlendes `with_capacity` bei bekannter Groesse
- Unzureichende Begruendung beim Unterdruecken von Clippy-Warnungen
- Oeffentliche APIs ohne `///` Dokumentation
- Ueberlegung zu `#[must_use]` fuer nicht-must_use-Rueckgabetypen die Werte ignorieren koennten

**Automatische Pruefbefehle**:
```bash
cargo check                           # Build-Pruefung
cargo clippy -- -D warnings            # Lints
cargo fmt --check                      # Formatpruefung
cargo test                            # Tests
cargo audit                           # Sicherheitsaudit (falls verfuegbar)
```

---

## /cpp-review

**Zweck**: Umfassendes C++ Code-Review. Speichersicherheit, moderne C++-Idiome, Concurrency und Sicherheit pruefen.

**Verwendung**:
```
/cpp-review
```

**Anwendungsszenarien**:
- Nach dem Schreiben oder Aendern von C++-Code
- Vor dem Committen von C++-Aenderungen
- Beim Review von C++-Projekt-PRs
- Speichersicherheitsprobleme pruefen

**Review-Kategorien**:

### CRITICAL (MUSS behoben werden)
- Bare `new`/`delete` ohne RAII
- Buffer Overflow und Use-after-free
- Data Races ohne Synchronisation
- Befehlsinjektion durch `system()`
- Uninitialisierte Variablenleser
- Null-Pointer-Dereferenzierung

### HIGH (SOLLTE behoben werden)
- Rule-of-Five-Verstoesze
- Fehlendes `std::lock_guard` / `std::scoped_lock`
- Falsches Lifetime-Management bei detached Threads
- C-Style Casts statt `static_cast`/`dynamic_cast`
- Fehlende `const`-Korrektheit

### MEDIUM (Empfohlen zu beruecksichtigen)
- Unnoetige Kopien (by value statt `const&`)
- Fehlendes `reserve()` bei bekannter Groesse
- `using namespace std;` in Headers
- Wichtige Rueckgabewerte ohne `[[nodiscard]]`
- Zu komplizierte Template-Metaprogrammierung

**Automatische Pruefbefehle**:
```bash
clang-tidy --checks='*,-llvmlibc-*' src/*.cpp -- -std=c++17
cppcheck --enable=all --suppress=missingIncludeSystem src/
cmake --build build -- -Wall -Wextra -Wpedantic
```

---

## /flutter-review

**Zweck**: Flutter/Dart Code-Review. Idiomatische Muster, Komponenten-Best-Practices, State-Management, Performance, Barrierefreiheit und Sicherheit pruefen.

**Verwendung**:
```
/flutter-review
```

**Anwendungsszenarien**:
- Vor dem Committen eines PRs mit Flutter/Dart-Aenderungen (nach erfolgreichem Build und Test)
- Fruehzeitig Probleme finden nach der Implementierung neuer Features
- Flutter-Code von anderen reviewen
- Komponenten, State-Management-Komponenten oder Service-Klassen reviewen
- Vor Produktveroeffentlichung

**Review-Bereiche**:

| Bereich | Schweregrad |
|---------|------------|
| Hartcodierte Schluessel, Klartext-HTTP | CRITICAL |
| Architekturverstoesze, State-Management-Anti-Patterns | CRITICAL |
| Widget-Rebuild-Probleme, Ressourcenlecks | HIGH |
| Fehlendes `dispose()`, BuildContext nach `await` | HIGH |
| Dart Null-Safety, fehlende Error/Loading-States | HIGH |
| Const-Propagation, Widget-Komposition | HIGH |
| Teure Arbeit in `build()` | HIGH |
| Barrierefreiheit, semantische Labels | MEDIUM |
| State-Transitions ohne Tests | HIGH |
| Hartcodierte Strings (l10n) | MEDIUM |

---

## /fastapi-review

**Zweck**: FastAPI-Anwendungs-Review. Architektur, Async-Korrektheit, Dependency Injection, Pydantic-Schemas, Sicherheit, Performance und Testbarkeit pruefen.

**Verwendung**:
```
/fastapi-review [Datei oder Verzeichnis]
```

**Review-Bereiche**:
- App factory, Router-Grenzen, Middleware und Exception-Handler
- Pydantic Request- und Response-Schema-Trennung
- Database Sessions, Authentifizierung, Paginierung und Settings Dependency Injection
- Asynchrone Datenbank- und externe HTTP-Muster
- CORS, Authentifizierung, Rate-Limiting, Logging und Schluesselbehandlung
- OpenAPI-Metadaten und dokumentierte Response-Modelle
- Test-Client-Setup und Dependency-Overrides

---

## Sprachpruefungs-Befehlsvergleichstabelle

| Befehl | Sprache | Wichtige Pruefungen | Schweregrade |
|--------|---------|---------------------|--------------|
| `/python-review` | Python | PEP 8, Typ-Hints, SQL-Injection | CRITICAL/HIGH/MEDIUM |
| `/go-review` | Go | Concurrency-Safety, Fehlerbehandlung, Goroutines | CRITICAL/HIGH/MEDIUM |
| `/kotlin-review` | Kotlin | Null-Safety, Koroutinen, GlobalScope | CRITICAL/HIGH/MEDIUM |
| `/rust-review` | Rust | Ownership, Lebensdauer, Unsafe | CRITICAL/HIGH/MEDIUM |
| `/cpp-review` | C++ | Speichersicherheit, RAII, Concurrency | CRITICAL/HIGH/MEDIUM |
| `/flutter-review` | Flutter | State-Management, Barrierefreiheit, Performance | CRITICAL/HIGH/MEDIUM |
| `/fastapi-review` | Python | Pydantic, DI, Async, Sicherheit | CRITICAL/HIGH/MEDIUM |