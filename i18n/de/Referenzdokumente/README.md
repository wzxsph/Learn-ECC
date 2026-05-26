# ECC-Dokumentationssystem

ECC (Everything Claude Code) ist ein Claude Code-Plugin, das 75 Befehle, 60 Agents, 16 Skills, 7 Regeldokumente und ein vollständiges Hook/MCP-Konfigurationssystem bietet.

---

## 1. ECC-Dokumentationsübersicht

ECC bietet produktionsreife Entwicklungs-Workflow-Unterstützung für Claude Code und deckt mehrere AI-Agent-Frameworks ab (Claude Code, Codex, OpenCode, Cursor, Gemini).

### Technologie-Stack

| Ebene | Technologie | Version |
|------|------------|---------|
| Primäre Sprache | JavaScript/Node.js | >=18 |
| Sekundäre Sprache | Python | >=3.11 |
| Paketmanager | Yarn | 4.9.2 |
| Test-Runner | Benutzerdefinierter Node.js-Test-Runner | - |
| Abdeckung | c8 | 11.x |
| Linting | ESLint + markdownlint-cli | - |

### Verzeichnisstruktur

```
ECC/
├── agents/        # 60 professionelle Agents
├── commands/      # 75 Befehlsdateien
├── skills/        # 16 Skills-Verzeichnisse
├── rules/         # 7 Regeldokumente (common + sprachspezifisch)
├── hooks/         # 3 Hook-Dokumente
├── mcp/           # 6 MCP-Serverkonfigurationen
├── scripts/       # 54 Hilfsskripte
├── README.md
```

---

## 2. Schnellnavigation

| Kategorie | Dokumentation | Beschreibung |
|----------|---------------|--------------|
| [Befehlsübersicht](./commands/) | [commands/](commands/) | Vollständige Liste der 75 Befehle |
| [Agent-Index](./agents/) | [agents/](agents/) | 60 professionelle Agents |
| [Skills-Index](./skills/) | [skills/](skills/) | 16 Skills nach Domäne |
| [Regeln-Index](./rules/) | [rules/](rules/) | 7 Regeldokumente (common + sprachspezifisch) |
| [Hooks-Index](./hooks/) | [hooks/](hooks/) | Hook-Systemkonfiguration und -entwicklung |
| [MCP-Index](./mcp/) | [mcp/](mcp/) | MCP-Serverkonfiguration und -entwicklung |
| [Scripts-Index](./scripts/) | [scripts/](scripts/) | 54 Hilfsskripte |

---

## 3. Befehlsindex

Insgesamt **75 Befehle**, nach Kategorie gruppiert:

### 3.1 Kern-Workflows (6)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/plan` | [01-Kern-Workflow.md](commands/01-Kern-Workflow.md) | Anforderungsanalyse + Risikobewertung + schrittweise Planung |
| `/code-review` | [01-Kern-Workflow.md](commands/01-Kern-Workflow.md) | Codequalität / Sicherheit / Wartbarkeitsprüfung |
| `/build-fix` | [01-Kern-Workflow.md](commands/01-Kern-Workflow.md) | Automatische Spracherkennung + inkrementelle Fehlerbehebung |
| `/verify` | [01-Kern-Workflow.md](commands/01-Kern-Workflow.md) | Vollständiger Verifizierungsablauf: Build → Lint → Test → Typprüfung |
| `/quality-gate` | [01-Kern-Workflow.md](commands/01-Kern-Workflow.md) | ECC-Qualitätspipeline bei Bedarf ausführen |
| `/tdd` | [01-Kern-Workflow.md](commands/01-Kern-Workflow.md) | Allgemeiner TDD-Workflow |

### 3.2 Testen (8)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/go-test` | [02-Testen.md](commands/02-Testen.md) | Go TDD (tabellengesteuert, 80%+ Abdeckung) |
| `/kotlin-test` | [02-Testen.md](commands/02-Testen.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-Testen.md](commands/02-Testen.md) | Rust TDD (cargo test + Integrationstests) |
| `/cpp-test` | [02-Testen.md](commands/02-Testen.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-Testen.md](commands/02-Testen.md) | Flutter TDD |
| `/e2e` | [02-Testen.md](commands/02-Testen.md) | Playwright E2E-Tests generieren + ausführen |
| `/test-coverage` | [02-Testen.md](commands/02-Testen.md) | Testabdeckungsbericht + Lückenanalyse |
| `/python-testing` | [02-Testen.md](commands/02-Testen.md) | Python-Test最佳实践 |

### 3.3 Sprachprüfung (7)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/python-review` | [03-Sprachprüfung.md](commands/03-Sprachprüfung.md) | Python PEP 8, Typhinweise, Sicherheit |
| `/go-review` | [03-Sprachprüfung.md](commands/03-Sprachprüfung.md) | Go-Idiome, Nebenläufigkeit, Fehlerbehandlung |
| `/kotlin-review` | [03-Sprachprüfung.md](commands/03-Sprachprüfung.md) | Kotlin-Nullsicherheit, Koroutinen, Architektur |
| `/rust-review` | [03-Sprachprüfung.md](commands/03-Sprachprüfung.md) | Rust-Besitz, Lebensdauer, Unsicherheit |
| `/cpp-review` | [03-Sprachprüfung.md](commands/03-Sprachprüfung.md) | C++-Speichersicherheit, moderne Idiome |
| `/flutter-review` | [03-Sprachprüfung.md](commands/03-Sprachprüfung.md) | Flutter/Dart-Muster |
| `/fastapi-review` | [03-Sprachprüfung.md](commands/03-Sprachprüfung.md) | FastAPI-Architektur, Async, Pydantic |

### 3.4 Build & Fehlerbehebung (6)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/go-build` | [04-Build-Reparatur.md](commands/04-Build-Reparatur.md) | Go-Build-Fehler beheben + go vet-Warnungen |
| `/kotlin-build` | [04-Build-Reparatur.md](commands/04-Build-Reparatur.md) | Kotlin/Gradle-Compilerfehler beheben |
| `/rust-build` | [04-Build-Reparatur.md](commands/04-Build-Reparatur.md) | Rust-Build + Borrow-Checker-Probleme beheben |
| `/cpp-build` | [04-Build-Reparatur.md](commands/04-Build-Reparatur.md) | C++ CMake + Linker-Probleme beheben |
| `/gradle-build` | [04-Build-Reparatur.md](commands/04-Build-Reparatur.md) | Android/KMP Gradle-Fehler beheben |
| `/flutter-build` | [04-Build-Reparatur.md](commands/04-Build-Reparatur.md) | Flutter-Build-Fixes |

### 3.5 Planung & Architektur (7)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/plan-prd` | [05-Planung-Architektur.md](commands/05-Planung-Architektur.md) | Interaktiver PRD-Generator |
| `/prp-plan` | [05-Planung-Architektur.md](commands/05-Planung-Architektur.md) | Umfassende Featureplanung |
| `/prp-prd` | [05-Planung-Architektur.md](commands/05-Planung-Architektur.md) | PRP-Workflow PRD-Generator |
| `/prp-implement` | [05-Planung-Architektur.md](commands/05-Planung-Architektur.md) | PRP-Plan ausführen + Verifizierungsschleife |
| `/prp-pr` | [05-Planung-Architektur.md](commands/05-Planung-Architektur.md) | PR aus PRP-Workflow erstellen |
| `/prp-commit` | [05-Planung-Architektur.md](commands/05-Planung-Architektur.md) | PRP-verifiziertes Commit |
| `/multi-plan` | [05-Planung-Architektur.md](commands/05-Planung-Architektur.md) | Kollaborative Multi-Modell-Planung (Codex + Gemini) |

### 3.6 Sitzungsverwaltung (5)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/save-session` | [06-Sitzungsverwaltung.md](commands/06-Sitzungsverwaltung.md) | Sitzungszustand speichern |
| `/resume-session` | [06-Sitzungsverwaltung.md](commands/06-Sitzungsverwaltung.md) | Gespeicherte Sitzung laden und wiederherstellen |
| `/sessions` | [06-Sitzungsverwaltung.md](commands/06-Sitzungsverwaltung.md) | Sitzungsverlauf durchsuchen + suchen + verwalten |
| `/checkpoint` | [06-Sitzungsverwaltung.md](commands/06-Sitzungsverwaltung.md) | Workflow-Prüfpunkt erstellen / verifizieren |
| `/aside` | [06-Sitzungsverwaltung.md](commands/06-Sitzungsverwaltung.md) | Seitenfragen beantworten ohne Kontext zu verlieren |

### 3.7 Lernen & Verbesserung (10)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/learn` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Wiederverwendbare Muster aus Sitzung extrahieren |
| `/learn-eval` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Muster extrahieren + Selbstbewertung der Qualität |
| `/evolve` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Instinct analysieren + Evolutionsstruktur vorschlagen |
| `/promote` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Projekt-Instinct auf globale Ebene fördern |
| `/instinct-status` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Alle gelernten Instincts anzeigen |
| `/instinct-export` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Instinct in Datei exportieren |
| `/instinct-import` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Instinct aus Datei / URL importieren |
| `/skill-create` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Git-History analysieren + Skill-Datei generieren |
| `/skill-health` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Skill-Kompositions-Dashboard |
| `/rules-distill` | [07-Lernen-Verbesserung.md](commands/07-Lernen-Verbesserung.md) | Skills scannen + domänenübergreifende Prinzipien extrahieren |

### 3.8 Schleifen & Automatisierung (3)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/loop-start` | [08-Schleifen-Automatisierung.md](commands/08-Schleifen-Automatisierung.md) | Verwalteten autonomen Schleifenmodus starten |
| `/loop-status` | [08-Schleifen-Automatisierung.md](commands/08-Schleifen-Automatisierung.md) | Status der laufenden Schleife prüfen |
| `/santa-loop` | [08-Schleifen-Automatisierung.md](commands/08-Schleifen-Automatisierung.md) | Santa-Art autonome Schleife |

### 3.9 Projektmanagement (6)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/projects` | [09-Projektmanagement.md](commands/09-Projektmanagement.md) | Bekannte Projekte + Instinct-Statistiken auflisten |
| `/harness-audit` | [09-Projektmanagement.md](commands/09-Projektmanagement.md) | Agent-Harness-Konfiguration prüfen |
| `/model-route` | [09-Projektmanagement.md](commands/09-Projektmanagement.md) | Aufgaben an geeignetes Modell weiterleiten |
| `/pm2` | [09-Projektmanagement.md](commands/09-Projektmanagement.md) | PM2-Prozessmanager-Initialisierung |
| `/setup-pm` | [09-Projektmanagement.md](commands/09-Projektmanagement.md) | Paketmanager konfigurieren |
| `/project-init` | [09-Projektmanagement.md](commands/09-Projektmanagement.md) | Projektinitialisierung |

### 3.10 PR-Workflow (6)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/pr` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | GitHub PR aus aktuellem Branch erstellen |
| `/review-pr` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | GitHub PR prüfen |
| `/multi-workflow` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Multi-Modell-kollaborative Entwicklung |
| `/multi-backend` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Backend-Multi-Modell-Entwicklung |
| `/multi-frontend` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Frontend-Multi-Modell-Entwicklung |
| `/multi-execute` | [10-PR-Workflow.md](commands/10-PR-Workflow.md) | Multi-Modell-kollaborative Ausführung |

### 3.11 Hookify-System (4)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/hookify` | [11-Hookify-System.md](commands/11-Hookify-System.md) | Hooks erstellen um schlechtes Verhalten zu verhindern |
| `/hookify-list` | [11-Hookify-System.md](commands/11-Hookify-System.md) | Alle konfigurierten Hookify-Regeln auflisten |
| `/hookify-configure` | [11-Hookify-System.md](commands/11-Hookify-System.md) | Hookify-Regeln interaktiv aktivieren / deaktivieren |
| `/hookify-help` | [11-Hookify-System.md](commands/11-Hookify-System.md) | Hookify-Systemhilfe |

### 3.12 Dokumentation & Forschung (3)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/update-docs` | [12-Dokumentation-Forschung.md](commands/12-Dokumentation-Forschung.md) | Projektdokumentation aktualisieren |
| `/update-codemaps` | [12-Dokumentation-Forschung.md](commands/12-Dokumentation-Forschung.md) | Codemaps neu generieren |
| `/ecc-guide` | [12-Dokumentation-Forschung.md](commands/12-Dokumentation-Forschung.md) | ECC-Benutzerhandbuch |

### 3.13 Refactoring & Bereinigung (2)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/refactor-clean` | [13-Refactoring-Bereinigung.md](commands/13-Refactoring-Bereinigung.md) | Toten Code löschen + Duplikate zusammenführen |
| `/auto-update` | [13-Refactoring-Bereinigung.md](commands/13-Refactoring-Bereinigung.md) | Auto-Update-Fähigkeiten |

### 3.14 Andere Befehle (7)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/jira` | [14-Sonstige-Befehle.md](commands/14-Sonstige-Befehle.md) | Jira-Ticket-Interaktion |
| `/gan-build` | [14-Sonstige-Befehle.md](commands/14-Sonstige-Befehle.md) | GAN-Build-Operationen |
| `/gan-design` | [14-Sonstige-Befehle.md](commands/14-Sonstige-Befehle.md) | GAN-Design-Operationen |
| `/prune` | [14-Sonstige-Befehle.md](commands/14-Sonstige-Befehle.md) | Veraltete Instincts löschen (>30 Tage) |
| `/security-scan` | [14-Sonstige-Befehle.md](commands/14-Sonstige-Befehle.md) | Sicherheitsscan |
| `/feature-dev` | [14-Sonstige-Befehle.md](commands/14-Sonstige-Befehle.md) | Feature-Entwicklungsassistent |
| `/cost-report` | [14-Sonstige-Befehle.md](commands/14-Sonstige-Befehle.md) | Modellkostenbericht |

---

## 4. Agent-Index

Insgesamt **60 Agents**, nach Kategorie gruppiert:

| Agent-Kategorie | Datei | Beschreibung |
|----------------|------|-------------|
| [Code Review](agents/01-Code-Review.md) | [01-Code-Review.md](agents/01-Code-Review.md) | 14 Review-Agents |
| [Build & Fix](agents/02-Build-Fix.md) | [02-Build-Fix.md](agents/02-Build-Fix.md) | 14 Build-Fix-Agents |
| [Planung](agents/03-Planning.md) | [03-Planning.md](agents/03-Planning.md) | 5 Planungs-Agents |
| [Testen](agents/04-Testing.md) | [04-Testing.md](agents/04-Testing.md) | 2 Test-Agents |
| [Sicherheit](agents/05-Security.md) | [05-Security.md](agents/05-Security.md) | 3 Sicherheits-Agents |
| [Architektur](agents/06-Architecture.md) | [06-Architecture.md](agents/06-Architecture.md) | 3 Architektur-Agents |

---

## 5. Skills-Index

**16 Skills-Domänen**, nach Domäne kategorisiert:

| Domäne | Datei | Beschreibung |
|--------|------|-------------|
| [Best Practices](skills/Best-Practices.md) | [Best-Practices.md](skills/Best-Practices.md) | Codierungsstandards, Fehlerbehandlung, autonome Schleifen |
| [Programmiersprachen](skills/Programmiersprachen.md) | [Programmiersprachen.md](skills/Programmiersprachen.md) | Python/Go/Rust/Kotlin/C++ usw. |
| [Frameworks](skills/Frameworks.md) | [Frameworks.md](skills/Frameworks.md) | Django/Laravel/NestJS/Spring Boot usw. |
| [Testen](skills/Testing.md) | [Testing.md](skills/Testing.md) | TDD/Unit-Test/Integrationstest/E2E |
| [Sicherheit](skills/Sicherheit.md) | [Sicherheit.md](skills/Sicherheit.md) | Sicherheitsprüfung, Schwachstellen-Scanning |
| [Frontend & Design](skills/Frontend-Design.md) | [Frontend-Design.md](skills/Frontend-Design.md) | Frontend-Entwicklung, Design-Systeme |
| [Backend & API](skills/Backend-API.md) | [Backend-API.md](skills/Backend-API.md) | Backend-Services, API-Design, Datenbanken |
| [Deployment & DevOps](skills/Deployment-DevOps.md) | [Deployment-DevOps.md](skills/Deployment-DevOps.md) | Docker/K8s/Deploy-Strategien |
| [Monitoring & Observability](skills/Monitoring-Observability.md) | [Monitoring-Observability.md](skills/Monitoring-Observability.md) | Observability, Netzwerkdiagnose |
| [Automatisierung & Skripting](skills/Automatisierung-Skripte.md) | [Automatisierung-Skripte.md](skills/Automatisierung-Skripte.md) | Autonome Schleifen, kontinuierliches Lernen, Agent-Engineering |
| [Suche & Datenabruf](skills/Suche-Datenerhebung.md) | [Suche-Datenerhebung.md](skills/Suche-Datenerhebung.md) | Exa-Suche, Data Scraping, MCP |
| [GitHub & Kollaboration](skills/GitHub-Kollaboration.md) | [GitHub-Kollaboration.md](skills/GitHub-Kollaboration.md) | GitHub-Workflows, Code-Review |
| [AI & Machine Learning](skills/KI-Maschinelles-Lernen.md) | [KI-Maschinelles-Lernen.md](skills/KI-Maschinelles-Lernen.md) | Neuronale Netze, PyTorch, MLOps |
| [Cloud Native & Infrastruktur](skills/Cloud-Native-Infrastruktur.md) | [Cloud-Native-Infrastruktur.md](skills/Cloud-Native-Infrastruktur.md) | Kubernetes, Docker, Terraform |
| [Spezialisierte Skills](skills/Spezialgebiete.md) | [Spezialgebiete.md](skills/Spezialgebiete.md) | Blockchain, Spielentwicklung, Audio/Video, IoT |
| [Entwicklungs-Toolchain](skills/Development-Toolchain.md) | [Development-Toolchain.md](skills/Development-Toolchain.md) | Test-Frameworks, CI/CD, Code-Qualität |
| [Spitzentechnologie](skills/Moderne-Technologien.md) | [Moderne-Technologien.md](skills/Moderne-Technologien.md) | AI-Agent, Quantencomputing, Edge Computing |

---

## 6. Regeln-Index

Insgesamt **7 Regeldokumente** (common + sprachspezifisch):

| Regeln | Datei | Beschreibung |
|-------|------|-------------|
| [Git-Workflow](rules/Git-Workflow.md) | [Git-Workflow.md](rules/Git-Workflow.md) | Git-Commit-Standards und PR-Workflow |
| [Hooks-System](rules/Hooks-System.md) | [Hooks-System.md](rules/Hooks-System.md) | Hook-Konfiguration und Nutzungshandbuch |
| [Agent-Orchestrierung](rules/Agent-Orchestration.md) | [Agent-Orchestration.md](rules/Agent-Orchestration.md) | Agent-Orchestrierungsmuster |
| [Leistungsoptimierung](rules/Performance-Optimization.md) | [Performance-Optimization.md](rules/Performance-Optimization.md) | Leistungsoptimierungshandbuch |
| [Codestil](rules/Coding-Style.md) | [Coding-Style.md](rules/Coding-Style.md) | Codierungsstilstandards |
| [Testregeln](rules/Testing-Rules.md) | [Testing-Rules.md](rules/Testing-Rules.md) | Testanforderungen (80% Abdeckung) |
| [Sicherheitsregeln](rules/Security-Rules.md) | [Security-Rules.md](rules/Security-Rules.md) | Sicherheitscheckliste |

---

## 7. Hooks-Index

Insgesamt **4 Dokumente**:

| Dokument | Datei | Beschreibung |
|----------|------|-------------|
| [Hook-Typen](hooks/Hook-Types.md) | [Hook-Types.md](hooks/Hook-Types.md) | PreToolUse, PostToolUse, Stop-Typen |
| [Eingebaute Hooks](hooks/Built-in-Hooks.md) | [Built-in-Hooks.md](hooks/Built-in-Hooks.md) | Eingebaute Hook-Liste und Nutzung |
| [Benutzerdefinierte Entwicklung](hooks/Custom-Development.md) | [Custom-Development.md](hooks/Custom-Development.md) | Benutzerdefinierte Hook-Entwicklungshandbuch |
| [Konfigurationsformat](hooks/Configuration-Format.md) | [Configuration-Format.md](hooks/Configuration-Format.md) | hooks.json-Konfigurationsformat |

---

## 8. MCP-Index

Insgesamt **6 MCP-Serverkonfigurationen**:

| Dokument | Datei | Beschreibung |
|----------|------|-------------|
| [MCP-Konfigurationsformat](mcp/MCP-Configuration-Format.md) | [MCP-Configuration-Format.md](mcp/MCP-Configuration-Format.md) | MCP-Konfigurationsdateiformat |
| [Eingebaute Server](mcp/Built-in-Servers.md) | [Built-in-Servers.md](mcp/Built-in-Servers.md) | Eingebaute MCP-Server |
| [Benutzerdefinierte Entwicklung](mcp/Custom-Development.md) | [Custom-Development.md](mcp/Custom-Development.md) | Benutzerdefinierte MCP-Serverentwicklung |

---

## 9. Scripts-Index

Insgesamt **54 Hilfsskripte**:

| Dokument | Datei | Beschreibung |
|----------|------|-------------|
| [Hilfsskripte](scripts/Tool-Scripts.md) | [Tool-Scripts.md](scripts/Tool-Scripts.md) | ecc.js, install-apply.js usw. |
| [Hilfsfunktionsbibliothek](scripts/Utility-Functions.md) | [Utility-Functions.md](scripts/Utility-Functions.md) | Gemeinsame Funktionsbibliothek |
| [Test-Runner](scripts/Test-Runner.md) | [Test-Runner.md](scripts/Test-Runner.md) | Test-Runner-Nutzung |
| [Build-Skripte](scripts/Build-Scripts.md) | [Build-Scripts.md](scripts/Build-Scripts.md) | Build-Skripte |

---

## 10. Beitragsleitfaden

### Dateibenennungskonventionen

- **Befehle, Skills, Agents, Hooks**: Kleinbuchstaben + Bindestrich (z.B. `code-review.md`)
- **Skripte**: camelCase oder kebab-case (z.B. `session-start.js`)
- **Regeln**: Nach Sprache / Thema organisiert

### Befehlsdateiformat

```yaml
---
description: "Kurze Beschreibung"
argument-hint: "[Optionales Argument]"
name: command-name
command: true
allowed_tools: ["Bash"]
---
```

### Agent-Dateiformat

```yaml
---
name: agent-name
description: Agent-Zweck
tools: ["Read", "Grep", "Glob", "Bash"]
model: sonnet
---
```

### Skill-Dateiformat

```markdown
# Skill-Name

## When to Use
## How It Works
## Examples
```

### Einreichungsprozess

1. Neue Datei erstellen oder vorhandene Datei ändern
2. Namenskonventionen einhalten
3. Tests ausführen: `node tests/run-all.js`
4. Markdown-Lint ausführen: `npx markdownlint-cli '**/*.md' --ignore node_modules`
5. PR zur Überprüfung erstellen

---

*ECC-Dokumentationssystem - Produktionsreife Entwicklungs-Workflows für Claude Code*