# Learn-ECC

> Ein strukturierter Lernpfad zur Beherrschung von ECC (Everything Claude Code)

Willkommen bei Learn-ECC! Dies ist ein systematischer Lernpfad für Claude Code, der dich vom Einsteiger zum versierten Benutzer führt.

---

## 🌐 Language / 语言 / 語言 / Dil / Язык / Ngôn ngữ

English | Português (Brasil) | 简体中文 | 繁體中文 | 日本語 | 한국어 | Türkçe | Русский | Tiếng Việt | ไทย

| [🇺🇸 English](./en/README.md) | [🇯🇵 日本語](./ja/README.md) | [🇰🇷 한국어](./ko/README.md) | [🇩🇪 Deutsch](./README.md) |
|:---:|:---:|:---:|:---:|
| [🇷🇺 Русский](./ru/README.md) | [🇹🇷 Türkçe](./tr/README.md) | [🇧🇷 Português](./pt-BR/README.md) | [🇻🇳 Tiếng Việt](./vi/README.md) |
| [🇹🇭 ไทย](./th/README.md) | [🇨🇳 简体中文](../README.md) | | | |
|---

> **⚠️ Hinweis zu mehrsprachigen Versionen**
> 
> Übersetzungsdokumente in anderen Sprachen (Englisch, Japanisch, Koreanisch, Russisch, Türkisch, Portugiesisch, Vietnamesisch, Thai) enthalten zahlreiche Übersetzungsfehler und Probleme. Aufgrund persönlicher Einschränkungen kann ich diese Inhalte nicht weiter pflegen und korrigieren.
> 
> Wenn Sie helfen möchten, eine bestimmte Sprache zu korrigieren, reichen Sie bitte einen Pull Request ein.
> 
> **Derzeit wird nur die chinesische Version aktiv gepflegt.**

---

---

## Hintergrund

### Warum gibt es dieses Projekt?

Ich hatte Schwierigkeiten mit der englischen ECC-Dokumentation. Deshalb habe ich die Erklärungen zu ECC-Befehlen (Commands), Agents, Skills usw. ins Chinesische übersetzt und als Lernkurs zusammengestellt.

Die Referenzdokumentation ist genau das - eine Übersetzung des ECC-Projekts, die erklärt, was jeder Befehl und jeder Agent tut.

### Woher stammt der Kurs?

Ehrlich gesagt, wurde dieser Kurs von KI generiert. Die KI erstellte den Inhalt basierend auf der ECC-Dateistruktur, aber es fehlen detaillierte Überlegungen und praktische Bewertungen. Wenn du Probleme findest oder der Inhalt nicht klar genug ist, freue ich mich über Verbesserungsvorschläge.

---

## Kernkonzepte

### Was ist ECC?

**ECC** (Everything Claude Code) ist ein Claude Code Plugin-Projekt ([Original-Repository](https://github.com/affaan-m/ECC)), das bietet:
- **75 Befehle** - Schrägstrichbefehle (wie `/plan`, `/code-review`) und Skriptbefehle
- **60 professionelle Agents** - Code-Review, Build-Fixes, Architekturdesign usw.
- **232+ Skills** - Wiederverwendbare Workflow-Vorlagen
- **Hooks-System** - Event-gesteuerter Automatisierungsmechanismus
- **Rules** - Verbindliche Entwicklungsrichtlinien

### Was ist Learn-ECC?

**Learn-ECC** ist ein von mir erstellter systematischer Lernpfad zum Erlernen von ECC - es ist nicht ECC selbst.

Er verwendet das "Learning by Doing"-Konzept, wobei jede Phase theoretisches Lernen und praktische Übungen umfasst. Durch die Durchführung realer Projekte wirst du die Kernfähigkeiten von Claude Code beherrschen.

### Was ist die Referenzdokumentation?

**Referenzdokumentation** (./Referenzdokumente/) ist eine chinesische Übersetzung des ECC-Projekts. Ich habe die englische ECC-Dokumentation ins Chinesische übersetzt, um sie zum Nachschlagen zugänglich zu machen.

Inhalt umfasst:
- Vollständige Befehlsbeschreibungen (was jeder Befehl tut)
- Detaillierte Einführungen aller Agents
- Skills-Verzeichnisstruktur und Schreibstandards
- Vollständige Dokumentation des Hooks-Systems
- Best Practices und Design Patterns

---

## Lernpfad-Übersicht

| Phase | Inhalt | Dauer | Schwierigkeit |
|------|--------|-------|---------------|
| [1-Einstieg](./1-Einstieg/README.md) | ECC-Befehle, Agents, Skills-Einführung + Installation | 2-3 Stunden | Einsteiger |
| [2-Kernfähigkeiten](./2-Kernfähigkeiten/README.md) | Befehlssystem, Agent-Zusammenarbeit, Hooks-Anpassung, Skills-Entwicklung, Rules-Erstellung | 8-12 Stunden | Fortgeschritten |
| [3-Expertenweg](./3-Expertenweg/README.md) | MCP-Entwicklung, Autonome Agents, Fortgeschrittene Patterns | 6-8 Stunden | Experte |
| [4-PraktischeProjekte](./4-PraktischeProjekte/README.md) | CLI-Tools, Code-Review, Automatisierung, Agent-Systeme | 10-15 Stunden | Praktisch |
| [5-Schnellreferenz](./5-Schnellreferenz/README.md) | Befehls-Cheatsheet, Agent-Cheatsheet, Workflow-Cheatsheet | Jederzeit | Referenz |

---

## Schnelle Navigation

### Phase 1: Einsteiger
- [Kurs-Einführung](./1-Einstieg/01-ECC-Einführung.md) - Was ist ECC, was kann Learn-ECC
- [Installation](./1-Einstieg/02-Installation.md) - Schneller Einstieg
- [Grundbefehle](./1-Einstieg/03-Grundbefehle.md) - Übersicht der häufigsten Befehle
- [Erste Aufgabe](./1-Einstieg/04-Erste-Aufgabe.md) - Deine erste Aufgabe abschließen
- [Einsteiger-Übungen](./1-Einstieg/exercises/Einstiegsübungen.md) - Grundlagen festigen

### Phase 2: Kernfähigkeiten
- [Befehlssystem](./2-Kernfähigkeiten/01-Befehlssystem/README.md) - Alle Befehlstypen beherrschen
- [Agent-Zusammenarbeit](./2-Kernfähigkeiten/02-Agent-Zusammenarbeit/README.md) - Multi-Agent-Systemdesign
- [Hooks-Anpassung](./2-Kernfähigkeiten/03-Hooks-Anpassung/README.md) - Workflow-Automatisierung
- [Skills-Entwicklung](./2-Kernfähigkeiten/04-Skills-Entwicklung/README.md) - Wiederverwendbare Fähigkeiten bauen
- [Rules-Erstellung](./2-Kernfähigkeiten/05-Rules-Erstellung/README.md) - Entwicklungsrichtlinien anpassen

### Phase 3: Expertenweg
- [MCP-Entwicklung](./3-Expertenweg/01-MCP-Entwicklung.md) - Model Context Protocol
- [Autonome Agents](./3-Expertenweg/02-Autonome-Agenten.md) - Autonome Agents bauen
- [Fortgeschrittene Patterns](./3-Expertenweg/03-Fortgeschrittene-Muster.md) - Design Patterns und Best Practices

### Phase 4: Praktische Projekte
- [Projekt 1: CLI-Tool](./4-PraktischeProjekte/project-1-cli.md) - Kommandozeilen-Tools bauen
- [Projekt 2: Code-Review](./4-PraktischeProjekte/project-2-review.md) - Automatisierter Review-Workflow
- [Projekt 3: Automatisierung](./4-PraktischeProjekte/project-3-auto.md) - End-to-End-Automatisierung
- [Projekt 4: Agent-System](./4-PraktischeProjekte/project-4-agent.md) - Multi-Agent-Kollaborationsplattform

### Phase 5: Schnellreferenz
- [Befehls-Cheatsheet](./5-Schnellreferenz/cheatsheet-commands.md)
- [Agent-Cheatsheet](./5-Schnellreferenz/cheatsheet-agents.md)
- [Workflow-Cheatsheet](./5-Schnellreferenz/cheatsheet-workflows.md)

---

## Wie man diesen Kurs nutzt

### Empfohlene Lernreihenfolge

1. **Sequentiell nach Phase lernen** - Jede Phase baut auf der vorherigen auf
2. **Theorie + Praxis kombinieren** - Jedes Kapitel hat zugehörige Übungen, erst verstehen dann动手
3. **Alle Übungen abschließen** - Übungen sind der Kern des Lernpfads

### Lernempfehlungen

- **Einsteiger-Phase**: 2-3 Stunden empfohlen, konzentriere dich auf grundlegende Konzepte
- **Kernfähigkeiten**: 8-12 Stunden empfohlen, jede Einheit praktisch durchführen
- **Expertenweg**: 6-8 Stunden empfohlen, erfordert Projekterfahrung
- **Praktische Projekte**: 10-15 Stunden empfohlen, wähle interessante Projekte zum Vertiefen

### Lern-Kontrollpunkte

Nach jeder Phase bestätige, dass du beherrschst:
- Alle Operationen der Phase selbstständig durchführen kannst
- Kernkonzepte anderen erklären kannst
- Wissen auf andere Szenarien anwenden kannst

---

## Voraussetzungen

### Grundlagenwissen
- Command-Line-Grundlagen (Grundbefehle kennen)
- JavaScript/Node.js-Grundlagen (einige fortgeschrittene Funktionen)
- Git-Grundlagen (Versionskontrolle)

### Umgebungsanforderungen
- Node.js >= 18
- npm/pnpm/yarn/bun einer davon
- Texteditor (VS Code empfohlen)

### Vorkurse
Keine. Die Einsteiger-Phase ist für Null-Grundlagen konzipiert.

---

## Lernpfad-Diagramm

```
┌─────────────────────────────────────────────────────────────────┐
│                      Learn-ECC Lernpfad                         │
└─────────────────────────────────────────────────────────────────┘

    Phase 1: Einsteiger
        │
        ↓
    ┌───────────────────────┐
    │   Phase 2: Kernfähigkeiten│←─────────────────┐
    │  (Befehle/Agent/Hooks/    │                  │
    │   Skills/Rules)          │                  │
    └───────────┬───────────┘                     │
                │                                │
                ↓                                │
    ┌───────────────────────┐                     │
    │   Phase 3: Expertenweg │                     │
    │  (MCP/Autonome Agents/ │                     │
    │   Fortgeschrittene Patterns)                 │
    └───────────┬───────────┘                     │
                │                                │
                ↓                                │
    ┌───────────────────────┐                     │
    │   Phase 4: Praktische │────────────────────┘
    │   Projekte            │
    │  (CLI/Review/Automation/│
    │   Agent)              │
    └───────────┬───────────┘
                │
                ↓
    ┌───────────────────────┐
    │   Phase 5: Schnellreferenz│ ← Jederzeit verfügbar
    │  (Befehls/Agent-Cheatsheets)
    └───────────────────────┘
```

**Geschätzte Gesamtlernzeit**: 26-38 Stunden (ca. 2-3 Wochen)

---

## Verzeichnisstruktur

```
Learn-ECC/
├── 1-Einstieg/                         # Phase 1: Einsteiger (2-3 Stunden)
│   ├── 01-ECC-Einführung.md               # ECC-Übersicht, Kernwert, Architektur
│   ├── 02-Installation.md              # Umgebungsvorbereitung, Installation, Verifizierung
│   ├── 03-Grundbefehle.md              # /plan, /code-review, /build-fix
│   ├── 04-Erste-Aufgabe.md           # Vollständiges Entwicklungsbeispiel
│   └── exercises/
│       └── Einstiegsübungen.md             # 4 Einsteiger-Übungen
│
├── 2-Kernfähigkeiten/                     # Phase 2: Kernfähigkeiten (8-12 Stunden)
│   ├── 01-命令系统精通/
│   ├── 02-Agent协作/
│   ├── 03-Hooks定制/
│   ├── 04-Skills开发/
│   └── 05-Rules编写/
│
├── 3-Expertenweg/                     # Phase 3: Expertenweg (6-8 Stunden)
│   ├── 01-MCP-Entwicklung.md
│   ├── 02-Autonome-Agenten.md
│   └── 03-Fortgeschrittene-Muster.md
│
├── 4-PraktischeProjekte/                     # Phase 4: Praktische Projekte (10-15 Stunden)
│   ├── project-1-cli.md           # CLI-Tool-Entwicklung
│   ├── project-2-review.md        # Automatisierte Code-Review-Pipeline
│   ├── project-3-auto.md          # Workflow-Automatisierung
│   └── project-4-agent.md         # Benutzerdefinierte Agent-Entwicklung
│
├── 5-Schnellreferenz/                     # Phase 5: Schnellreferenz
│   ├── cheatsheet-commands.md      # Befehls-Cheatsheet
│   ├── cheatsheet-agents.md       # Agent-Cheatsheet
│   └── cheatsheet-workflows.md    # Workflow-Cheatsheet
│
├── assets/                         # Bildressourcen
├── progress-tracker.md             # Lernfortschritt-Tracker
├── contributing.md                 # Beitragsleitfaden
└── README.md                       # Diese Datei
```

---

## Erfolgs-Tipps

1. **Schritt für Schritt**: Nicht zwischen Phasen springen, jede Phase ist die Grundlage für die nächste
2. **Praktisch anwenden**: Jedes Konzept sofort praktisch üben, nicht nur lesen
3. **Übungen abschließen**: Jede Phase hat zugehörige Übungen, nur durch Abschließen werden Kenntnisse gefestigt
4. **Cheatsheets nutzen**: Im täglichen Gebrauch Cheatsheets verwenden, nicht auswendig lernen
5. **Projektgetrieben**: Phase 4 verwendet echte Projekte, um alle Kenntnisse zu verknüpfen
6. **Notizen machen**: Lernerfahrungen und Probleme dokumentieren
7. **Andere lehren**: Nur wenn du es anderen beibringen kannst, hast du es wirklich gemeistert

---

## Häufig gestellte Fragen (FAQ)

### F: Wird Programmiererfahrung benötigt?

A: Grundlegende Command-Line- und Programmierkenntnisse werden benötigt (JavaScript/Node.js-Grundlagen helfen bei einigen Funktionen), aber keine Meisterschaft in einer bestimmten Sprache. Die Einsteiger-Phase ist für Null-Grundlagen konzipiert.

### F: Kann ich Phasen überspringen?

A: Phase 1-3 sollte sequentiell absolviert werden, Phase 4 kann nach Bedarf mit interessanten Projekten gewählt werden, Phase 5 ist jederzeit verfügbar.

### F: Was kann ich nach dem Lernen tun?

A: Produktionsreife Claude Code-Workflows beherrschen, effizient entwickeln, reviewen, testen und automatisieren; benutzerdefinierte Agent-Systeme entwerfen und implementieren; Hooks und Skills anpassen.

### F: Was tun bei Problemen während des Lernens?

A: 1) Relevante Kapitel in der [Referenzdokumentation](./Referenzdokumente/) nachschlagen; 2) [Befehls-Cheatsheet](./5-Schnellreferenz/cheatsheet-commands.md) prüfen; 3) Issue erstellen um Hilfe bitten.

### F: Wie kann ich den Lerneffekt überprüfen?

A: [progress-tracker.md](./progress-tracker.md) verwenden um Fortschritt zu verfolgen, jede Phase hat klare Lernziele, alle Übungen abschließen bedeutet die Phase gemeistert zu haben.

### F: Wird der Kursinhalt aktualisiert?

A: Ja, wird kontinuierlich aktualisiert, PRs sind willkommen. Details unter [contributing.md](./contributing.md).

---

## Beiträge und Feedback

Wenn du Probleme findest oder Verbesserungsvorschläge hast, erstelle gerne ein Issue oder Pull Request.

---

*Zuletzt aktualisiert: 2026-05-26*