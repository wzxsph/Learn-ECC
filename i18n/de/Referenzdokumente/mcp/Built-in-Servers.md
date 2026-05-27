# Eingebaute MCP-Server

Das ECC-Projekt stellt mehrere haeufig verwendete MCP-Serverkonfigurationen in `mcp-configs/mcp-servers.json` vor.

## Nach Funktionalitaet kategorisiert

### Entwicklungstools

| Server | Typ | Beschreibung |
|--------|------|------|
| `github` | process | GitHub-Operationen - PRs, Issues, Repos |
| `jira` | process | Jira-Problemverfolgung - Suchen, Erstellen, Aktualisieren, Kommentieren, Workflow von Problemen |
| `confluence` | process | Confluence Cloud-Integration - Seiten suchen, Inhalt abrufen, Raeume erkunden |
| `filesystem` | process | Dateisystem-Operationen (Pfad muss konfiguriert werden) |
| `memory` | process | Cross-Session persistent Memory |
| `omega-memory` | process | Verbessertes persistentes Agent-Memory mit semantischer Suche, Multi-Agent-Kollaboration und Knowledge Graph |
| `sequential-thinking` | process | Verkettete Denk-Reasoning |

### Deployment und Infrastruktur

| Server | Typ | Beschreibung |
|--------|------|------|
| `vercel` | http | Vercel Deployment und Projekte |
| `railway` | process | Railway Deployment |
| `cloudflare-docs` | http | Cloudflare-Dokumentationssuche |
| `cloudflare-workers-builds` | http | Cloudflare Workers Builds |
| `cloudflare-workers-bindings` | http | Cloudflare Workers Bindings |
| `cloudflare-observability` | http | Cloudflare Observability/Logging |

### Datenbanken

| Server | Typ | Beschreibung |
|--------|------|------|
| `supabase` | process | Supabase Datenbankoperationen |
| `clickhouse` | http | ClickHouse analytische Abfragen |

### KI und maschinelles Lernen

| Server | Typ | Beschreibung |
|--------|------|------|
| `fal-ai` | process | fal.ai Modell AI Bild/Video/Audio-Generierung |
| `exa-web-search` | process | Websuche und Recherche ueber Exa API |
| `context7` | process | Echtzeit-Dokumentationsabfrage - funktioniert mit /docs Befehl und documentation-lookup skill |
| `magic` | process | Magic UI Komponenten |
| `evalview` | process | AI Agent Regressions-Tests - Verhalten schnappen, Regressions bei Tool-Aufrufen und Ausgabequalitaet erkennen |

### Browser-Automatisierung

| Server | Typ | Beschreibung |
|--------|------|------|
| `playwright` | process | Browser-Automatisierung und Testing via Playwright |
| `browserbase` | process | Browserbase Cloud-Browser-Sessions |
| `browser-use` | http | AI Browser-Agent, fuehrt Web-Aufgaben aus |

### Sonstige Tools

| Server | Typ | Beschreibung |
|--------|------|------|
| `firecrawl` | process | Web-Scraping und Crawling |
| `longhand` | process | verlustfreier Claude Code Sitzungsverlauf - Indexiert RAW Tool-Aufrufe in lokale SQLite + ChromaDB |
| `token-optimizer` | process | Token-Optimierung - 95%+ Kontextreduktion durch Content-Deduplizierung und -Komprimierung |
| `devfleet` | http | Multi-Agent-Orchestrierung - Schedules parallele Claude Code Agents in isolierten Worktrees |
| `laraplugins` | http | Laravel Plugin-Entdeckung - Sucht Pakete nach Keywords, Health-Scores, Laravel/PHP-Versionskompatibilitaet |

## Schnellstart

Relevante Serverkonfiguration in `~/.claude.json` `mcpServers`-Abschnitt kopieren und `YOUR_*_HERE`-Platzhalter durch tatsaechliche Werte ersetzen.

**Beispiel - GitHub-Server aktivieren:**

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_actual_token"
      },
      "description": "GitHub operations - PRs, issues, repos"
    }
  }
}
```

## Umgebungsvariablen-Anforderungen

| Server | Erforderliche Umgebungsvariablen |
|--------|----------------|
| `github` | `GITHUB_PERSONAL_ACCESS_TOKEN` |
| `jira` | `JIRA_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN` |
| `firecrawl` | `FIRECRAWL_API_KEY` |
| `supabase` | `--project-ref` Parameter |
| `exa-web-search` | `EXA_API_KEY` |
| `fal-ai` | `FAL_KEY` |
| `browserbase` | `BROWSERBASE_API_KEY` |
| `browser-use` | `x-browser-use-api-key` Request-Header |
| `confluence` | `CONFLUENCE_BASE_URL`, `CONFLUENCE_EMAIL`, `CONFLUENCE_API_TOKEN` |
| `evalview` | `OPENAI_API_KEY` (optional) |