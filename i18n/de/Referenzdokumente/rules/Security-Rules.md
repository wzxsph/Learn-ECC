# Sicherheitsregeln

## Regeluebersicht

ECC Rules Sicherheitsregeln sind obligatorische Sicherheitspruefungen die sicherstellen dass Code vor dem Commit den hoechsten Sicherheitsstandards entspricht. Diese Regeln decken alles von Secrets-Management bis MCP-Server-Sicherheit ab und sind der Mindeststandard den alle Codeaenderungen einhalten müssen.

## Kernanforderungen

### Obligatorische Pruefliste vor dem Commit

Vor dem Committen von Code müssen folgende Sicherheitspruefungen abgeschlossen werden:

| Pruefpunkt | Beschreibung |
|------------|--------------|
| Keine hartcodierten Secrets | API-Schluessel, Passwoerter, Tokens usw. duerfen nicht in Quellcode hartcodiert sein |
| Benutzereingabe-Validierung | Alle Benutzereingaben müssen validiert werden |
| SQL-Injection-Schutz | Parameterisierte Abfragen verwenden um SQL-Injection zu verhindern |
| XSS-Schutz | Benutzereingaben HTML-escapen |
| CSRF-Schutz | Sicherstellen dass CSRF-Schutz aktiviert ist |
| Authentifizierung/Autorisierung verifizieren | Authentifizierungs- und Autorisierungslogik korrekt bestaetigen |
| Rate-Limiting | Alle Endpoints müssen Rate-Limiting haben |
| Sicherere Fehlermeldungen | Fehlermeldungen duerfen keine sensiblen Daten exponieren |