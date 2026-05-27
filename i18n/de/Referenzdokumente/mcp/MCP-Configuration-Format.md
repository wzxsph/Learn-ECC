# MCP-Konfigurationsformat

## Ueberblick

MCP (Model Context Protocol) Server werden ueber den `mcpServers`-Abschnitt in der `~/.claude.json`-Datei konfiguriert. Claude Code unterstuetzt zwei Typen von MCP-Servern:

- **Process-Modus (process)**: Server die ueber lokale Befehle gestartet werden
- **HTTP-Modus (http)**: Remote-Server die ueber HTTP/HTTPS zugreifbar sind

## JSON-Konfigurationsstruktur

```json
{
  "mcpServers": {
    "Server-Name": {
      "type": "http",           // Optional, standard is process
      "command": "npx",        // Bei process-Modus erforderlich
      "url": "https://...",    // Bei http-Modus erforderlich
      "args": ["argument1", "argument2"],  // Optional
      "env": {                 // Optional, Umgebungsvariablen
        "KEY": "value"
      },
      "headers": {             // Optional, HTTP-Request-Headers
        "Header-Name": "value"
      },
      "description": "Server-Beschreibung"  // Optional
    }
  }
}
```

## Feldbeschreibungen

| Feld | Typ | Erforderlich | Beschreibung |
|------|------|------|------|
| `type` | string | Nein | Verbindungstyp: `process` (Standard) oder `http` |
| `command` | string | bei type=process | Startbefehl (z.B. `npx`, `uvx`, `python3`) |
| `url` | string | bei type=http | HTTP/HTTPS-URL des MCP-Servers |
| `args` | array | Nein | Array von Argumenten die an den Befehl übergeben werden |
| `env` | object | Nein | Umgebungsvariablen-Schluessel-Wert-Paare |
| `headers` | object | Nein | HTTP-Request-Headers (nur http-Modus) |
| `description` | string | Nein | Beschreibung der Serverfunktionalitaet |

## Konfigurationsbeispiele

### Process-Modus-Beispiel

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "GitHub-Operationen - PRs, Issues, Repos"
    }
  }
}
```

### HTTP-Modus-Beispiel

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Vercel Deployment und Projekte"
    }
  }
}
```

### HTTP-Modus mit Authentifizierungs-Headers

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "YOUR_KEY_HERE"
      },
      "description": "AI Browser-Agent"
    }
  }
}
```

## Best Practices

1. **Umgebungsvariablen-Management**: Sensible Informationen (API-Keys, Tokens) MÜSSEN Umgebungsvariablen verwenden, nicht hartcodieren
2. **Kontextfenster**: Es wird empfohlen nicht mehr als 10 MCP-Server aktiviert zu halten um Kontextfenster zu erhalten
3. **Server deaktivieren**: Mit der Umgebungsvariablen `ECC_DISABLED_MCPS=server1,server2` können verpackte MCP-Server deaktiviert werden

## Fehlerbehebung

- Sicherstellen dass Befehle ausführbar sind (npx, uvx usw. sind installiert)
- Prüfen ob Umgebungsvariablen korrekt gesetzt sind
- Serververbindungsfehler in Claude Code-Logs überprüfen