# Hooks-System

## Hook-Typen

- **PreToolUse**: Vor Werkzeugausfuehrung (Validierung, Parameteraenderung)
- **PostToolUse**: Nach Werkzeugausfuehrung (Auto-Formatierung, Pruefungen)
- **Stop**: Bei Sitzungsende (Endgueltige Verifizierung)

## Auto-Akzept-Permissions

Mit Vorsicht verwenden:
- Fuer vertrauenswuerdige, klar definierte Plaene aktivieren
- Fuer explorative Arbeit deaktivieren
- Die `dangerously-skip-permissions`-Flag niemals verwenden
- `allowedTools` in `~/.claude.json` konfigurieren

## TodoWrite Best Practices

TodoWrite-Werkzeug verwenden um:
- Fortschritt bei mehrstufigen Aufgaben zu verfolgen
- Verstaendnis der Anweisungen zu verifizieren
- Echtzeit-Steuerung zu ermoeglichen
- Granulare Implementierungsschritte anzuzeigen

Todo-Liste zeigt auf:
- Schritte in falscher Reihenfolge
- Fehlende Elemente
- Ueberzaehlige unnoetige Elemente
- Falsche Granularitaet
- Missverstandene Anforderungen