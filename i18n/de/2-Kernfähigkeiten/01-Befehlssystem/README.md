# 01 - Befehlssystem meistern

> Die 75+ Befehle von Claude Code beherrschen, effiziente Entwicklung durch flexible Kombination

## Modulziele

Nach Abschluss dieses Moduls wirst du in der Lage sein:

- Alle Befehle unter jeder Kategorie zu identifizieren und verwenden
- Nach Szenario den am besten geeigneten Befehl auszuwählen
- Mehrere Befehle kombinieren um benutzerdefinierte Workflows zu erstellen
- Befehlsdokumentation verwenden um schnell Befehlsverwendung nachzuschlagen

## Befehlsübersicht

Claude Code bietet 75+ Befehle, die folgende Kategorien abdecken:

| Kategorie | Befehlszahl | Beispielbefehle |
|------|--------|----------|
| Entwicklungsworkflow | 15+ | `/plan`, `/tdd`, `/build-fix`, `/verify` |
| Code-Review | 10+ | `/code-review`, `/security-review`, `/python-review` |
| Build und Test | 15+ | `/go-build`, `/rust-build`, `/flutter-build` |
| Projektmanagement | 10+ | `/project-init`, `/projects`, `/santa-loop` |
| Skill-Management | 10+ | `/skill-create`, `/skill-stocktake`, `/skill-scout` |
| Lernen und Forschung | 10+ | `/learn`, `/learn-eval`, `/deep-research` |
| Hilfsprogramme | 15+ | `/config`, `/jira`, `/pr`, `/e2e` |

## Befehlsdokumentationsstruktur

Jeder Befehl hat eine Standard-Dokumentationsstruktur:

```
Befehlsname
├── Befehlsbeschreibung
├── Nutzungsszenarien
├── Parameterbeschreibung
├── Nutzungsbeispiele
└── Warnhinweise
```

## Häufig verwendete Befehle-Übersicht

### Entwicklungsworkflow
- `/plan` - Implementierungsplan erstellen
- `/tdd` - Testgetriebene Entwicklung
- `/build-fix` - Build-Fehler beheben
- `/verify` - Code-Änderungen verifizieren
- `/review` - Code-Review

### Build-bezogen
- `/go-build`, `/rust-build`, `/kotlin-build` - Verschiedene Sprach-Builds
- `/cpp-build`, `/flutter-build` - Kompilierte Sprach-Builds
- `/docker-build` - Docker-Image-Build

### Projektmanagement
- `/project-init` - Neues Projekt initialisieren
- `/projects` - Alle Projekte auflisten
- `/santa-loop` - Automatisierte Aufgaben-Schleife

### Skill-Entwicklung
- `/skill-create` - Skill aus Git-Verlauf erstellen
- `/skill-scout` - Verfügbare Skills suchen
- `/learn` - Muster aus Sitzung extrahieren

## Wie man Befehlsdokumentation verwendet

Die vollständige Befehlsdokumentation befindet sich in: `../../Referenzdokumente/commands/01-核心工作流.md`

Dokumentation enthält:
- Detaillierte Erklärung jedes Befehls
- Parameter und Optionen
- Nutzungsbeispiele und Ausgaben
- Best Practice Empfehlungen

## Befehle kombiniert verwenden

Befehle können kombiniert werden um komplexe Workflows zu erstellen:

```
/plan → /tdd → /build-fix → /code-review → /verify
```

Die Ausgabe jedes Befehls kann an den nächsten Befehl übergeben werden, forming an automated pipeline.

## Nächste Schritte

- [Übungsaufgaben lernen](./exercises/练习.md)
- [Vollständige Befehlsdokumentation lesen](../../Referenzdokumente/commands/01-核心工作流.md)