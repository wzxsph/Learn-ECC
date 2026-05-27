# Hookify-System-Befehle

## Ueberblick

Das Hookify-System wird verwendet um Hooks zu erstellen und zu verwalten, um unerwuenschtes Verhalten zu verhindern und Workflows zu automatisieren.

## Befehlsliste

### /hookify

**Zweck**: Hooks erstellen um unerwuenschtes Verhalten zu verhindern

**Beschreibung**: Erstellt benutzerdefinierte Hooks um unerwuenschtes Verhalten zu verhindern.

**Anwendungsszenarien**:
- Versehentliche Loeschoperationen verhindern
- Gefaehrliche Befehle blockieren
- Automatische Formatierung
- Qualitaetspruefungen

**Hook-Typen**:
- **PreToolUse**: Vor Werkzeugausfuehrung
- **PostToolUse**: Nach Werkzeugausfuehrung
- **Stop**: Bei Sitzungsende

---

### /hookify-list

**Zweck**: Alle konfigurierten Hookify-Regeln auflisten

**Beschreibung**: Zeigt alle aktuell konfigurierten Hookify-Regeln.

---

### /hookify-configure

**Zweck**: Interaktiv Hookify-Regeln aktivieren/deaktivieren

**Beschreibung**: Interaktive Konfiguration der Aktivierungs- und Deaktivierungsstatus von Hookify-Regeln.

---

### /hookify-help

**Zweck**: Hookify-System-Hilfe

**Beschreibung**: Detaillierte Hilfeinformationen ueber das Hookify-System abrufen.

---

## Hook-Konfigurationsbeispiel

```json
{
  "matcher": { "pattern": "Bash|Edit|Write" },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Running quality check'"
    }
  ]
}
```

---

## Zugehoerige Befehle

- `/hookify` - Hook erstellen
- `/hookify-list` - Regeln auflisten
- `/hookify-configure` - Regeln konfigurieren
- `/hookify-help` - Hilfe abrufen