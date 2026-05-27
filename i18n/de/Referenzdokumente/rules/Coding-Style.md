# Codestil-Regeln

## Regeluebersicht

Die ECC Rules Codestil-Regeln definieren Kernprinzipien und Best Practices fuer Codeorganisation. Diese Regeln stellen sicher dass Code lesbar, wartbar und konsistent bleibt, von Immutabilitaet bis zu Namenskonventionen.

## Kernanforderungen

### Immutabilitaet (Schluesselprinzip)

**MUST**: Immer neue Objekte erstellen, niemals existierende veraendern.

```typescript
// FALSCH - Original veraendern
function modify(user, name) {
  user.name = name;  // veraendert das Original
  return user;
}

// RICHTIG - Neues Objekt zurueckgeben
function update(user, name) {
  return { ...user, name };  // Erstellt neue Kopie
}
```

**Begruendung**: Immutable Daten verhindern versteckte Nebenwirkungen, machen Debugging einfacher und unterstuetzen sichere Concurrency.

### Kern-Designprinzipien

| Prinzip | Beschreibung | Praxis |
|---------|--------------|--------|
| **KISS** | Keep It Simple | Einfachste funktionierende Loesung bevorzugen, fruehe Optimierung vermeiden, Klarheit vor Cleverness |
| **DRY** | Don't Repeat Yourself | Wiederholte Logik in geteilte Funktionen oder Utilities extrahieren, Copy-Paste-Drift vermeiden |
| **YAGNI** | You Aren't Gonna Need It | Keine Features oder Abstraktionen bauen die vielleicht spaeter gebraucht werden, einfach starten und bei echtem Bedarf refaktorieren |
### Dateiorganisation

| Anforderung | Beschreibung |
|-------------|--------------|
| Viele kleine Dateien > wenige grosse Dateien | Hohe Kohaesion, niedrige Kopplung |
| Typische Zeilenanzahl | 200-400 Zeilen, maximal 800 |
| Grosse Module aufteilen | Utilities aus grossen Modulen extrahieren |
| Organisiert nach | Funktions-/Domänenbereich, nicht nach Typ |

### Fehlerbehandlung

| Anforderung | Beschreibung |
|-------------|--------------|
| Explizit behandeln | Auf jeder Ebene Fehler explizit behandeln |
| UI-Code | Benutzerfreundliche Fehlermeldungen bereitstellen |
| Server-seitig | Detaillierten Fehlerkontext loggen |
| Verboten | Fehler nicht stillschweigend schlucken |

### Eingabevalidierung

| Anforderung | Beschreibung |
|-------------|--------------|
| Grenzen validieren | Alle Eingaben an Systemgrenzen validieren |
| Schema-Validierung | Wo moeglich Schema-basierte Validierung verwenden |
| Schnelles Scheitern | Mit klaren Fehlermeldungen schnell scheitern |
| Externe Daten | Niemals externen Daten vertrauen (API-Antworten, Benutzereingaben, Dateiinhalt) |
## Implementierungsdetails

### Namenskonventionen

| Typ | Konvention | Beispiel |
|-----|-------------|----------|
| Variablen und Funktionen | camelCase + beschreibende Namen | `calculateTotal`, `userName` |
| Booleans | is/has/should/can Praefix | `isActive`, `hasPermission` |
| Interfaces, Typen, Komponenten | PascalCase | `UserProfile`, `ButtonComponent` |
| Konstanten | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT`, `API_TIMEOUT` |
| Custom Hooks | camelCase + use Praefix | `useUserData`, `useFetch` |

### Code-Smells

#### Tiefe Verschachtelung

Mehr als 4 Ebenen Verschachtelung mit fruehen Returns refaktorieren:

```typescript
// Vermeiden
function processOrder(order) {
  if (order) {
    if (order.items) {
      if (order.items.length > 0) {
        // Verarbeitungslogik
      }
    }
  }
}

// Empfohlen
function processOrder(order) {
  if (!order || !order.items || order.items.length === 0) {
    return;
  }
  // Verarbeitungslogik
}
```

#### Magische Zahlen

Bennant-Konstanten statt magischer Zahlen verwenden:

```typescript
// Vermeiden
setTimeout(callback, 300000);

// Empfohlen
const SESSION_TIMEOUT_MS = 5 * 60 * 1000; // 5 Minuten
setTimeout(callback, SESSION_TIMEOUT_MS);
```

#### Lange Funktionen

Grosse Funktionen in kleine mit klaren Verantwortlichkeiten aufteilen:

| Funktionslaenge | Empfehlung |
|-----------------|------------|
| <50 Zeilen | Ideal |
| 50-100 Zeilen | Akzeptabel, Aufteilung in Betracht ziehen |
| >100 Zeilen | MUSS aufgeteilt werden |

## Verstossshandhabung

### Codequalitaets-Pruefliste

Vor dem Markieren der Arbeit als abgeschlossen pruefen:

- [ ] Code ist lesbar und gut benannt
- [ ] Funktionen sind klein (<50 Zeilen)
- [ ] Dateien sind fokussiert (<800 Zeilen)
- [ ] Keine tiefe Verschachtelung (>4 Ebenen)
- [ ] Angemessene Fehlerbehandlung
- [ ] Keine hartcodierten Werte (Konstanten oder Konfiguration verwenden)
- [ ] Immutable Muster verwendet (keine Mutation)

### Schweregrad-Stufen

| Stufe | Bedeutung | Aktion |
|-------|-----------|--------|
| CRITICAL | Sicherheitsluecke oder Datenverlustrisiko | **BLOCK** - Muss vor dem Merge behoben werden |
| HIGH | Bug oder signifikantes Qualitaetsproblem | **WARN** - Sollte vor dem Merge behoben werden |
| MEDIUM | Wartbarkeitsproblem | **INFO** - Empfohlen zu beheben |
| LOW | Stil oder kleine Anmerkung | **NOTE** - Optional |

## Zugehoerige Regeln

| Zugehoerige Regel | Beschreibung |
|------------------|--------------|
| Testregeln | Testabdeckungsanforderungen |
| Sicherheitsregeln | Sicherheitspruefpunkte |
| Code-Review-Regeln | Detaillierte Review-Standards |
| Entwicklungsworkflow | TDD- und Code-Review-Prozess |