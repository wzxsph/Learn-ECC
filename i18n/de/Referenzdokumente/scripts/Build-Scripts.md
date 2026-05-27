# Build-Skripte

Dieses Dokument beschreibt die Build- und Release-Skripte des ECC-Projekts.

---

## release.sh

**Pfad**: `scripts/release.sh`

Plugin-Versions-Upgrade und Release-Skript.

### Funktionen

1. **Versionsvalidierung**
   - Bestaetigt dass semver-Format Versionsnummer angegeben (X.Y.Z oder X.Y.Z-prerelease)
   - Bestaetigt dass aktueller Branch main ist
   - Bestaetigt dass Arbeitsbereich sauber ist

2. **Dateien aktualisieren**
   - Versionsnummern in allen Paket-/Plugin-Manifesten aktualisieren
   - Versionsreferenzen in Dokumentation aktualisieren
   - Agent-Konfiguration aktualisieren

3. **Build-Verifizierung**
   - OpenCode-Build ausfuehren
   - Build-Tests ausfuehren
   - Plugin-Manifest-Tests ausfuehren

4. **Git-Operationen**
   - Alle Aenderungen stagen
   - Commit erstellen
   - Tag erstellen und pushen

### Verwendung

```bash
# Neue Version veroeffentlichen
./scripts/release.sh 1.5.0

# Vorabveroentlichung
./scripts/release.sh 2.0.0-rc.1
```

### Aktualisierte Dateien

| Datei | Aktualisierung |
|------|----------|
| `package.json` | version-Feld |
| `package-lock.json` | version und packages[""].version |
| `AGENTS.md` | Versionszeile |
| `docs/tr/AGENTS.md` | Sürüm-Zeile |
| `docs/zh-CN/AGENTS.md` | Versions-Zeile |
| `agent.yaml` | version-Feld |
| `VERSION` | Versionsdatei |
| `.claude-plugin/plugin.json` | version-Feld |
| `.claude-plugin/marketplace.json` | version-Feld |
| `.agents/plugins/marketplace.json` | ecc-Plugin-Version |
| `.codex-plugin/plugin.json` | version-Feld |
| `.opencode/package.json` | version-Feld |
| `.opencode/package-lock.json` | version und packages[""].version |
| `.opencode/plugins/ecc-hooks.ts` | Banner-Version |
| `README.md` | Versions-Tabellenzeile |
| `README.zh-CN.md` | Versions-Tabellenzeile |
| `docs/tr/README.md` | Neueste Veroeffentlichungstitel |
| `docs/pt-BR/README.md` | Neueste Veroeffentlichungstitel |
| `docs/zh-CN/README.md` | Neueste Veroeffentlichungstitel |
| `docs/SELECTIVE-INSTALL-ARCHITECTURE.md` | repoVersion-Beispiel |

### Voraussetzungen

- Git-Arbeitsbereich muss sauber sein
- Muss auf main-Branch sein
- Alle Manifestdateien müssen vorhanden sein

---

## build-opencode.js

**Pfad**: `scripts/build-opencode.js`

Baut TypeScript-Code des OpenCode-Plugins.

### Build-Prozess

1. `dist`-Verzeichnis bereinigen
2. TypeScript-Compiler finden (`typescript/bin/tsc`)
3. `.opencode`-Verzeichnis mit tsconfig.json im Projektroot kompilieren

### Verwendung

```bash
# OpenCode bauen
node scripts/build-opencode.js

# Automatisch als Teil des Releases ausgefuehrt
./scripts/release.sh 1.5.0
```

### Voraussetzungen

Erfordert installierte Development-Abhaengigkeiten im Root, TypeScript-Compiler muss verfuegbar sein.

---

## Sonstige Skripte

### gan-harness.sh

**Pfad**: `scripts/gan-harness.sh`

GAN (General Agent Network) Testtool-bezogen.

### orchestrate-codex-worker.sh

**Pfad**: `scripts/orchestrate-codex-worker.sh`

Orchestriert Codex-Worker-Prozesse.

### sync-ecc-to-codex.sh

**Pfad**: `scripts/sync-ecc-to-codex.sh`

Synchronisiert ECC zu Codex.

---

## Veroeffentlichungs-Workflow

```
1. Sicherstellen dass auf main-Branch und Arbeitsbereich sauber
2. Release-Skript mit Versionsnummer ausfuehren
3. Skript automatisch:
   ├── Voraussetzungen validieren
   ├── Alle Versionsreferenzen aktualisieren
   ├── OpenCode bauen
   ├── Tests zur Validierung ausfuehren
   ├── Git-Commit erstellen
   └── Tag pushen
4. GitHub Actions erkennt Tag und veroeffentlicht automatisch
```