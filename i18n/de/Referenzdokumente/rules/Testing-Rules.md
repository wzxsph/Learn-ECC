# Testregeln

## Regeluebersicht

ECC Rules Testregeln definieren verbindliche Teststandards die Codequalitaet sicherstellen. Diese Regeln decken testgetriebene Entwicklung (TDD), Mindestabdeckungsanforderungen und Teststrukturnormen ab und stellen sicher dass aller Code umfassend verifiziert wird.

## Kernanforderungen

### Mindesttestabdeckung: 80%

Die folgenden drei Testtypen sind erforderlich:

| Testtyp | Beschreibung | Abdeckungsbereich |
|----------|------|----------|
| Unit-Tests | Eigenstaendige Funktionen, Utilities, Komponenten | Individual units |
| Integrationstests | API-Endpoints, Datenbankoperationen | Integration points |
| End-to-End-Tests | Kritische Benutzerflows | Critical user flows |

### Testgetriebene Entwicklung (TDD)

**Obligatorischer Workflow**:

```
1. Test schreiben (ROT)   - Erst einen fehlschlagenden Test schreiben
2. Test ausfuehren          - Verifizieren dass Test fehlschlaegt
3. Minimale Implementierung schreiben (GRUEN) - Code schreiben der Test besteht
4. Test ausfuehren          - Verifizieren dass Test besteht
5. Refaktorieren (VERBESSERN)    - Codestruktur verbessern
6. Abdeckung verifizieren        - Sicherstellen dass 80%+ erreicht
```

## Implementierungsdetails

### AAA-Muster (Arrange-Act-Assert)

Alle Tests muessen dem AAA-Muster folgen:

```typescript
test('berechnet Kosinusaehnlichkeit korrekt', () => {
  // Arrange - Testdaten vorbereiten
  const vector1 = [1, 0, 0];
  const vector2 = [0, 1, 0];

  // Act - Zu testende Operation ausfuehren
  const similarity = calculateCosineSimilarity(vector1, vector2);

  // Assert - Ergebnis verifizieren
  expect(similarity).toBe(0);
});
```

### Testbenennungskonventionen

Beschreibende Namen verwenden die das getestete Verhalten erklaeren:

```typescript
// Empfohlen - Beschreibt erwartetes Verhalten
test('gibt leeres Array zurueck wenn keine Maerkte Abfrage entsprechen', () => {});
test('wirft Fehler wenn API-Schluessel fehlt', () => {});
test('fallt auf Substring-Suche zurueck wenn Redis nicht verfuegbar ist', () => {});

// Vermeiden - Mehrdeutige Namen
test('test1', () => {});
test('edge case', () => {});
```

### Testfehlerbehebung

Wenn Tests fehlschlagen:

| Schritt | Aktion |
|------|------|
| 1 | **tdd-guide** Agent verwenden |
| 2 | Testisolation pruefen |
| 3 | Mock-Korrektheit verifizieren |
| 4 | Implementierung reparieren, nicht Tests (es sei denn Tests selbst sind falsch) |

### Agent-Unterstuetzung

| Agent | Zweck | Wann verwenden |
|-------|------|----------|
| **tdd-guide** | Testgetriebene Entwicklungsanleitung | Bei neuer Feature-Entwicklung, Fehlerbehebung obligatorisch verwenden |
| **code-reviewer** | Code-Review | Nach dem Schreiben von Code |

## Verstossshandhabung

### Unzureichende Abdeckung

- Code mit Abdeckung unter 80% darf nicht gemergt werden
- CI/CD-Pipeline sollte niedrige Abdeckung blockieren
- `c8` oder aehnliche Tools verwenden um Abdeckungsberichte zu generieren

### Testfehlerbehandlung

```
1. Fehlerursache analysieren
2. Diagnostizieren ob es ein Implementierungs- oder Testproblem ist
3. Wenn Implementierungsproblem - Implementierungscode reparieren
4. Wenn Testproblem - Testcode reparieren
5. Sicherstellen dass alle Tests bestehen bevor committet wird
```

### Haeufige Probleme

| Problem | Ursache | Loesung |
|------|------|----------|
| Flaky Tests | Asynchrone Operationen, Race Conditions | Wartezeiten erhoehen, Mocks verwenden |
| Testisolationsfehler | Shared State-Kontamination | Zustand vor jedem Test zuruecksetzen |
| Inkorrektes Mock | Erwartungen stimmen nicht mit Realitaet ueberein | Mock-Setup verifizieren |

## Zugehoerige Regeln

| Zugehoerige Regel | Beschreibung |
|------------------|--------------|
| Codestil-Regeln | Codelesbarkeit und -wartbarkeit |
| Sicherheitsregeln | Sicherheitstest-Anforderungen |
| Code-Review-Regeln | Testabdeckungspruefungen |
| Entwicklungsworkflow | TDD-Prozessintegration |