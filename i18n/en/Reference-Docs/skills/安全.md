# Security

This section covers security review, vulnerability scanning, and security best practices for various frameworks.

---

## Security Review

### Purpose
Security review skills ensure all code follows security best practices and identify potential vulnerabilities.

### When to Use
- Implementing authentication or authorization
- Handling user input or file uploads
- Creating new API endpoints
- Using keys or credentials
- Implementing payment functionality
- Storing or transmitting sensitive data
- Integrating third-party APIs

### Core Concepts

**1. Key Management**
Never hardcode keys in source code.

**2. Input Validation**
All user input must be validated.

**3. SQL Injection Prevention**
Use parameterized queries.

**4. XSS Prevention**
Sanitize user-provided HTML.

**5. CSRF Protection**
Use CSRF tokens on state-changing operations.

**6. Rate Limiting**
Enable rate limiting on all API endpoints.

### Security Checklist

| Check | Description |
|-------|-------------|
| Keys | No hardcoded keys, all keys in environment variables |
| Input validation | All user input validated with schema |
| SQL injection | All queries use parameterized queries |
| XSS | User content is sanitized |
| CSRF | Protection enabled |
| Authentication | Proper token handling |
| Authorization | Role checks in place |
| Rate limiting | Enabled on all endpoints |
| HTTPS | Enforced in production |
| Security headers | CSP, X-Frame-Options configured |
| Error handling | Error messages don't expose sensitive data |
| Logging | No sensitive data logged |
| Dependencies | Updated, no vulnerabilities |

### Usage Examples

```typescript
// Key management - wrong way
const apiKey = "sk-proj-xxxxx"  // Never do this

// Key management - correct way
const apiKey = process.env.OPENAI_API_KEY
if (!apiKey) {
  throw new Error('OPENAI_API_KEY not configured')
}

// Input validation
import { z } from 'zod'

const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
})

// SQL injection prevention - wrong way
const query = `SELECT * FROM users WHERE email = '${userEmail}'`  // Dangerous!

// SQL injection prevention - correct way
const { data } = await supabase
  .from('users')
  .select('*')
  .eq('email', userEmail)

// XSS prevention
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

## Security Scanning

### Purpose
Use AgentShield to scan for security vulnerabilities, misconfigurations, and injection risks in Claude Code configuration (`.claude/` directory).

### When to Use
- Setting up a new Claude Code project
- After modifying `.claude/settings.json`, `CLAUDE.md`, or MCP configuration
- Before committing configuration changes
- When joining a new repository with existing Claude Code configuration
- For regular security checks

### Scanning Content

| File | Checks |
|------|--------|
| `CLAUDE.md` | Hardcoded keys, auto-run instructions, prompt injection patterns |
| `settings.json` | Overly permissive allowlists, missing blocklists, dangerous bypass flags |
| `mcp.json` | Risky MCP servers, hardcoded env keys, npx supply chain risks |
| `hooks/` | Command injection via interpolation, data leakage, silent error suppression |
| `agents/*.md` | Unrestricted tool access, prompt injection surface, missing model specs |

### Usage Examples

```bash
# Basic scan
npx ecc-agentshield scan

# Scan specific path
npx ecc-agentshield scan --path /path/to/.claude

# Minimum severity filter
npx ecc-agentshield scan --min-severity medium

# JSON format (for CI/CD)
npx ecc-agentshield scan --format json

# Auto-fix
npx ecc-agentshield scan --fix

# Opus deep analysis (requires ANTHROPIC_API_KEY)
export ANTHROPIC_API_KEY=your-key
npx ecc-agentshield scan --opus --stream
```

### Severity Levels

| Grade | Score | Meaning |
|-------|-------|---------|
| A | 90-100 | Secure configuration |
| B | 75-89 | Minor issues |
| C | 60-74 | Needs attention |
| D | 40-59 | Significant risk |
| F | 0-39 | Severe vulnerability |

---

## Django Security

### Purpose
Django security best practices, protecting applications from common vulnerabilities.

### When to Use
- Setting up Django authentication and authorization
- Implementing user permissions and roles
- Configuring production security settings
- Reviewing Django application security issues
- Deploying Django applications to production

### Core Security Settings

```python
# settings/production.py
DEBUG = False  # Critical: never use True in production

ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS', '').split(',')

# Security headers
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
SECURE_HSTS_SECONDS = 31536000  # 1 year
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_BROWSER_XSS_FILTER = True
X_FRAME_OPTIONS = 'DENY'

# Cookie security
SESSION_COOKIE_HTTPONLY = True
CSRF_COOKIE_HTTPONLY = True
SESSION_COOKIE_SAMESITE = 'Lax'

# Password validation
AUTH_PASSWORD_VALIDATORS = [
    {'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator', 'OPTIONS': {'min_length': 12}},
    {'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator'},
]
```

### SQL Injection Prevention

```python
# Safe: Django ORM auto-escapes parameters
def get_user(username):
    return User.objects.get(username=username)  # Safe

# Safe: raw() with parameters
def search_users(query):
    return User.objects.raw('SELECT * FROM users WHERE username = %s', [query])

# Dangerous: never interpolate directly
def get_user_bad(username):
    return User.objects.raw(f'SELECT * FROM users WHERE username = {username}')  # Vulnerability!
```

### XSS Prevention

```django
{# Django auto-escapes variables by default - Safe #}
{{ user_input }}  {# HTML will be escaped #}

{# Mark as safe only for trusted content #}
{{ trusted_html|safe }}  {# Not escaped #}

{# Use template filter to safely handle HTML #}
{{ user_input|striptags }}  {# Remove all HTML tags #}
```

### CSRF Protection

```django
{# In template #}
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Submit</button>
</form>

{# AJAX request #}
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

### Rate Limiting

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

### Quick Security Checklist

| Check | Description |
|-------|-------------|
| `DEBUG = False` | Never run with DEBUG in production |
| HTTPS only | Enforce SSL, secure cookies |
| Strong keys | Use SECRET_KEY from environment |
| Password validation | Enable all validators |
| CSRF protection | Enabled by default, don't disable |
| XSS prevention | Django escapes by default, don't use `|safe` on user input |
| SQL injection | Use ORM, never concatenate strings in queries |
| File uploads | Validate file type and size |
| Rate limiting | Throttle API endpoints |
| Security headers | CSP, X-Frame-Options, HSTS |
| Logging | Log security events |
| Updates | Keep Django and dependencies updated |

---

## Related Skills

| Skill | Purpose |
|-------|---------|
| `security-review` | Comprehensive security checklist |
| `security-scan` | Claude Code configuration scanning |
| `django-security` | Django security best practices |
| `coding-standards` | Coding standards and security basics |
