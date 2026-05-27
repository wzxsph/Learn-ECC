# Sonstige Befehle

## Ueberblick

Sonstige diverse Befehle, einschliesslich Jira-Integration, GAN-Operationen, Sicherheitsscans und weitere Funktionen.

## Befehlsliste

### /jira

**Zweck**: Mit Jira-Tickets interagieren - Abrufen, Analysieren, Kommentieren, Statusaenderungen, Suchen

**Beschreibung**: Integration mit Jira-Projektmanagement-Tool, unterstuetzt direkte Workflow-Nutzung von Jira-Tickets.

**Verwendung**:

| Befehl | Beschreibung |
|---|---|
| `/jira get <TICKET-KEY>` | Jira-Ticket abrufen und analysieren |
| `/jira comment <TICKET-KEY>` | Fortschrittskommentar hinzufuegen |
| `/jira transition <TICKET-KEY>` | Ticketstatus aendern |
| `/jira search <JQL>` | Probleme mit JQL suchen |

**`/jira get` Workflow**:
1. Ticket von Jira abrufen (ueber MCP oder REST API)
2. Alle Felder extrahieren: Zusammenfassung, Beschreibung, Akzeptanzkriterien, Prioritaet, Tags, verknuepfte Probleme
3. Optional Kommentare abrufen fuer zusaetzlichen Kontext
4. Strukturierten Analysebericht generieren

**Voraussetzungen** - Jira-Credentials konfigurieren (eins von zwei):

**Option A — MCP Server (empfohlen)**:
`jira` zu `mcpServers`-Konfiguration hinzufuegen.

**Option B — Umgebungsvariablen**:
```bash
export JIRA_URL="https://yourorg.atlassian.net"
export JIRA_EMAIL="your.email@example.com"
export JIRA_API_TOKEN="your-api-token"
```

**Integration mit anderen Befehlen**:
- Nach Ticket-Analyse `/plan` zum Erstellen eines Implementierungsplans verwenden
- `tdd-workflow` skill fuer testgetriebene Entwicklung verwenden
- `/code-review` zum Reviewen der Implementierung verwenden
- `/jira comment` oder `/jira transition` zum Aktualisieren des Ticketstatus verwenden

---

### /gan-build (in Bearbeitung)

**Zweck**: GAN-Build-Operationen

**Beschreibung**: GAN (Generative Agent Network) Build-Operationen.

---

### /gan-design (in Bearbeitung)

**Zweck**: GAN-Design-Operationen

**Beschreibung**: GAN-Design-bezogene Operationen.

---

### /prune

**Zweck**: Veraltete pending Instincts loeschen die ueber 30 Tage nicht erhoeht wurden

**Beschreibung**: Veraltete Instincts bereinigen die ueber 30 Tage alt sind, automatisch generiert aber nie ueberprueft oder erhoeft wurden.

**Verwendung**:
```
/prune                    # Instincts loeschen die ueber 30 Tage alt sind
/prune --max-age 60      # Benutzerdefinierte Tagesschwelle
/prune --dry-run         # Vorschau ohne zu loeschen
```

**Implementierung**:
```bash
python3 "${CLAUDE_PLUGIN_ROOT}/skills/continuous-learning-v2/scripts/instinct-cli.py" prune
```

Oder bei manueller Installation (wenn `CLAUDE_PLUGIN_ROOT` nicht gesetzt):
```bash
python3 ~/.claude/skills/continuous-learning-v2/scripts/instinct-cli.py prune
```

**Parameter**:

| Parameter | Beschreibung |
|---|---|
| Keine | Instincts loeschen die ueber 30 Tage alt sind |
| `--max-age <days>` | Benutzerdefinierte Zeitschwelle (Tage) |
| `--dry-run` | Vorschau was geloescht wird, ohne tatsaechlich zu loeschen |

**Best Practices**:
- Zuerst `--dry-run` verwenden um anzuzeigen was geloescht wird
- Regelmaessig prune ausfuehren um Instinct-Inventar sauber zu halten
- Beachtung: Es werden nur Instincts im Status pending geloescht, keine erhoeften

---

### /security-scan

**Zweck**: Sicherheitsscan

**Beschreibung**: Sicherheitsluecken-Scan der Codebasis.

**Scan-Inhalt**:
- Hartcodierte Credentials
- SQL-Injection-Risiken
- XSS-Schwachstellen
- Abhaengigkeitsschwachstellen

---

### /feature-dev (in Bearbeitung)

**Zweck**: Feature-Entwicklungs-Assistent

**Beschreibung**: Bietet Hilfs-Workflow fuer Feature-Entwicklung.

---

### /cost-report

**Zweck**: Modellkostenbericht

**Beschreibung**: Generiert Kostenbericht fuer AI-Modellnutzung.

---

## Zugehoerige Befehle

- `/security-scan` - Sicherheitsscan
- `/jira` - Jira-Integration
- `/prune` - Instinct-Bereinigung