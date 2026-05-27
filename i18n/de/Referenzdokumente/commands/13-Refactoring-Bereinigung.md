# Refactoring- und Bereinigungs-Befehle

## Ueberblick

Refactoring- und Bereinigungs-Befehle werden verwendet fuer Coderefactoring, Totcode-Bereinigung und automatische Updates.

## Befehlsliste

### /refactor-clean

**Zweck**: Toten Code loeschen + Duplikate zusammenfuehren

**Beschreibung**: Identifiziert und loescht ungenutzten Code, fuehrt duplizierte Codeteile zusammen.

**Analysiert**:
- Ungenutzte Funktionen und Variablen
- Duplizierte Codebloecke
- Veraltete Kommentare
- Ungenutzte Imports

**Werkzeugunterstuetzung**:
- `knip` - Totcode-Erkennung
- `depcheck` - Ungenutzte Abhaengigkeiten erkennen
- `ts-prune` - TypeScript-Totcode-Erkennung

---

### /auto-update

**Zweck**: Automatische Update-Faehigkeiten

**Beschreibung**: Aktualisiert Projektabhaengigkeiten und Konfigurationen automatisch auf neueste Versionen.

**Aktualisiert**:
- npm/pip-Pakete
- Konfigurationsdateien
- Codemigrationen
- Versionskompatibilitaetspruefungen

---

## Zugehoerige Befehle

- `/refactor-clean` - Refactoring-Bereinigung
- `/auto-update` - Automatisches Update