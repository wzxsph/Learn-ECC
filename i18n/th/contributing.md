# Learn-ECC Beitritts-Leitfaden

Willkommen, Learn-ECC-Inhalte beizutragen! Dieser Leitfaden erklärt, wie du neue Inhalte hinzufügen, bestehende verbessern oder Änderungen einreichen kannst.

## Beitragstypen

### 1. Neue Inhalte hinzufügen

Beitragbare Inhaltstypen:
- Neues Kapitel oder Modul
- Neue Übungen
- Neue praktische Projekte
- Cheatsheet-Ergänzungen
- Übersetzungsverbesserungen

### 2. Bestehende Inhalte verbessern

- Fehler korrigieren
- Erklärungen klarer machen
- Mehr Beispiele hinzufügen
- Übungsantworten vervollständigen
- Veraltete Informationen aktualisieren

### 3. Probleme melden

- Tippfehler oder Grammatikfehler
- Code-Beispiele funktionieren nicht
- Links funktionieren nicht
- Inhalt fehlt

## Inhaltsformat-Standards

### Markdown-Format

Alle Inhalte müssen Markdown-Format verwenden, mit folgenden Abschnitten:

```markdown
# Titel

## Lernziele

Nach diesem Abschnitt wirst du können:
- Ziel1
- Ziel2

---

## Hauptinhalt

### Unterüberschrift

Inhalt...

---

## Übungen

### Übung 1

Aufgabenbeschreibung...

### Verifikationsmethode

1. Verifikationsschritt 1
2. Verifikationsschritt 2

---

## Nächste Schritte

- Nächstes Kapitel: [Link](./next.md)
- Zurück: [Inhaltsverzeichnis](../README.md)
```

### Dateibenennung

- Chinesische Benennung verwenden
- Wörter mit Bindestrich `-` trennen
- Kapiteldateien: `01-Kapitelname.md`, `02-Kapitelname.md`
- Übungsdateien: `Übung-Name.md`

### Link-Standards

- Referenzdokumentation-Links: `./เอกสารอ้างอิง/Dateiname.md`
- Kursinterne Links: `./Kapitelname.md`
- Relative Pfade: Vom aktuellen Dateistandort berechnet

### Code-Beispiele

```javascript
// Code-Sprachmarkierung
function example() {
  return "Hello ECC"
}
```

```bash
# Kommandozeilen-Beispiel
node tests/run-all.js
```

### Tabellenformat

| Überschrift1 | Überschrift2 | Überschrift3 |
|-------|-------|-------|
| Inhalt1 | Inhalt2 | Inhalt3 |

## Änderungen einreichen

### Schritte

1. Repository forken
2. Branch erstellen: `git checkout -b feature/NeueInhaltsbeschreibung`
3. Änderungen durchführen
4. Commit: `git commit -m "feat: Neuen Inhalt hinzufügen"`
5. zum Remote pushen: `git push origin feature/NeueInhaltsbeschreibung`
6. Pull Request erstellen

### Commit-Nachrichten-Standard

```
<type>: <description>

<optional body>
```

Type-Typen:
- `feat`: Neue Funktion
- `fix`: Fehlerbehebung
- `docs`: Dokumentationsaktualisierung
- `test`: Testaktualisierung
- `refactor`: Refactoring

## Qualitätsprüfung

Vor dem Commit bitte prüfen:

- [ ] Markdown-Format korrekt
- [ ] Alle Links funktionieren
- [ ] Code-Beispiele ausführbar
- [ ] Übungen haben Verifikationsmethoden
- [ ] Chinesisch ohne Tippfehler
- [ ] Entspricht den Formatstandards

## Probleme melden

Wenn du Probleme oder Vorschläge hast, erstelle ein Issue mit:
- Problembeschreibung
- Dateistandort
- Vorgeschlagene Verbesserung

---

Danke für deinen Beitrag!