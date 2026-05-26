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
| `/plan` | [01-核心工作流.md](commands/01-核心工作流.md) | Anforderungsanalyse + Risikobewertung + schrittweise Planung |
| `/code-review` | [01-核心工作流.md](commands/01-核心工作流.md) | Codequalität / Sicherheit / Wartbarkeitsprüfung |
| `/build-fix` | [01-核心工作流.md](commands/01-核心工作流.md) | Automatische Spracherkennung + inkrementelle Fehlerbehebung |
| `/verify` | [01-核心工作流.md](commands/01-核心工作流.md) | Vollständiger Verifizierungsablauf: Build → Lint → Test → Typprüfung |
| `/quality-gate` | [01-核心工作流.md](commands/01-核心工作流.md) | ECC-Qualitätspipeline bei Bedarf ausführen |
| `/tdd` | [01-核心工作流.md](commands/01-核心工作流.md) | Allgemeiner TDD-Workflow |

### 3.2 Testen (8)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/go-test` | [02-测试相关.md](commands/02-测试相关.md) | Go TDD (tabellengesteuert, 80%+ Abdeckung) |
| `/kotlin-test` | [02-测试相关.md](commands/02-测试相关.md) | Kotlin TDD (Kotest + Kover) |
| `/rust-test` | [02-测试相关.md](commands/02-测试相关.md) | Rust TDD (cargo test + Integrationstests) |
| `/cpp-test` | [02-测试相关.md](commands/02-测试相关.md) | C++ TDD (GoogleTest + gcov/lcov) |
| `/flutter-test` | [02-测试相关.md](commands/02-测试相关.md) | Flutter TDD |
| `/e2e` | [02-测试相关.md](commands/02-测试相关.md) | Playwright E2E-Tests generieren + ausführen |
| `/test-coverage` | [02-测试相关.md](commands/02-测试相关.md) | Testabdeckungsbericht + Lückenanalyse |
| `/python-testing` | [02-测试相关.md](commands/02-测试相关.md) | Python-Test最佳实践 |

### 3.3 Sprachprüfung (7)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/python-review` | [03-语言审查.md](commands/03-语言审查.md) | Python PEP 8, Typhinweise, Sicherheit |
| `/go-review` | [03-语言审查.md](commands/03-语言审查.md) | Go-Idiome, Nebenläufigkeit, Fehlerbehandlung |
| `/kotlin-review` | [03-语言审查.md](commands/03-语言审查.md) | Kotlin-Nullsicherheit, Koroutinen, Architektur |
| `/rust-review` | [03-语言审查.md](commands/03-语言审查.md) | Rust-Besitz, Lebensdauer, Unsicherheit |
| `/cpp-review` | [03-语言审查.md](commands/03-语言审查.md) | C++-Speichersicherheit, moderne Idiome |
| `/flutter-review` | [03-语言审查.md](commands/03-语言审查.md) | Flutter/Dart-Muster |
| `/fastapi-review` | [03-语言审查.md](commands/03-语言审查.md) | FastAPI-Architektur, Async, Pydantic |

### 3.4 Build & Fehlerbehebung (6)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/go-build` | [04-构建修复.md](commands/04-构建修复.md) | Go-Build-Fehler beheben + go vet-Warnungen |
| `/kotlin-build` | [04-构建修复.md](commands/04-构建修复.md) | Kotlin/Gradle-Compilerfehler beheben |
| `/rust-build` | [04-构建修复.md](commands/04-构建修复.md) | Rust-Build + Borrow-Checker-Probleme beheben |
| `/cpp-build` | [04-构建修复.md](commands/04-构建修复.md) | C++ CMake + Linker-Probleme beheben |
| `/gradle-build` | [04-构建修复.md](commands/04-构建修复.md) | Android/KMP Gradle-Fehler beheben |
| `/flutter-build` | [04-构建修复.md](commands/04-构建修复.md) | Flutter-Build-Fixes |

### 3.5 Planung & Architektur (7)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/plan-prd` | [05-规划与架构.md](commands/05-规划与架构.md) | Interaktiver PRD-Generator |
| `/prp-plan` | [05-规划与架构.md](commands/05-规划与架构.md) | Umfassende Featureplanung |
| `/prp-prd` | [05-规划与架构.md](commands/05-规划与架构.md) | PRP-Workflow PRD-Generator |
| `/prp-implement` | [05-规划与架构.md](commands/05-规划与架构.md) | PRP-Plan ausführen + Verifizierungsschleife |
| `/prp-pr` | [05-规划与架构.md](commands/05-规划与架构.md) | PR aus PRP-Workflow erstellen |
| `/prp-commit` | [05-规划与架构.md](commands/05-规划与架构.md) | PRP-verifiziertes Commit |
| `/multi-plan` | [05-规划与架构.md](commands/05-规划与架构.md) | Kollaborative Multi-Modell-Planung (Codex + Gemini) |

### 3.6 Sitzungsverwaltung (5)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/save-session` | [06-会话管理.md](commands/06-会话管理.md) | Sitzungszustand speichern |
| `/resume-session` | [06-会话管理.md](commands/06-会话管理.md) | Gespeicherte Sitzung laden und wiederherstellen |
| `/sessions` | [06-会话管理.md](commands/06-会话管理.md) | Sitzungsverlauf durchsuchen + suchen + verwalten |
| `/checkpoint` | [06-会话管理.md](commands/06-会话管理.md) | Workflow-Prüfpunkt erstellen / verifizieren |
| `/aside` | [06-会话管理.md](commands/06-会话管理.md) | Seitenfragen beantworten ohne Kontext zu verlieren |

### 3.7 Lernen & Verbesserung (10)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/learn` | [07-学习与改进.md](commands/07-学习与改进.md) | Wiederverwendbare Muster aus Sitzung extrahieren |
| `/learn-eval` | [07-学习与改进.md](commands/07-学习与改进.md) | Muster extrahieren + Selbstbewertung der Qualität |
| `/evolve` | [07-学习与改进.md](commands/07-学习与改进.md) | Instinct analysieren + Evolutionsstruktur vorschlagen |
| `/promote` | [07-学习与改进.md](commands/07-学习与改进.md) | Projekt-Instinct auf globale Ebene fördern |
| `/instinct-status` | [07-学习与改进.md](commands/07-学习与改进.md) | Alle gelernten Instincts anzeigen |
| `/instinct-export` | [07-学习与改进.md](commands/07-学习与改进.md) | Instinct in Datei exportieren |
| `/instinct-import` | [07-学习与改进.md](commands/07-学习与改进.md) | Instinct aus Datei / URL importieren |
| `/skill-create` | [07-学习与改进.md](commands/07-学习与改进.md) | Git-History analysieren + Skill-Datei generieren |
| `/skill-health` | [07-学习与改进.md](commands/07-学习与改进.md) | Skill-Kompositions-Dashboard |
| `/rules-distill` | [07-学习与改进.md](commands/07-学习与改进.md) | Skills scannen + domänenübergreifende Prinzipien extrahieren |

### 3.8 Schleifen & Automatisierung (3)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/loop-start` | [08-循环与自动化.md](commands/08-循环与自动化.md) | Verwalteten autonomen Schleifenmodus starten |
| `/loop-status` | [08-循环与自动化.md](commands/08-循环与自动化.md) | Status der laufenden Schleife prüfen |
| `/santa-loop` | [08-循环与自动化.md](commands/08-循环与自动化.md) | Santa-Art autonome Schleife |

### 3.9 Projektmanagement (6)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/projects` | [09-项目管理.md](commands/09-项目管理.md) | Bekannte Projekte + Instinct-Statistiken auflisten |
| `/harness-audit` | [09-项目管理.md](commands/09-项目管理.md) | Agent-Harness-Konfiguration prüfen |
| `/model-route` | [09-项目管理.md](commands/09-项目管理.md) | Aufgaben an geeignetes Modell weiterleiten |
| `/pm2` | [09-项目管理.md](commands/09-项目管理.md) | PM2-Prozessmanager-Initialisierung |
| `/setup-pm` | [09-项目管理.md](commands/09-项目管理.md) | Paketmanager konfigurieren |
| `/project-init` | [09-项目管理.md](commands/09-项目管理.md) | Projektinitialisierung |

### 3.10 PR-Workflow (6)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/pr` | [10-PR工作流.md](commands/10-PR工作流.md) | GitHub PR aus aktuellem Branch erstellen |
| `/review-pr` | [10-PR工作流.md](commands/10-PR工作流.md) | GitHub PR prüfen |
| `/multi-workflow` | [10-PR工作流.md](commands/10-PR工作流.md) | Multi-Modell-kollaborative Entwicklung |
| `/multi-backend` | [10-PR工作流.md](commands/10-PR工作流.md) | Backend-Multi-Modell-Entwicklung |
| `/multi-frontend` | [10-PR工作流.md](commands/10-PR工作流.md) | Frontend-Multi-Modell-Entwicklung |
| `/multi-execute` | [10-PR工作流.md](commands/10-PR工作流.md) | Multi-Modell-kollaborative Ausführung |

### 3.11 Hookify-System (4)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/hookify` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Hooks erstellen um schlechtes Verhalten zu verhindern |
| `/hookify-list` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Alle konfigurierten Hookify-Regeln auflisten |
| `/hookify-configure` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Hookify-Regeln interaktiv aktivieren / deaktivieren |
| `/hookify-help` | [11-Hookify系统.md](commands/11-Hookify系统.md) | Hookify-Systemhilfe |

### 3.12 Dokumentation & Forschung (3)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/update-docs` | [12-文档与研究.md](commands/12-文档与研究.md) | Projektdokumentation aktualisieren |
| `/update-codemaps` | [12-文档与研究.md](commands/12-文档与研究.md) | Codemaps neu generieren |
| `/ecc-guide` | [12-文档与研究.md](commands/12-文档与研究.md) | ECC-Benutzerhandbuch |

### 3.13 Refactoring & Bereinigung (2)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/refactor-clean` | [13-重构与清理.md](commands/13-重构与清理.md) | Toten Code löschen + Duplikate zusammenführen |
| `/auto-update` | [13-重构与清理.md](commands/13-重构与清理.md) | Auto-Update-Fähigkeiten |

### 3.14 Andere Befehle (7)

| Befehl | Datei | Zweck |
|--------|------|-------|
| `/jira` | [14-其他命令.md](commands/14-其他命令.md) | Jira-Ticket-Interaktion |
| `/gan-build` | [14-其他命令.md](commands/14-其他命令.md) | GAN-Build-Operationen |
| `/gan-design` | [14-其他命令.md](commands/14-其他命令.md) | GAN-Design-Operationen |
| `/prune` | [14-其他命令.md](commands/14-其他命令.md) | Veraltete Instincts löschen (>30 Tage) |
| `/security-scan` | [14-其他命令.md](commands/14-其他命令.md) | Sicherheitsscan |
| `/feature-dev` | [14-其他命令.md](commands/14-其他命令.md) | Feature-Entwicklungsassistent |
| `/cost-report` | [14-其他命令.md](commands/14-其他命令.md) | Modellkostenbericht |

---

## 4. Agent-Index

Insgesamt **60 Agents**, nach Kategorie gruppiert:

| Agent-Kategorie | Datei | Beschreibung |
|----------------|------|-------------|
| [Code Review](agents/1.%20代码审查类.md) | [1. 代码审查类.md](agents/1.%20代码审查类.md) | 14 Review-Agents |
| [Build & Fix](agents/2.%20构建修复类.md) | [2. 构建修复类.md](agents/2.%20构建修复类.md) | 14 Build-Fix-Agents |
| [Planung](agents/3.%20规划类.md) | [3. 规划类.md](agents/3.%20规划类.md) | 5 Planungs-Agents |
| [Testen](agents/4.%20测试类.md) | [4. 测试类.md](agents/4.%20测试类.md) | 2 Test-Agents |
| [Sicherheit](agents/5.%20安全类.md) | [5. 安全类.md](agents/5.%20安全类.md) | 3 Sicherheits-Agents |
| [Architektur](agents/6.%20架构类.md) | [6. 架构类.md](agents/6.%20架构类.md) | 3 Architektur-Agents |

---

## 5. Skills-Index

**16 Skills-Domänen**, nach Domäne kategorisiert:

| Domäne | Datei | Beschreibung |
|--------|------|-------------|
| [Best Practices](skills/最佳实践.md) | [最佳实践.md](skills/最佳实践.md) | Codierungsstandards, Fehlerbehandlung, autonome Schleifen |
| [Programmiersprachen](skills/编程语言.md) | [编程语言.md](skills/编程语言.md) | Python/Go/Rust/Kotlin/C++ usw. |
| [Frameworks](skills/框架.md) | [框架.md](skills/框架.md) | Django/Laravel/NestJS/Spring Boot usw. |
| [Testen](skills/测试.md) | [测试.md](skills/测试.md) | TDD/Unit-Test/Integrationstest/E2E |
| [Sicherheit](skills/安全.md) | [安全.md](skills/安全.md) | Sicherheitsprüfung, Schwachstellen-Scanning |
| [Frontend & Design](skills/前端与设计.md) | [前端与设计.md](skills/前端与设计.md) | Frontend-Entwicklung, Design-Systeme |
| [Backend & API](skills/后端与API.md) | [后端与API.md](skills/后端与API.md) | Backend-Services, API-Design, Datenbanken |
| [Deployment & DevOps](skills/部署与DevOps.md) | [部署与DevOps.md](skills/部署与DevOps.md) | Docker/K8s/Deploy-Strategien |
| [Monitoring & Observability](skills/监控与可观测性.md) | [监控与可观测性.md](skills/监控与可观测性.md) | Observability, Netzwerkdiagnose |
| [Automatisierung & Skripting](skills/自动化与脚本.md) | [自动化与脚本.md](skills/自动化与脚本.md) | Autonome Schleifen, kontinuierliches Lernen, Agent-Engineering |
| [Suche & Datenabruf](skills/搜索与数据获取.md) | [搜索与数据获取.md](skills/搜索与数据获取.md) | Exa-Suche, Data Scraping, MCP |
| [GitHub & Kollaboration](skills/GitHub与协作.md) | [GitHub与协作.md](skills/GitHub与协作.md) | GitHub-Workflows, Code-Review |
| [AI & Machine Learning](skills/AI与机器学习.md) | [AI与机器学习.md](skills/AI与机器学习.md) | Neuronale Netze, PyTorch, MLOps |
| [Cloud Native & Infrastruktur](skills/云原生与基础设施.md) | [云原生与基础设施.md](skills/云原生与基础设施.md) | Kubernetes, Docker, Terraform |
| [Spezialisierte Skills](skills/特殊领域技能.md) | [特殊领域技能.md](skills/特殊领域技能.md) | Blockchain, Spielentwicklung, Audio/Video, IoT |
| [Entwicklungs-Toolchain](skills/开发工具链.md) | [开发工具链.md](skills/开发工具链.md) | Test-Frameworks, CI/CD, Code-Qualität |
| [Spitzentechnologie](skills/前沿技术.md) | [前沿技术.md](skills/前沿技术.md) | AI-Agent, Quantencomputing, Edge Computing |

---

## 6. Regeln-Index

Insgesamt **7 Regeldokumente** (common + sprachspezifisch):

| Regeln | Datei | Beschreibung |
|-------|------|-------------|
| [Git-Workflow](rules/Git工作流.md) | [Git工作流.md](rules/Git工作流.md) | Git-Commit-Standards und PR-Workflow |
| [Hooks-System](rules/Hooks系统.md) | [Hooks系统.md](rules/Hooks系统.md) | Hook-Konfiguration und Nutzungshandbuch |
| [Agent-Orchestrierung](rules/代理编排.md) | [代理编排.md](rules/代理编排.md) | Agent-Orchestrierungsmuster |
| [Leistungsoptimierung](rules/性能优化.md) | [性能优化.md](rules/性能优化.md) | Leistungsoptimierungshandbuch |
| [Codestil](rules/代码风格.md) | [代码风格.md](rules/代码风格.md) | Codierungsstilstandards |
| [Testregeln](rules/测试规则.md) | [测试规则.md](rules/测试规则.md) | Testanforderungen (80% Abdeckung) |
| [Sicherheitsregeln](rules/安全规则.md) | [安全规则.md](rules/安全规则.md) | Sicherheitscheckliste |

---

## 7. Hooks-Index

Insgesamt **4 Dokumente**:

| Dokument | Datei | Beschreibung |
|----------|------|-------------|
| [Hook-Typen](hooks/Hook类型.md) | [Hook类型.md](hooks/Hook类型.md) | PreToolUse, PostToolUse, Stop-Typen |
| [Eingebaute Hooks](hooks/内置Hooks.md) | [内置Hooks.md](hooks/内置Hooks.md) | Eingebaute Hook-Liste und Nutzung |
| [Benutzerdefinierte Entwicklung](hooks/自定义开发.md) | [自定义开发.md](hooks/自定义开发.md) | Benutzerdefinierte Hook-Entwicklungshandbuch |
| [Konfigurationsformat](hooks/配置格式.md) | [配置格式.md](hooks/配置格式.md) | hooks.json-Konfigurationsformat |

---

## 8. MCP-Index

Insgesamt **6 MCP-Serverkonfigurationen**:

| Dokument | Datei | Beschreibung |
|----------|------|-------------|
| [MCP-Konfigurationsformat](mcp/MCP配置格式.md) | [MCP配置格式.md](mcp/MCP配置格式.md) | MCP-Konfigurationsdateiformat |
| [Eingebaute Server](mcp/内置服务器.md) | [内置服务器.md](mcp/内置服务器.md) | Eingebaute MCP-Server |
| [Benutzerdefinierte Entwicklung](mcp/自定义开发.md) | [自定义开发.md](mcp/自定义开发.md) | Benutzerdefinierte MCP-Serverentwicklung |

---

## 9. Scripts-Index

Insgesamt **54 Hilfsskripte**:

| Dokument | Datei | Beschreibung |
|----------|------|-------------|
| [Hilfsskripte](scripts/工具脚本.md) | [工具脚本.md](scripts/工具脚本.md) | ecc.js, install-apply.js usw. |
| [Hilfsfunktionsbibliothek](scripts/工具函数库.md) | [工具函数库.md](scripts/工具函数库.md) | Gemeinsame Funktionsbibliothek |
| [Test-Runner](scripts/测试运行器.md) | [测试运行器.md](scripts/测试运行器.md) | Test-Runner-Nutzung |
| [Build-Skripte](scripts/构建脚本.md) | [构建脚本.md](scripts/构建脚本.md) | Build-Skripte |

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