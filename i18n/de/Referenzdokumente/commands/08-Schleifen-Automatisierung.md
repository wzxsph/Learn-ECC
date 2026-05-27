# Schleifen- und Automatisierungs-Befehle

## Ueberblick

Schleifen- und Automatisierungs-Befehle werden verwendet um autonome Schleifen-Arbeitsmodi zu starten und zu verwalten.

## Befehlsliste

### /loop-start

**Zweck**: Verwalteten autonomen Schleifenmodus mit sicheren Standardwerten und klaren Stoppbedingungen starten

**Beschreibung**: Startet verwalteten autonomen Schleifenmodus mit sicheren Standardwerten und klaren Stoppbedingungen.

**Verwendung**: `/loop-start [pattern] [--mode safe|fast]`

**Parameter**:

| Parameter | Wert | Beschreibung |
|---|---|---|
| `pattern` | `sequential` | Aufgaben sequentiell ausfuehren, eine nach der anderen |
| | `continuous-pr` | Kontinuierlicher PR-Erstellungs- und Merge-Schleife |
| | `rfc-dag` | RFC-basierter gerichteterazyklischer Graph-Workflow |
| | `infinite` | Unendliche Schleife (erfordert klare Stoppbedingung) |
| `--mode` | `safe` (Standard) | Strikte Qualitaets-Gates und Pruefpunkte |
| | `fast` | Weniger Gates fuer Geschwindigkeit |

**Workflow**:
1. Repository-Status und Branch-Strategie verifizieren
2. Schleifenmodus und zugehoerige Modellhierarchie-Strategie auswaehlen
3. Erforderliche hooks/profile fuer ausgewaehlten Modus aktivieren
4. Schleifenplan erstellen und Runbook in `.claude/plans/` schreiben
5. Start- und Ueberwachungsbefehle ausgeben

**Erforderliche Sicherheitspruefungen**:
- Verifizieren dass Tests vor erster Schleife bestehen
- Sicherstellen dass `ECC_HOOK_PROFILE` nicht global deaktiviert ist
- Sicherstellen dass Schleife klare Stoppbedingung hat

**Parameterweiterleitung**:
- `$ARGUMENTS`: `[pattern]` und optional `[--mode safe|fast]`
- pattern ist optional (Standard ist sequential)
- mode-Flag ist optional (Standard ist safe)

**Best Practices**:
- Vor infinite-Modus klare Stoppbedingungen setzen
- Fast-Modus nur auf ausreichend getestetem Code verwenden
- Schleifenfortschritt ueberwachen, `/loop-status` zum Pruefen des Status verwenden

**Haeufige Fallstricke**:
- Schleife starten ohne Tests zu verifizieren
- infinite-Modus ohne klare Stoppbedingung verwenden
- Versuchen sichereren Modus zu verwenden wenn hooks global deaktiviert sind

**Integration mit anderen Befehlen**:
- `/loop-status` zum Ueberwachen laufender Schleifen verwenden
- `/santa-loop` fuer Santa-Style-Schleifen verwenden
- `/checkpoint` zum Erstellen von Pruefpunkten an kritischen Knotenpunkten verwenden

---

### /loop-status

**Zweck**: Status einer laufenden Schleife pruefen

**Beschreibung**: Zeigt Status und Fortschritt der aktuell laufenden Schleife.

---

### /santa-loop

**Zweck**: Santa-Style autonome Schleife

**Beschreibung**: Ein spezieller Stil des autonomen Schleifenmodus.

---

## Zugehoerige Befehle

- `/loop-start` - Schleife starten
- `/loop-status` - Status pruefen
- `/santa-loop` - Santa-Style