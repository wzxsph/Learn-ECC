# Bảo Mật

Phần này cover security review, vulnerability scanning và security best practice cho các framework.

---

## Security Review

### Mục đích
Security review skill đảm bảo tất cả code tuân theo security best practice, identify potential vulnerability.

### Khi nào sử dụng
- Implement authentication hoặc authorization
- Handle user input hoặc file upload
- Tạo new API endpoint
- Sử dụng key hoặc credential
- Implement payment function
- Store hoặc transmit sensitive data
- Integrate third-party API

### Core concept

**1. Key Management**
Không bao giờ hard-code key trong source code.

**2. Input Validation**
Tất cả user input phải được validate.

**3. SQL Injection Prevention**
Sử dụng parameterized query.

**4. XSS Prevention**
Sanitize user-provided HTML.

**5. CSRF Protection**
Sử dụng CSRF token trên state-changing operation.

**6. Rate Limiting**
Enable rate limiting trên tất cả API endpoint.

### Security Checklist

| Check item | Mô tả |
|--------|------|
| Key | Không có hard-coded key, tất cả key trong environment variable |
| Input Validation | Tất cả user input sử dụng schema validation |
| SQL Injection | Tất cả query sử dụng parameterized query |
| XSS | User content được sanitize |
| CSRF | Enable protection |
| Authentication | Correct token handling |
| Authorization | Role check in place |
| Rate Limiting | Enable trên tất cả endpoint |
| HTTPS | Enforce trong production |
| Security Header | CSP, X-Frame-Options configured |
| Error Handling | Error message không expose sensitive data |
| Logging | Không log sensitive data |
| Dependency | Updated, no vulnerability |

### Usage Example

```typescript
// Key Management - Wrong way
const apiKey = "sk-proj-xxxxx"  // Never do this

// Key Management - Correct way
const apiKey = process.env.OPENAI_API_KEY
if (!apiKey) {
  throw new Error('OPENAI_API_KEY not configured')
}

// Input Validation
import { z } from 'zod'

const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
})

// SQL Injection Prevention - Wrong way
const query = `SELECT * FROM users WHERE email = '${userEmail}'`  // Dangerous!

// SQL Injection Prevention - Correct way
const { data } = await supabase
  .from('users')
  .select('*')
  .eq('email', userEmail)

// XSS Prevention
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

### Mục đích
Sử dụng AgentShield scan security vulnerability, configuration error và injection risk trong Claude Code configuration (`.claude/` directory).

### Khi nào sử dụng
- Setup new Claude Code project
- Sau khi modify `.claude/settings.json`, `CLAUDE.md` hoặc MCP configuration
- Trước khi commit configuration change
- Khi join new repo với existing Claude Code configuration
- Regular security check

### Scan Content

| File | Check item |
|------|--------|
| `CLAUDE.md` | Hard-coded key, auto-run instruction, prompt injection pattern |
| `settings.json` | Overly permissive allowlist, missing blocklist, dangerous bypass flag |
| `mcp.json` | Risky MCP server, hard-coded env key, npx supply chain risk |
| `hooks/` | Command injection via interpolation, data leak, silent error suppression |
| `agents/*.md` | Unrestricted tool access, prompt injection surface, missing model specification |

### Usage Example

```bash
# Basic scan
npx ecc-agentshield scan

# Scan specified path
npx ecc-agentshield scan --path /path/to/.claude

# Minimum severity filter
npx ecc-agentshield scan --min-severity medium

# JSON format (for CI/CD)
npx ecc-agentshield scan --format json

# Auto fix
npx ecc-agentshield scan --fix

# Opus deep analysis (cần ANTHROPIC_API_KEY)
export ANTHROPIC_API_KEY=your-key
npx ecc-agentshield scan --opus --stream
```

### Severity Level

| Grade | Score | Meaning |
|------|------|------|
| A | 90-100 | Secure configuration |
| B | 75-89 | Minor issue |
| C | 60-74 | Need attention |
| D | 40-59 | Significant risk |
| F | 0-39 | Critical vulnerability |

---

## Django Security

### Mục đích
Django security best practice, protect application khỏi common vulnerability.

### Khi nào sử dụng
- Setup Django authentication và authorization
- Implement user permission và role
- Configure production security setting
- Review Django application security issue
- Deploy Django application to production

### Core Security Setting

```python
# settings/production.py
DEBUG = False  # Critical: Never use True in production

ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS', '').split(',')

# Security header
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
# Safe: Django ORM auto-escapes parameter
def get_user(username):
    return User.objects.get(username=username)  # Safe

# Safe: raw() với parameter
def search_users(query):
    return User.objects.raw('SELECT * FROM users WHERE username = %s', [query])

# Dangerous: Never interpolate directly
def get_user_bad(username):
    return User.objects.raw(f'SELECT * FROM users WHERE username = {username}')  # Vulnerability!
```

### XSS Prevention

```django
{# Django auto-escapes variable by default - Safe #}
{{ user_input }}  {# HTML will be escaped #}

{# Explicitly mark as safe chỉ for trusted content #}
{{ trusted_html|safe }}  {# Not escaped #}

{# Use template filter to safe handle HTML #}
{{ user_input|striptags }}  {# Remove all HTML tags #}
```

### CSRF Protection

```django
{# Use in template #}
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

| Check item | Mô tả |
|--------|------|
| `DEBUG = False` | Never run in production with DEBUG |
| HTTPS only | Enforce SSL, secure cookie |
| Strong key | Use SECRET_KEY from environment variable |
| Password validation | Enable all password validators |
| CSRF protection | Enable by default, don't disable |
| XSS protection | Django escapes by default, don't use `\|safe` on user input |
| SQL injection | Use ORM, never concatenate string in query |
| File upload | Validate file type và size |
| Rate limiting | Rate limit API endpoint |
| Security header | CSP, X-Frame-Options, HSTS |
| Logging | Log security event |
| Update | Keep Django và dependency updated |

---

## Related Skill

| Skill | Usage |
|------|------|
| `security-review` | Comprehensive security checklist |
| `security-scan` | Claude Code configuration scan |
| `django-security` | Django security best practice |
| `coding-standards` | Coding standard và security foundation |