# Performance-Optimierung

## Modell-Auswahlstrategie

**Haiku 4.5** (90% von Sonnet-Faehigkeit, 1/3 der Kosten):
- Leichtgewichtige Agents mit haeufigem Aufruf
- Pair-Programming und Codegenerierung
- Worker Agents in Multi-Agent-Systemen

**Sonnet 4.6** (Bestes Coding-Modell):
- Hauptentwicklungsarbeit
- Multi-Agent-Workflows orchestrieren
- Komplexe Coding-Aufgaben

**Opus 4.5** (Tiefste Reasoning):
- Komplexe Architekturentscheidungen
- Maximale Reasoning-Anforderungen
- Recherche- und Analyseaufgaben

## Kontextfenster-Management

Die letzten 20% des Kontextfensters vermeiden fuer:
- Grossflaechige Refactorings
- Funktionsimplementierung ueber mehrere Dateien
- Komplexe Interaktionen debuggen

Niedrig kontextsensitive Aufgaben:
- Einzelfile-Edits
- Unabhaengige Utility-Erstellung
- Dokumentationsaktualisierungen
- Einfache Bug-Fixes

## Erweitertes Denken + Plan-Modus

Erweitertes Denken ist standardmaessig aktiviert und reserviert bis zu 31.999 Tokens fuer internes Reasoning.

Erweitertes Denken steuern:
- **Umschalten**: Option+T (macOS) / Alt+T (Windows/Linux)
- **Konfiguration**: `alwaysThinkingEnabled` in `~/.claude/settings.json` setzen
- **Budget-Limit**: `export MAX_THINKING_TOKENS=10000`
- **Verbose-Modus**: Ctrl+O um Thinking-Output anzuzeigen

Fuer komplexe Aufgaben die tiefes Reasoning erfordern:
1. Sicherstellen dass erweitertes Denken aktiviert ist (standard)
2. **Plan-Modus** aktivieren fuer strukturierte Herangehensweise
3. Mehrere Kritikrunden fuer gruendliche Analyse verwenden
4. Split-Role Sub-Agents fuer verschiedene Perspektiven verwenden

## Build-Fehlerbehebung

Wenn Build fehlschlaegt:
1. **build-error-resolver** Agent verwenden
2. Fehlermeldungen analysieren
3. Inkrementell beheben
4. Nach jeder Reparatur verifizieren