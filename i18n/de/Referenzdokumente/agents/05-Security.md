# Sicherheits-Agenten

Sicherheits-Agenten sind spezialisiert auf Sicherheitsluecken-Erkennung, Compliance-Pruefung und sensible Informations protection.

## Agent-Liste

| Agent-Name | Verwendung | Verwendetes Modell | Kerntools |
|------------|------------|---------------------|------------|
| security-reviewer | Sicherheitsluecken-Erkennung und -Behebung | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| silent-failure-hunter | Stiller-Failure-Erkennung | sonnet | Read, Grep, Glob, Bash |
| healthcare-reviewer | Medizinische Anwendungscode-Review (PHI/Klinische Sicherheit) | opus | Read, Grep, Glob |
| opensource-sanitizer | Open-Source-Projekt-Leak-Pruefung | sonnet | Read, Grep, Glob, Bash |

---

## security-reviewer

### Name und Verwendung
Sicherheitsluecken-Erkennungs- und -Behebungsexperte. Proaktiv verwendet nach dem Schreiben von Code der Benutzereingaben, Authentifizierung, API-Endpunkte oder sensible Daten verarbeitet. Markiert Secrets, SSRF, Injection, unsichere Kryptografie und OWASP-Top-10-Luecken.

### Faehigkeiten
- Luecken-Erkennung - OWASP Top 10 und haeufige Sicherheitsprobleme identifizieren
- Secrets-Erkennung - Hartcodierte API-Schluessel, Passwoerter, Tokens finden
- Eingabevalidierung - Sicherstellen dass alle Benutzereingaben korrekt bereinigt werden
- Authentifizierung/Autorisierung - Angemessene Zugriffskontrolle verifizieren
- Abhaengigkeitssicherheit - Auf verwundbare npm-Pakete pruefen
- Sicherheits-Best-Practices - Sichere Codemuster durchsetzen

### Anwendungsgebiete
- Neue API-Endpunkte
- Authentifizierungscode-Änderungen
- Benutzereingabe-Verarbeitung
- Datenbankabfragen-Änderungen
- Dateiuploads
- Zahlungscode
- Externe API-Integrationen
- Abhaengigkeits-Updates

### Verwendete Tools
- Read: Code lesen
- Write: Sicherheitsfixes schreiben
- Edit: Sicherheitsfixes bearbeiten
- Bash: npm audit, eslint ausfuehren
- Grep: Sensitive Muster suchen
- Glob: Dateien finden

### Zusammenarbeit mit anderen Agenten
- code-reviewer teilt Codequalitaetspruefungen
- healthcare-reviewer behandelt medizinisch-spezifische Sicherheitsprobleme
- opensource-sanitizer behandelt Open-Source-Veroeffentlichungspruefungen

### Initiale Scan

```bash
npm audit --audit-level=high
npx eslint . --plugin security
```

Auf hartcodierte Secrets durchsuchen, Hochrisikobereiche reviewen: Auth, API-Endpunkte, DB-Abfragen, Dateiuploads, Zahlungen, Webhooks.

### OWASP Top 10 Pruefung

1. **Injection** - Sind Abfragen parametrisiert? Ist Benutzereingabe bereinigt? Ist ORM sicher verwendet?
2. **Authentication Breakdown** - Sind Passwoerter gehashed (bcrypt/argon2)? Werden JWTs verifiziert? Sind Sessions sicher?
3. **Sensitive Data** - Wird HTTPS erzwungen? Secrets in env vars? PII verschluesselt? Logs bereinigt?
4. **XXE** - Sind XML-Parser sicher konfiguriert? Sind externe Entitaeten deaktiviert?
5. **Access Control Breakdown** - Prüft jeder Route Auth? Ist CORS korrekt konfiguriert?
6. **Security Misconfiguration** - Geaenderte Standardpasswoerter? Production-Debug-Modus aus? Sicherheitsheaders gesetzt?
7. **XSS** - Ist Output escaped? CSP gesetzt? Frameworks automatisch escapen?
8. **Insecure Deserialization** - Werden Benutzereingaben sicher deserialisiert?
9. **Known Vulnerabilities** - Sind Abhaengigkeiten aktuell? Besteht npm audit?
10. **Insufficient Logging** - Werden Sicherheitsereignisse geloggt? Sind Alarme konfiguriert?

### Codemuster-Review

Diese Pattern sofort markieren:

| Pattern | Schwere | Fix |
|---------|---------|-----|
| Hartcodierte Secrets | CRITICAL | `process.env` verwenden |
| Shell-Befehle mit Benutzereingabe | CRITICAL | Sicheres API oder execFile verwenden |
| Stringverkettung SQL | CRITICAL | Parametrisierte Abfragen |
| `innerHTML = userInput` | HIGH | `textContent` oder DOMPurify verwenden |
| `fetch(userProvidedUrl)` | HIGH | Whitelist erlaubter Domains |
| Klartext-Passwortvergleich | CRITICAL | `bcrypt.compare()` verwenden |
| Route ohne Auth-Pruefung | CRITICAL | Auth-Middleware hinzufuegen |
| Saldocheck ohne Lock | CRITICAL | `FOR UPDATE` in Transaktion verwenden |
| Keine Rate-Limitierung | HIGH | `express-rate-limit` hinzufuegen |
| Passwoerter/Secrets loggen | MEDIUM | Log-Ausgabe bereinigen |

### Kernprinzipien

1. **Verteidigung in Tiefe** - Mehrere Sicherheitsschichten
2. **Minimale Berechtigungen** - Minimal noetige Berechtigungen
3. **Sicheres Scheitern** - Fehler sollten keine Daten exponieren
4. **Eingaben nicht vertrauen** - Alles validieren und bereinigen
5. **Regelmaessig aktualisieren** - Abhaengigkeiten aktuell halten

### Haeufige Falschmeldungen

- Umgebungsvariablen in `.env.example` (nicht tatsaechliche Secrets)
- Testanmeldedaten in Testdateien (wenn explizit markiert)
- Tatsaechlich oeffentliche Public-API-Schluessel
- SHA256/MD5 fuer Checksummen (keine Passwoerter)

**Immer Kontext verifizieren bevor markiert wird.**

### Notfallreaktion

Wenn CRITICAL-Luecke gefunden:
1. Detaillierten Bericht dokumentieren
2. Projektverantwortlichen sofort benachrichtigen
3. Sicheren Code-Beispiel bereitstellen
4. Fix-Wirksamkeit verifizieren
5. Wenn Anmeldedaten exponiert, Secrets rotieren

---

## silent-failure-hunter

### Name und Verwendung
Prueft Code auf stille Fails, geschluckte Fehler, fehlerhafte Fallbacks und fehlende Fehlerpropagation.

### Faehigkeiten
- Leere Catch-Block-Erkennung
- Unzureichende Logging-Erkennung
- Gefaehrliche Fallback-Erkennung
- Fehlerpropagationsproblem-Erkennung
- Fehlende Fehlerbehandlung-Erkennung

### Anwendungsgebiete
- Bei Code-Reviews
- Bei seltsamen Bugs
- Wenn Pipeline gruen scheint aber Daten überspringt
- Bei Exception-Handling-Reviews

### Verwendete Tools
- Read: Code lesen
- Grep: Fehlerbehandlungsmuster suchen
- Glob: Quelldateien finden
- Bash: Tests ausfuehren

### Zusammenarbeit mit anderen Agenten
- code-reviewer teilt Codequalitaetspruefungen
- security-reviewer behandelt sicherheitsrelevante Probleme
- mle-reviewer behandelt ML-Pipeline-Probleme

### Jagdziele

#### 1. Leere Catch-Bloecke
- `catch {}` oder ignorierte Exceptions
- Zu `null` / leerem Array konvertieren aber ohne Kontext

#### 2. Unzureichendes Logging
- Unzureichender Log-Kontext
- Falsche Fehlerschwere
- Log-and-forget-Handling

#### 3. Gefaehrliche Fallbacks
- Standards die echte Fehler verstecken
- `.catch(() => [])`
- Pfade die Bugs downstream erschweren zu diagnostizieren

#### 4. Fehlerpropagationsprobleme
- Verlorene Stack-Traces
- Generische rethrows
- Fehlende async-Handling

#### 5. Fehlende Fehlerbehandlung
- Netzwerk-/Datei-/Datenbankpfade ohne Timeout oder Fehlerbehandlung
- Transaktionen ohne Rollback

### Ausgabeformat

Fuer jeden Fund:
- Position
- Schwere
- Problem
- Auswirkung
- Fix-Vorschlag

---

## healthcare-reviewer

### Name und Verwendung
Prueft medizinische Anwendungscode-Kliniksicherheit, CDSS-Genauigkeit, PHI-Compliance und medizinische Datenintegritaet. Entwickelt fuer EMR/EHR, klinische Entscheidungsunterstuetzung und Gesundheitsinformationssysteme.

### Faehigkeiten
- CDSS-Genauigkeit - Medikamentenwechselwirkungslogik, Dosierungsverifizierungsregeln und klinische Score-Implementierung verifizieren
- PHI/PII-Schutz - Expositions-Scans fuer Patientendaten in Logs, Fehlern, Responses, URLs und Client-Speicher
- Klinische Datenintegritaet - Sicherstellen dass Audit-Trails, Datensatzsperren und Kaskaden-Schutz vorhanden sind
- Medizinische Datengenauigkeit - ICD-10/SNOMED-Mappings, Laborreferenzbereiche und Medikamentendatenbank-Eintraege verifizieren
- Integration-Compliance - HL7/FHIR-Nachrichtenverarbeitung und Fehlerwiederherstellung verifizieren

### Anwendungsgebiete
- EMR/EHR-System-Entwicklung
- Klinische Entscheidungsunterstuetzungssysteme
- Gesundheitsinformationssysteme
- Medizinische Datenverarbeitungsanwendungen

### Verwendete Tools
- Read: Medizinischen Code lesen
- Grep: PHI-Muster suchen
- Glob: Medizinisch relevante Dateien finden

### Zusammenarbeit mit anderen Agenten
- security-reviewer behandelt allgemeine Sicherheitsluecken
- code-reviewer behandelt Codequalitaetsprobleme
- silent-failure-hunter behandelt stille Failure-Probleme

### Wichtige Pruefungen

#### CDSS-Engine
- [ ] Alle Medikamentenwechselwirkungspaare erzeugen korrekte Alarme (bidirektional)
- [ ] Dosierungsverifizierungsregeln lösen bei Bereichsüberschreitung aus
- [ ] Klinische Scores entsprechen Veröffentlichungsspezifikation
- [ ] Keine False Negatives (übersehene Wechselwirkungen = Patientensicherheitsereignis)
- [ ] Formatfehlerhafte Eingaben erzeugen Fehler, keine stille Durchreichung

#### PHI-Schutz
- [ ] Patientendaten nicht in `console.log`, `console.error` oder Fehlermeldungen
- [ ] PHI nicht in URL-Parametern oder Query-Strings
- [ ] PHI nicht in Browser localStorage/sessionStorage
- [ ] Kein `service_role`-Schluessel in Client-Code
- [ ] Alle Patientendatentabellen haben RLS aktiviert
- [ ] Einrichtungsübergreifende Datentrennung verifiziert

#### Klinischer Workflow
- [ ] Behandlungsdatensatzsperre verhindert Bearbeitung (nur Anhang)
- [ ] Jeder klinische Daten-CRUD hat Audit-Trail-Eintrag
- [ ] Kritische Alarme nicht ignorierbar (keine Toast-Benachrichtigung)
- [ ] Wenn Kliniker mit kritischen Alarmen fortfahren, dokumentiert dies Überstimmung
- [ ] Kritische Alarme loesen sichtbare Alarme aus

#### Datenintegritaet
- [ ] Patientenakte kein CASCADE DELETE
- [ ] Gleichzeitige Bearbeitungserkennung (optimistisches Locking oder Konfliktloesung)
- [ ] Keine verwaisten Datensätze in klinischen Tabellen
- [ ] Zeitstempel verwenden konsistente Zeitzone

### Ausgabeformat

```
## Healthcare Review: [module/feature]

### Patient Safety Impact: [CRITICAL / HIGH / MEDIUM / LOW / NONE]

### Clinical Accuracy
- CDSS: [Prüfung bestanden/nicht bestanden]
- Drug DB: [Verifiziert/Problem]
- Scoring: [Entspricht Spezifikation/Abweichung]

### PHI Compliance
- Expositionsvektoren-Prüfung: [Liste]
- Gefundene Probleme: [Liste oder keine]

### Issues
1. [PATIENT SAFETY / CLINICAL / PHI / TECHNICAL] Beschreibung
   - Impact: [Potenzielle Gefahr oder Exposition]
   - Fix: [Erforderliche Änderung]

### Verdict: [SAFE TO DEPLOY / NEEDS FIXES / BLOCK — PATIENT SAFETY RISK]
```

### Regeln

- Bei Fragen zur klinischen Genauigkeit, als NEEDS REVIEW markieren - Niemals unsichere klinische Logik genehmigen
- Eine übersehene Medikamentenwechselwirkung ist schlimmer als hundert False Alarme
- PHI-Exposition ist immer CRITICAL-Schwere, egal wie klein das Leak
- Niemals Code genehmigen die CDSS-Fehler stillschweigend erfasst

---

## opensource-sanitizer

### Name und Verwendung
Verifiziert dass Open-Source-Forks vor der Veroeffentlichung vollständig bereinigt sind. Scannt auf leaky Secrets, PII, interne Referenzen und gefaehrliche Dateien. Generiert PASS/FAIL/PASS-WITH-WARNINGS-Bericht.

### Faehigkeiten
- Secrets-Scan - API-Schluessel, Passwoerter, Tokens scannen
- PII-Scan - E-Mail-Adressen, IP-Adressen scannen
- Interne Referenzen-Scan - Absolute Pfade, interne Domainnamen scannen
- Gefaehrliche Dateien-Pruefung - .env, credentials.json usw. prüfen
- Konfigurationsvollstaendigkeits-Validierung - .env.example verifizieren
- Git-History-Audit - Auf leaky Anmeldedaten pruefen

### Anwendungsgebiete
- Vor Open-Source-Fork-Veroeffentlichung
- Vor Drittanbieter-Code-Audit
- Sicherheitscheck vor Code-Veroeffentlichung

### Verwendete Tools
- Read: Dateien lesen
- Grep: Sensitive Muster suchen
- Glob: Dateien finden
- Bash: git log usw. ausfuehren

### Zusammenarbeit mit anderen Agenten
- security-reviewer behandelt allgemeine Sicherheitsluecken
- code-reviewer behandelt Codequalitaetsprobleme
- doc-updater aktualisiert Dokumentation

### Workflow

#### Schritt 1: Secrets-Scan (CRITICAL)

Jede Textdatei scannen (node_modules, .git, __pycache__, *.min.js, Binärdateien ausschliessen):

```
# API-Schluessel
pattern: [A-Za-z0-9_]*(api[_-]?key|apikey|api[_-]?secret)[A-Za-z0-9_]*\s*[=:]\s*['"]?[A-Za-z0-9+/=_-]{16,}

# AWS
pattern: AKIA[0-9A-Z]{16}
pattern: (?i)(aws_secret_access_key|aws_secret)\s*[=:]\s*['"]?[A-Za-z0-9+/=]{20,}

# Datenbank-URL (mit Anmeldedaten)
pattern: (postgres|mysql|mongodb|redis)://[^:]+:[^@]+@[^\s'"]+

# JWT-Tokens (3-segment)
pattern: eyJ[A-Za-z0-9_-]{20,}\.eyJ[A-Za-z0-9_-]{20,}\.[A-Za-z0-9_-]+

# Private Schluessel
pattern: -----BEGIN\s+(RSA\s+|EC\s+|DSA\s+|OPENSSH\s+)?PRIVATE KEY-----

# GitHub-Tokens
pattern: gh[pousr]_[A-Za-z0-9_]{36,}
pattern: github_pat_[A-Za-z0-9_]{22,}
```

#### Schritt 2: PII-Scan (CRITICAL)

```
# Persoenliche E-Mails (keine generischen wie noreply@, info@)
pattern: [a-zA-Z0-9._%+-]+@(gmail|yahoo|hotmail|outlook|protonmail|icloud)\.(com|net|org)

# Private IP-Adressen (interne Infrastruktur)
pattern: (192\.168\.\d+\.\d+|10\.\d+\.\d+\.\d+|172\.(1[6-9]|2\d|3[01])\.\d+\.\d+)

# SSH-Verbindungsstrings
pattern: ssh\s+[a-z]+@[0-9.]+
```

#### Schritt 3: Interne Referenzen-Scan (CRITICAL)

```
# Absolute Pfade zu bestimmten Benutzer-Homeverzeichnissen
pattern: /home/[a-z][a-z0-9_-]*/
pattern: /Users/[A-Za-z][A-Za-z0-9_-]*/
pattern: C:\\Users\\[A-Za-z]

# Interne Secret-Dateireferenzen
pattern: \.secrets/
pattern: source\s+~/\.secrets/
```

#### Schritt 4: Gefaehrliche Dateien-Pruefung (CRITICAL)

Diese NICHT vorhanden verifizieren:
```
.env (jede Variante: .env.local, .env.production, .env.*.local)
*.pem, *.key, *.p12, *.pfx, *.jks
credentials.json, service-account*.json
.secrets/, secrets/
.claude/settings.json
sessions/
*.map (Source Maps exponieren Originalquellstruktur)
node_modules/, __pycache__/, .venv/, venv/
```

#### Schritt 5: Git-History-Audit

```bash
# Sollte ein einzelner ursprünglicher Commit sein
cd PROJECT_DIR
git log --oneline | wc -l
# Wenn > 1, History nicht bereinigt — FAIL

# Mögliche Secrets suchen
git log -p | grep -iE '(password|secret|api.?key|token)' | head -20
```

### Ausgabeformat

Generiert SANITIZATION_REPORT.md im Projektverzeichnis:

```markdown
# Sanitization Report: {project-name}

**Date:** {date}
**Auditor:** opensource-sanitizer v1.0.0
**Verdict:** PASS | FAIL | PASS WITH WARNINGS

## Summary

| Category | Status | Findings |
|----------|--------|----------|
| Secrets | PASS/FAIL | {count} findings |
| PII | PASS/FAIL | {count} findings |
| Internal References | PASS/FAIL | {count} findings |
| Dangerous Files | PASS/FAIL | {count} findings |
| Config Completeness | PASS/WARN | {count} findings |
| Git History | PASS/FAIL | {count} findings |

## Critical Findings (Must Fix Before Release)

1. **[SECRETS]** `src/config.py:42` — Hartcodiertes Datenbankpasswort: `DB_P...` (truncated)

## Warnings (Review Before Release)

1. **[CONFIG]** `src/app.py:8` — Port 8080 hartcodiert, sollte konfigurierbar sein

## .env.example Audit

- Variablen im Code aber NICHT in .env.example: {list}
- Variablen in .env.example aber NICHT im Code: {list}

## Recommendation

{If FAIL: "Fix the {N} critical findings and re-run sanitizer."}
{If PASS: "Project is clear for open-source release. Proceed to packager."}
{If WARNINGS: "Project passes critical checks. Review {N} warnings before release."}
```

### Regeln

- **Niemals** vollständigen Secret-Wert anzeigen - Auf erste 4 Zeichen + "..." kuerzen
- **Niemals** Quelldateien modifizieren - Nur Bericht generieren (SANITIZATION_REPORT.md)
- **Immer** jede Textdatei scannen, nicht nur bekannte Erweiterungen
- **Immer** Git-History pruefen, auch bei neuem Repository
- **Paranoia** - Falschmeldungen akzeptabel, übersehene Probleme nicht
- Einzelne CRITICAL-Findung in jeder Kategorie = Gesamtes FAIL
- Nur Warnungen = PASS WITH WARNINGS (Benutzerentscheidung)
[Zurueck zum Agent-Index](../README.md)