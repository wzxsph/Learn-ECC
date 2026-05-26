# 04 - Erste Aufgabe

## Lernziele

- Den vollständigen Entwicklungsprozess verstehen
- Den vollständigen Prozess /ecc:plan → /ecc:code-review → /ecc:build-fix → Commit beherrschen
- In der Lage sein, eine einfache Funktion selbstständig zu entwickeln

---

## Vollständiger Entwicklungsprozess

ECC empfiehlt folgenden Entwicklungsprozess:

```
Step 1: /ecc:plan        → Aufgabe planen
Step 2: /ecc:code-review → Code prüfen
Step 3: /ecc:build-fix   → Build reparieren
Step 4: Code commiten     → Git Commit
```

---

## Step 1: Mit /ecc:plan planen

### Eingabe

Angenommen, wir wollen eine einfache Rechner-Funktion implementieren:

```
/ecc:plan Implementiere einen Rechner, der vier Operationen unterstützt: Addition, Subtraktion, Multiplikation, Division
```

### Erwartete Ausgabe

Der /ecc:plan Befehl wird:

1. **Anforderung wiederholen** - "Implementiere einen Rechner, der vier Grundoperationen unterstützt: Addition, Subtraktion, Multiplikation, Division..."
2. **Risikobewertung** - "Zu beachten: Division durch Null..."
3. **Schritt-für-Schritt-Plan** -
   - Phase 1: Rechner-Kernfunktion erstellen
   - Phase 2: Vier Operationen implementieren
   - Phase 3: Fehlerbehandlung hinzufügen
   - Phase 4: Unit-Tests schreiben
4. **Auf Bestätigung warten** - "Bitte bestätige, ob der Plan starten kann"

### Nach Bestätigung

Nach Benutzerbestätigung wartet ECC auf Benutzereingabe, um jede Phase zu starten.

### Tipps

- PRD-Dateimodus für detailliertere Planung verwenden
- Komplexe Funktionen sollten zuerst PRD schreiben, dann planen

---

## Step 2: Mit /ecc:code-review prüfen

### Eingabe

Nach dem Code-Schreiben, ausführen:

```
/ecc:code-review
```

### Erwartete Ausgabe

/ecc:code-review wird:

1. Geänderte Dateien sammeln
2. 7 Kategorien prüfen:
   - Korrektheit
   - Typsicherheit
   - Musterkonformität
   - Sicherheit
   - Performance
   - Vollständigkeit
   - Wartbarkeit
3. Review-Bericht generieren, CRITICAL/HIGH/MEDIUM/LOW Stufen markieren

### Review-Bericht-Beispiel

```
## Codereview-Bericht

### CRITICAL
- [ ] Hartcodierte Anmeldedaten: src/auth.js Zeile 23

### HIGH
- [ ] Funktion zu lang: src/calculator.js Zeilen 45-78 (34 Zeilen)
- [ ] Fehlerbehandlung fehlt: src/calculator.js Divisionsfunktion

### MEDIUM
- [ ] Namenskonvention nicht eingehalten: Variable `tmp` sollte `temporary` sein

### Aktionsvorschläge
1. Hartcodierten API-Key entfernen
2. Divisionsfunktion in calculator.js refaktorieren, Nullprüfung hinzufügen
3. Zu lange Funktion aufteilen
```

### Tipps

- /ecc:code-review unbedingt vor dem Commit ausführen
- CRITICAL und HIGH Probleme müssen behoben werden

---

## Step 3: Mit /ecc:build-fix reparieren

### Eingabe

Wenn der Build fehlschlägt, ausführen:

```
/ecc:build-fix
```

### Erwartete Ausgabe

/ecc:build-fix wird:

1. **Build-System erkennen** - "TypeScript-Projekt erkannt"
2. **Build ausführen** - "tsc --noEmit ausführen..."
3. **Fehler analysieren** - Nach Dateien gruppiert
4. **Schrittweise beheben** - Einen Fehler nach dem anderen
5. **Verifizieren** - Nach jeder Reparatur neu bauen

### Beispielausgabe

```
[build-fix] TypeScript-Projekt erkannt
[build-fix] tsc --noEmit ausführen...
[build-fix] 3 Fehler gefunden:

Fehler 1: src/calculator.ts:15 - Typ 'string' kann nicht Typ 'number' zugewiesen werden
Fehler 2: src/calculator.ts:28 - Eigenschaft 'result' existiert nicht
Fehler 3: src/calculator.ts:42 - Parameter 'b' wird nie verwendet

[build-fix] Beginne Fehler 1 zu beheben...
[build-fix] Reparatur abgeschlossen, verifiziere...
[build-fix] Build erfolgreich!
```

### Tipps

- Build-Reparatur ist inkrementell, jeweils einen Fehler beheben
- Kann jederzeit mit Strg+C gestoppt werden

---

## Step 4: Code commiten

### Commit-Prozess

1. Sicherstellen, dass alle Tests bestehen
2. Sicherstellen, dass Build erfolgreich
3. Sicherstellen, dass Codereview-Probleme behoben

### Git Commit-Standard

ECC verwendet Conventional Commits Format:

```
<type>: <description>

[optional body]
```

**Type-Typen**:
- `feat` - Neue Funktion
- `fix` - Bug behoben
- `refactor` - Refaktorierung
- `docs` - Dokumentation
- `test` - Test
- `chore` - Wartung
- `perf` - Performance-Optimierung
- `ci` - CI-Konfiguration

### Commit-Beispiel

```
feat: Vier Grundoperationen für Rechner implementiert

- Additionsfunktion hinzugefügt
- Subtraktionsfunktion hinzugefügt
- Multiplikationsfunktion hinzugefügt
- Divisionsfunktion hinzugefügt (mit Nullprüfung)

Löst: Issue #123
```

### Tipps

- `/ecc:verify quick` für schnelle Verifizierung vor Commit verwenden
- Große Funktionen in mehrere Commits aufteilen, kleine Funktionen in einem Commit

---

## Vollständiges Beispiel

### Szenario

Eine einfache String-Utility-Funktion implementieren: `truncate(str, maxLength)`, die String auf angegebene Länge kürzt.

### Step 1: Planen

```
/ecc:plan truncate Funktion implementieren, die String auf angegebene Länge kürzt
```

Ausgabe:
```
## Anforderungswiederholung
Implementiere truncate(str, maxLength) Funktion, der Teil über maxLength wird mit ... ersetzt

## Risikobewertung
- Behandlung wenn maxLength kleiner als 3
- Behandlung wenn str leer oder null
- Behandlung von Unicode-Zeichen

## Schritt-für-Schritt-Plan
1. src/string-utils.ts erstellen
2. truncate Funktion implementieren
3. Grenzfälle-Tests hinzufügen
4. JSDoc-Dokumentation hinzufügen

Bitte bestätige, ob gestartet werden kann.
```

### Step 2: Implementieren

(Nach Benutzerbestätigung implementiert ECC den Code)

### Step 3: Review

```
/ecc:code-review
```

Ausgabe:
```
## Codereview-Bericht
Status: APPROVED

### Review-Ergebnisse
- Korrektheit: Bestanden
- Typsicherheit: Bestanden
- Musterkonformität: Bestanden
- Sicherheit: Bestanden
- Performance: Bestanden
- Vollständigkeit: Mehr Grenztests empfohlen
- Wartbarkeit: Bestanden

### Empfehlungen
- Leeren String-Test hinzufügen
- Null-Wert-Test hinzufügen
```

### Step 4: Build-Verifizierung

```
/ecc:build-fix
```

```
[build-fix] TypeScript-Projekt erkannt
[build-fix] tsc --noEmit ausführen...
[build-fix] Build erfolgreich!
```

### Step 5: Commit

```
feat: truncate String-Kürzungsfunktion implementiert

- Unterstützt angegebene Maximallänge
- Überschreitender Teil wird mit ... ersetzt
- Behandelt leere Strings und Null-Werte
```

---


## Zusammenfassung

1. **Vollständiger Prozess**: /ecc:plan → Implementieren → /ecc:code-review → /ecc:build-fix → Commit
2. **/ecc:plan** 用于规划，确保方向正确
3. **/ecc:code-review** 用于审查，发现问题
4. **/ecc:build-fix** 用于修复构建错误
5. **提交** 使用 conventional commits 格式

---

## 下一步

- 完成课程学习后，进行 [exercises/Einstiegsübungen.md](./exercises/Einstiegsübungen.md)
- 进入 Stage 2: 核心命令进阶