# Sicherheit

Dieser Abschnitt behandelt Sicherheitsreviews, Schwachstellen-Scanning und Sicherheits-Best-Practices fuer verschiedene Frameworks.

---

## Sicherheits-Review

### Verwendungszweck
Sicherheits-Review-Faehigkeiten stellen sicher, dass aller Code Sicherheits-Best-Practices folgt und potenzielle Schwachstellen identifiziert werden.

### Verwendung
- Authentifizierung oder Autorisierung implementieren
- Benutzereingaben oder Datei-Uploads verarbeiten
- Neue API-Endpunkte erstellen
- Schluessel oder Anmeldedaten verwenden
- Zahlungsfunktionen implementieren
- Sensible Daten speichern oder uebertragen
- Drittanbieter-APIs integrieren

### Kernkonzepte

**1. Schluesselverwaltung**
Niemals Schluessel in den Quellcode hardcodieren.

**2. Eingabevalidierung**
Alle Benutzereingaben muessen validiert werden.

**3. SQL-Injection-Schutz**
Parametrisierte Abfragen verwenden.

**4. XSS-Schutz**
Benutzer-bereitgestelltes HTML bereinigen.

**5. CSRF-Schutz**
CSRF-Token bei zustandsaendernden Operationen verwenden.

**6. Rate-Limiting**
Rate-Limiting auf allen API-Endpunkten aktivieren.

### Sicherheits-Checkliste

| Pruefpunkt | Beschreibung |
|--------|------|
| Schluessel | Keine hardcodierten Schluessel, alle Schluessel in Umgebungsvariablen |
| Eingabevalidierung | Alle Benutzereingaben mit Schema validieren |
| SQL-Injection | Alle Abfragen verwenden parametrisierte Abfragen |
| XSS | Benutzerinhalte werden bereinigt |
| CSRF | Schutz aktiviert |
| Authentifizierung | Korrekte Token-Verarbeitung |
| Autorisierung | Rollenpruefungen vorhanden |
| Rate-Limiting | Alle Endpunkte aktiviert |
| HTTPS | Produktionsumgebung erzwingt HTTPS |
| Sicherheitsheader | CSP, X-Frame-Options konfiguriert |
| Fehlerbehandlung | Fehlermeldungen geben keine sensiblen Daten preis |
| Logging | Keine sensiblen Daten aufzeichnen |
| Abhaengigkeiten | Aktuell, keine Schwachstellen |

### Verwendungsbeispiel

```typescript
// Schluesselverwaltung - Falsche Methode
const apiKey = "sk-proj-xxxxx"  // Das niemals tun

// Schluesselverwaltung - Richtige Methode
const apiKey = process.env.OPENAI_API_KEY
if (!apiKey) {
  throw new Error('OPENAI_API_KEY not configured')
}

// Eingabevalidierung
import { z } from 'zod'

const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
})

// SQL-Injection-Schutz - Falsche Methode
const query = `SELECT * FROM users WHERE email = '${userEmail}'`  // Gefaehrlich!

// SQL-Injection-Schutz - Richtige Methode
const { data } = await supabase
  .from('users')
  .select('*')
  .eq('email', userEmail)

// XSS-Schutz
import DOMPurify from 'isomorphic-dompurify'

function renderUserContent(html: string) {
  const clean = DOMPurify.sanitize(html, {
    ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'p'],
    ALLOWED_ATTR: []
  })
  return <div dangerouslySetInnerHTML={{ __html: clean }} />
}
```

---

## Sicherheits-Scanning

### Verwendungszweck
AgentShield verwenden, um Sicherheitsluecken, Fehlkonfigurationen und Injection-Risiken in Claude Code-Konfiguration (`.claude/`-Verzeichnis) zu scannen.

### Verwendung
- Neues Claude Code-Projekt einrichten
- Nach dem Aendern von `.claude/settings.json`, `CLAUDE.md` oder MCP-Konfiguration
- Vor dem Committen von Konfigurationsaenderungen
- Beim Hinzufuegen eines neuen Repositorys zu einer bestehenden Claude Code-Konfiguration
- Regelmaessige Sicherheitspruefungen

### Scan-Inhalt

| Datei | Pruefpunkte |
|------|--------|
| `CLAUDE.md` | Hardcodierte Schluessel, Auto-Run-Anweisungen, Prompt-Injection-Muster |
| `settings.json` | Ueberrmaessig lockere allowance list, fehlende Ablehnungsliste, gefaehrliche Umgehungsflags |
| `mcp.json` | Riskante MCP-Server, hardcodierte env-Schluessel, npx Supply-Chain-Risiken |
| `hooks/` | Command-Injection durch Interpolation, Datenleck, silencie Fehlerunterdrueckung |
| `agents/*.md` | Uneingeschraenkte Tool-Zugriffe, Prompt-Injection-Flaechen, fehlende Modellspezifikationen |

### Verwendungsbeispiel

```bash
# Basis-Scan
npx ecc-agentshield scan

# Scan mit Pfad
npx ecc-agentshield scan --path /path/to/.claude

# Minimaler Schweregrad filtern
npx ecc-agentshield scan --min-severity medium

# JSON-Format ( fuer CI/CD)
npx ecc-agentshield scan --format json

# Automatische Reparatur
npx ecc-agentshield scan --fix

# Opus-Tiefenanalyse (ANTHROPIC_API_KEY erforderlich)
export ANTHROPIC_API_KEY=your-key
npx ecc-agentshield scan --opus --stream
```

### Schweregrad-Stufen

| Note | Punkte | Bedeutung |
|------|------|------|
| A | 90-100 | Sichere Konfiguration |
| B | 75-89 | Kleinere Probleme |
| C | 60-74 | Aufmerksamkeit erforderlich |
| D | 40-59 | Erhebliches Risiko |
| F | 0-39 | Kritische Schwachstelle |

---

## Django-Sicherheit

### Verwendungszweck
Django-Sicherheits-Best-Practices zum Schutz von Anwendungen vor haeufigen Schwachstellen.

### Verwendung
- Django-Authentifizierung und -Autorisierung einrichten
- Benutzerberechtigungen und -rollen implementieren
- Produktionssicherheitseinstellungen konfigurieren
- Django-Anwendungssicherheitsprobleme ueberpruefen
- Django-Anwendungen in Produktion bereitstellen

### Kernsicherheitseinstellungen

```python
# settings/production.py
DEBUG = False  # Wichtig: Niemals True in Produktion

ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS', '').split(',')

# Sicherheitsheader
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
SECURE_HSTS_SECONDS = 31536000  # 1 Jahr
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_BROWSER_XSS_FILTER = True
X_FRAME_OPTIONS = 'DENY'

# Cookie-Sicherheit
SESSION_COOKIE_HTTPONLY = True
CSRF_COOKIE_HTTPONLY = True
SESSION_COOKIE_SAMESITE = 'Lax'

# Passwortvalidierung
AUTH_PASSWORD_VALIDATORS = [
    {'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator', 'OPTIONS': {'min_length': 12}},
    {'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator'},
]
```

### SQL-Injection-Schutz

```python
# Sicher: Django ORM escaped automatisch
def get_user(username):
    return User.objects.get(username=username)  # Sicher

# Sicher: raw() mit Parametern
def search_users(query):
    return User.objects.raw('SELECT * FROM users WHERE username = %s', [query])

# Gefaehrlich: Niemals direkt interpolieren
def get_user_bad(username):
    return User.objects.raw(f'SELECT * FROM users WHERE username = {username}')  # Schwachstelle!
```

### XSS-Schutz

```django
{# Django-escaped Variablen standardmaessig automatisch - Sicher #}
{{ user_input }}  {# HTML wird escaped #}

{# Als sicher markieren nur fuer vertrauenswuerdige Inhalte #}
{{ trusted_html|safe }}  {# Kein Escaping #}

{# Sichere HTML-Verarbeitung mit Template-Filter #}
{{ user_input|striptags }}  {# Alle HTML-Tags entfernen #}
```

### CSRF-Schutz

```django
{# Im Template verwenden #}
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Absenden</button>
</form>

{# AJAX-Anfrage #}
function getCookie(name) {
    let cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        const cookies = document.cookie.split(';');
        for (let i = 0; i < cookies.length; i++) {
            const cookie = cookies[i].trim();
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}

fetch('/api/endpoint/', {
    method: 'POST',
    headers: {
        'X-CSRFToken': getCookie('csrftoken'),
        'Content-Type': 'application/json',
    },
    body: JSON.stringify(data)
});
```

### Rate-Limiting

```python
# settings.py
REST_FRAMEWORK = {
    'DEFAULT_THROTTLE_CLASSES': [
        'rest_framework.throttling.AnonRateThrottle',
        'rest_framework.throttling.UserRateThrottle'
    ],
    'DEFAULT_THROTTLE_RATES': {
        'anon': '100/day',
        'user': '1000/day',
    }
}
```

### Schnelle Sicherheits-Checkliste

| Pruefpunkt | Beschreibung |
|--------|------|
| `DEBUG = False` | Niemals mit DEBUG in Produktion ausfuehren |
| Nur HTTPS | SSL erzwingen, sichere Cookies |
| Starke Schluessel | SECRET_KEY aus Umgebungsvariablen verwenden |
| Passwortvalidierung | Alle Passwortvalidatoren aktivieren |
| CSRF-Schutz | Standardmaessig aktiviert, nicht deaktivieren |
| XSS-Schutz | Django escaped standardmaessig, kein `|safe` fuer Benutzereingaben verwenden |
| SQL-Injection | ORM verwenden, niemals Strings in Abfragen konkatenieren |
| Dateiuploads | Dateitypen und -groessen validieren |
| Rate-Limiting | API-Endpunkte begrenzen |
| Sicherheitsheader | CSP, X-Frame-Options, HSTS |
| Logging | Sicherheitsereignisse aufzeichnen |
| Updates | Django und Abhaengigkeiten aktuell halten |

---

## Zugehoerige Faehigkeiten

| Faehigkeit | Verwendungszweck |
|------|------|
| `security-review` | Umfassende Sicherheits-Checkliste |
| `security-scan` | Claude Code-Konfigurationsscan |
| `django-security` | Django-Sicherheits-Best-Practices |
| `coding-standards` | Coding-Standards und Sicherheitsgrundlagen |