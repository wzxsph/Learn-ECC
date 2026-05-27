# Segurança

Esta seção cobre revisão de segurança, scanning de vulnerabilidades e Melhores-Práticas de segurança para vários frameworks.

---

## Revisão de Segurança

### Propósito
Habilidades de revisão de segurança garantem que todo código siga Melhores-Práticas de segurança, identificando vulnerabilidades potenciais.

### Quando Usar
- Implementar autenticação ou autorização
- Processar input de usuário ou upload de arquivos
- Criar novos endpoints de API
- Usar chaves ou credenciais
- Implementar funcionalidades de pagamento
- Armazenar ou transmitir dados sensíveis
- Integrar com APIs de terceiros

### Conceitos Centrais

**1. Gerenciamento de Chaves**
Nunca hardcodar chaves no código fonte.

**2. Validação de Input**
Todo input de usuário deve ser validado.

**3. Proteção contra SQL Injection**
Usar queries parametrizadas.

**4. Proteção contra XSS**
Sanitizar HTML fornecido pelo usuário.

**5. Proteção CSRF**
Usar tokens CSRF em operações que alteram estado.

**6. Rate Limiting**
Habilitar rate limiting em todos os endpoints de API.

### Checklist de Segurança

| Item de Verificação | Descrição |
|--------|------|
| Chaves | Sem chaves hardcoded, todas em variáveis de ambiente |
| Validação de Input | Todo input de usuário validado com schema |
| SQL Injection | Todas as queries usam parâmetros |
| XSS | Conteúdo do usuário é sanitizado |
| CSRF | Proteção habilitada |
| Autenticação | Tratamento correto de tokens |
| Autorização | Verificações de papel em lugar |
| Rate Limiting | Habilitado em todos os endpoints |
| HTTPS | Forçado em ambiente de produção |
| Headers de Segurança | CSP, X-Frame-Options configurados |
| Tratamento de Erros | Mensagens de erro não expõem dados sensíveis |
| Logs | Não registram dados sensíveis |
| Dependências | Atualizadas, sem vulnerabilidades |

### Exemplo de Uso

```typescript
// Gerenciamento de chaves - má prática
const apiKey = "sk-proj-xxxxx"  // Nunca fazer isso

// Gerenciamento de chaves - prática correta
const apiKey = process.env.OPENAI_API_KEY
if (!apiKey) {
  throw new Error('OPENAI_API_KEY not configured')
}

// Validação de input
import { z } from 'zod'

const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
})

// Proteção contra SQL injection - prática errada
const query = `SELECT * FROM users WHERE email = '${userEmail}'`  // Perigoso!

// Proteção contra SQL injection - prática errada correta
const { data } = await supabase
  .from('users')
  .select('*')
  .eq('email', userEmail)

// Proteção contra XSS
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

## Scanning de Segurança

### Propósito
Usar AgentShield para escanear vulnerabilidades de segurança, erros de configuração e riscos de injeção na configuração do Claude Code (diretório `.claude/`).

### Quando Usar
- Configurar novo projeto Claude Code
- Após modificar `.claude/settings.json`, `CLAUDE.md` ou configurações MCP
- Antes de fazer commit de mudanças de configuração
- Ao adicionar um novo repositório com configuração Claude Code existente
- Verificações periódicas de segurança

### Conteúdo do Scan

| Arquivo | Itens de Verificação |
|------|--------|
| `CLAUDE.md` | Chaves hardcoded, instruções de auto-execução, padrões de injeção de prompt |
| `settings.json` | Allow lists excessivamente permissivos, falta de deny lists, flags perigosas de bypass |
| `mcp.json` | Servidores MCP arriscados, chaves env hardcoded, riscos de supply chain npx |
| `hooks/` | Injeção de comandos via interpolação, vazamento de dados, supressão silenciosa de erros |
| `agents/*.md` | Acesso irrestrito a ferramentas, superfície de injeção de prompt, falta de especificações de modelo |

### Exemplo de Uso

```bash
# Scan básico
npx ecc-agentshield scan

# Scan com caminho especificado
npx ecc-agentshield scan --path /path/to/.claude

# Filtrar por severidade mínima
npx ecc-agentshield scan --min-severity medium

# Formato JSON (para CI/CD)
npx ecc-agentshield scan --format json

# Auto correção
npx ecc-agentshield scan --fix

# Análise profunda Opus (requer ANTHROPIC_API_KEY)
export ANTHROPIC_API_KEY=your-key
npx ecc-agentshield scan --opus --stream
```

### Níveis de Severidade

| Grade | Pontuação | Significado |
|------|------|------|
| A | 90-100 | Configuração segura |
| B | 75-89 | Problemas menores |
| C | 60-74 | Requer atenção |
| D | 40-59 | Risco significativo |
| F | 0-39 | Vulnerabilidade crítica |

---

## DjangoSegurança

### Propósito
Melhores-Práticas de segurança Django, protegendo aplicações contra vulnerabilidades comuns.

### Quando Usar
- Configurar autenticação e autorização Django
- Implementar permissões de usuário e papéis
- Configurar settings de segurança para produção
- Revisar problemas de segurança de aplicações Django
- Fazer deploy de aplicações Django em produção

### Configurações de Segurança Centrais

```python
# settings/production.py
DEBUG = False  # Crítico: nunca usar True em produção

ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS', '').split(',')

# Headers de segurança
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
SECURE_HSTS_SECONDS = 31536000  # 1 ano
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_BROWSER_XSS_FILTER = True
X_FRAME_OPTIONS = 'DENY'

# Segurança de Cookies
SESSION_COOKIE_HTTPONLY = True
CSRF_COOKIE_HTTPONLY = True
SESSION_COOKIE_SAMESITE = 'Lax'

# Validação de senhas
AUTH_PASSWORD_VALIDATORS = [
    {'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator', 'OPTIONS': {'min_length': 12}},
    {'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator'},
]
```

### Proteção contra SQL Injection

```python
# Seguro: Django ORM faz escape automático de parâmetros
def get_user(username):
    return User.objects.get(username=username)  # Seguro

# Seguro: usando raw() com parâmetros
def search_users(query):
    return User.objects.raw('SELECT * FROM users WHERE username = %s', [query])

# Perigoso: nunca usar interpolação direta
def get_user_bad(username):
    return User.objects.raw(f'SELECT * FROM users WHERE username = {username}')  # Vulnerável!
```

### Proteção contra XSS

```django
{# Django faz escape automático de variáveis por padrão - Seguro #}
{{ user_input }}  {# HTML será escapado #}

{# Marcar explicitamente como seguro apenas para conteúdo confiado #}
{{ trusted_html|safe }}  {# Não escapa #}

{# Usar filtro de template para sanitizar HTML com segurança #}
{{ user_input|striptags }}  {# Remove todas as tags HTML #}
```

### Proteção CSRF

```django
{# No template #}
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Enviar</button>
</form>

{# Solicitações AJAX #}
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

### Checklist Rápido de Segurança

| Item de Verificação | Descrição |
|--------|------|
| `DEBUG = False` | Nunca executar em produção com DEBUG |
| Apenas HTTPS | Forçar SSL, cookies seguros |
| Chave forte | Usar SECRET_KEY de variáveis de ambiente |
| Validação de senhas | Habilitar todos os validadores de senha |
| Proteção CSRF | Habilitada por padrão, não desabilitar |
| Proteção XSS | Django faz escape por padrão, não usar `|safe` em input de usuário |
| SQL Injection | Usar ORM, nunca concatenar strings em queries |
| Upload de arquivos | Validar tipo e tamanho dos arquivos |
| Rate Limiting | Rate limit em endpoints de API |
| Headers de Segurança | CSP, X-Frame-Options, HSTS |
| Logs | Registrar eventos de segurança |
| Updates | Manter Django e dependências atualizados |

---

## Habilidades Relacionadas

| Habilidade | Propósito |
|------|------|
| `security-review` | Checklist completo de revisão de segurança |
| `security-scan` | Scan de configuração do Claude Code |
| `django-security` | Melhores-Práticas de segurança Django |
| `coding-standards` | Padrões de codificação e fundamentos de segurança |