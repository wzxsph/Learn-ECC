# Einsteiger-Übungen

Folgende Übungen abschließen, um ECC-Grundlagen zu festigen.

## Übung 1: Umgebungsverifizierung

**Ziel**: Bestätigen, dass ECC-Umgebung korrekt installiert ist

**Schritte**:
1. Testsuite ausführen: `node tests/run-all.js`
2. Alle Tests bestehen verifizieren
3. `.claude/` Verzeichnisstruktur auflisten

**Abnahmekriterien**:
- Alle Tests bestanden
- agents, skills, hooks, rules Verzeichnisse sichtbar

---

## Übung 2: Schrägstrich-Befehle verwenden

**Ziel**: Sich mit häufigen Schrägstrich-Befehlen vertraut machen

**Schritte**:
1. Claude Code starten
2. Folgende Befehle nacheinander ausführen, Ausgabe beobachten:
   - `/help` oder `/ecc:plan`
   - `/ecc:code-review`
3. Ausgabeformat jedes Befehls notieren

**Abnahmekriterien**:
- Funktion und Ausgabeformat jedes Befehls verstanden

---

## Übung 3: Einfachen Agent erstellen

**Ziel**: Agent-Dateiformat verstehen

**Schritte**:
1. Vorhandene Agent-Dateien im `agents/` Verzeichnis ansehen
2. Format von `planner.md` als Referenz verwenden
3. Einen einfachen `hello-agent.md` erstellen, der eine Begrüßung ausgibt

**Abnahmekriterien**:
- Neues Agent-Dateiformat korrekt
- YAML Frontmatter (name, description, tools, model) enthalten

---

## Übung 4: Hook ausführen

**Ziel**: Hook-Mechanismus verstehen

**Schritte**:
1. Hook-Konfiguration im `hooks/` Verzeichnis ansehen
2. `node scripts/hooks/run-with-flags.js --help` ausführen (falls unterstützt)
3. Hook-Auslösezeitpunkt und Ausführungslogik verstehen

**Abnahmekriterien**:
- Arbeitsprinzip von Hooks beschreiben können
- Unterschied zwischen PreToolUse und PostToolUse verstehen

---

## Übung 5: Komplexe Aufgabe

**Ziel**: Ein kleines Projekt abschließen

**Aufgabe**: Einen Namensformatierer erstellen

**Funktionsanforderungen**:
- `formatName(name)` - Als "Nachname, Vorname" formatieren
- `initials(name)` - Initialen zurückgeben

**Ausführungsablauf**:
1. `/ecc:plan` zum Planen verwenden
2. `/tdd` zum Entwickeln verwenden
3. `/ecc:code-review` zum Prüfen verwenden

**Abnahmekriterien**:
- Beide Funktionen vollständig implementiert
- Testfälle enthalten
- Codereview bestanden

---

[Zurück zum Einsteiger-Verzeichnis](../README.md)