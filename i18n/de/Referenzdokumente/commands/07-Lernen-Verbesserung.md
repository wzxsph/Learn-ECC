# Lern- und Verbesserungs-Befehle

## Ueberblick

Lern- und Verbesserungs-Befehle werden verwendet um Muster aus Sitzungen zu extrahieren, Instincts zu verfolgen und Organisationswissen zu verwalten.

## Befehlsliste

### /learn

**Zweck**: Wiederverwendbare Muster aus der aktuellen Sitzung extrahieren und als Kandidaten-Skills speichern

**Beschreibung**: Analysiert die aktuelle Sitzung und extrahiert alle Muster die wert sind als Skills gespeichert zu werden. `/learn` kann jederzeit waehrend der Sitzung verwendet werden.

**Extrahierte Inhaltstypen**:

| Typ | Beschreibung |
|---|---|
| **Fehlerloesungs-Muster** | Welcher Fehler? Was war die Grundursache? Was hat ihn behoben? Kann fuer aehnliche Fehler wiederverwendet werden? |
| **Debugging-Techniken** | Nicht-offensichtliche Debugging-Schritte, effektive Werkzeugkombinationen, Diagnosemuster |
| **Workarounds** | Bibliotheks-Quirks, API-Einschraenkungen, versionsspezifische Fixes |
| **Projektspezifische Muster** | Entdeckte Codebase-Konventionen, Architekturentscheidungen, Integrationsmuster |

**Ausgabeformat**: Gespeichert in `~/.claude/skills/learned/[pattern-name].md`

```markdown
# [Descriptive Pattern Name]

**Extracted:** [Date]
**Context:** [Brief description of when this applies]

## Problem
[What problem this solves - be specific]

## Solution
[The pattern/technique/workaround]

## Example
[Code example if applicable]

## When to Use
[Trigger conditions - what should activate this skill]
```

**Workflow**:
1. Sitzung nach extrahierbaren Mustern ueberpruefen
2. Wertvollste/wiederverwendbarste Erkenntnisse identifizieren
3. Skill-Datei entwerfen
4. Benutzerbestaetigung einholen bevor gespeichert wird
5. In `~/.claude/skills/learned/` speichern

**Best Practices**:
- Keine trivialen Fixes extrahieren (Tippfehler, einfache Syntaxfehler)
- Keine Einmal-Probleme extrahieren (spezifischer API-Ausfall usw.)
- Auf Muster fokussieren die zukuenftige Sitzungen Zeit sparen
- Skills fokussiert halten - ein Muster pro Skill

---

### /learn-eval

**Zweck**: Muster extrahieren + Selbstbewertung der Qualitaet

**Beschreibung**: Fuehrt Qualitaetsbewertung waehrend der Musterextraktion durch.

---

### /evolve

**Zweck**: Instincts analysieren + Evolutionsstruktur empfehlen

**Beschreibung**: Analysiert erlernte Instincts und bietet strukturelle Evolutionsempfehlungen.

---

### /promote

**Zweck**: Projekt-Instincts auf globale Ebene heben

**Beschreibung**: Hebt projektspezifische Instincts auf global verfuegbares Wissen.

---

### /instinct-status

**Zweck**: Alle erlernten Instincts anzeigen

**Beschreibung**: Zeigt Status und Konfidenz aller aktuellen Instincts.

---

### /instinct-export

**Zweck**: Instincts in Datei exportieren

**Beschreibung**: Exportiert Instincts in sharebares Dateiformat.

---

### /instinct-import

**Zweck**: Instincts aus Datei/URL importieren

**Beschreibung**: Importiert Instincts aus Datei oder URL ins System.

---

### /skill-create

**Zweck**: Git-Historie analysieren+Skill-Datei generieren

**Beschreibung**: Analysiert Projekt-Git-Historie und generiert wiederverwendbare Skill-Dateien aus Mustern.

**Workflow**:
1. Git-Commit-Historie analysieren
2. Wiederholende Muster identifizieren
3. Skill-Datei generieren
4. Verifizieren und speichern

---

### /skill-health

**Zweck**: Skill-Portfolio-Gesundheits-Dashboard

**Beschreibung**: Zeigt Gesundheitsstatus und Nutzungsstatistiken des Skill-Portfolios.

---

### /rules-distill

**Zweck**: Skills scannen + domaenenuebergreifende Prinzipien extrahieren

**Beschreibung**: Scannt alle Skills und extrahiert domaenenuebergreifende universelle Prinzipien.

---

## Zugehoerige Befehle

- `/learn` - Muster extrahieren
- `/skill-create` - Skill aus Git-Historie generieren
- `/instinct-status` - Instinct-Status anzeigen