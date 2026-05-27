# Güvenlik

Bu bölüm Güvenlik incelemesi, Açık Tarama ve çeşitli Çerçeveler'in Güvenlik En İyi Uygulamalarını kapsar.

---

## Güvenlik İncelemesi

### Kullanım Açıklaması
Güvenlik incelemesi becerileri, tüm kodun Güvenlik En İyi Uygulamalarını takip etmesini sağlar ve potansiyel güvenlik açıklarını tanımlar.

### Kullanım Zamanı
- Kimlik doğrulama veya yetkilendirme Gerçekleştirirken
- Kullanıcı girdisi veya dosya yüklemesi işlerken
- Yeni API uç noktaları oluştururken
- Anahtar veya kimlik bilgileri kullanırken
- Ödeme işlevi Gerçekleştirirken
- Hassas verileri depolarken veya iletirken
- Üçüncü taraf API'leri entegre ederken

### Temel Kavramlar

**1. Anahtar Yönetimi**
Kaynak koduna asla anahtar kodlamayın.

**2. Girdi Doğrulama**
Tüm kullanıcı girdilerini Zorunlu doğrulayın.

**3. SQL Enjeksiyon Koruması**
Parametreli Sorgu Kullanın.

**4. XSS Koruması**
Kullanıcı tarafından Sağlanan HTML'i temizleyin.

**5. CSRF Koruması**
Durum değişikliği işlemlerinde CSRF belirteçleri kullanın.

**6. Hız Sınırlama**
Tüm API uç noktalarında hız sınırlama etkinleştirin.

### Güvenlik Kontrol Listesi

| Kontrol | Tanım |
|--------|------|
| Anahtarlar | Sabit kodlanmış anahtar yok, tüm anahtarlar ortam değişkenlerinde |
| Girdi Doğrulama | Tüm kullanıcı girdileri schema doğrulama kullanır |
| SQL enjeksiyon | Tüm sorgular Parametreli Sorgu Kullanır |
| XSS | Kullanıcı içeriği temizlenir |
| CSRF | Koruma etkin |
| Kimlik Doğrulama | Doğru belirteç işleme |
| Yetkilendirme | Rol kontrolleri yerinde |
| Hız Sınırlama | Tüm uç noktalar etkin |
| HTTPS | Üretim ortamında zorunlu |
| Güvenlik başlıkları | CSP, X-Frame-Options yapılandırılmış |
| Hata Yönetimi | Hata mesajları hassas verileri ifşa etmez |
| Günlükleme | Hassas verileri günlüğe kaydetmez |
| Bağımlılıklar | Güncel, güvenlik açığı yok |

### Kullanım Örneği

```typescript
// Anahtar yönetimi - yanlış yaklaşım
const apiKey = "sk-proj-xxxxx"  // Asla bunu yapmayın

// Anahtar yönetimi - doğru yaklaşım
const apiKey = process.env.OPENAI_API_KEY
if (!apiKey) {
  throw new Error('OPENAI_API_KEY not configured')
}

// Girdi doğrulama
import { z } from 'zod'

const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
})

// SQL Enjeksiyon Koruması - yanlış yaklaşım
const query = `SELECT * FROM users WHERE email = '${userEmail}'`  // Tehlikeli!

// SQL Enjeksiyon Koruması - doğru yaklaşım
const { data } = await supabase
  .from('users')
  .select('*')
  .eq('email', userEmail)

// XSS koruması
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

## Güvenlik Taraması

### Kullanım Açıklaması
Claude Code yapılandırmasındaki (`.claude/` Dizin) Güvenlik Açıkları, yapılandırma hataları ve enjeksiyon risklerini taramak için AgentShield'ı kullanır.

### Kullanım Zamanı
- Yeni Claude Code projesi kurarken
- `.claude/settings.json`, `CLAUDE.md` veya MCP yapılandırmasını değiştirdikten sonra
- Yapılandırma değişikliklerini işlemeden önce
- Yeni Claude Code yapılandırması olan bir depoya katılırken
- Düzenli Güvenlik kontrolleri yaparken

### Tarama İçeriği

| Dosya | Kontroller |
|------|--------|
| `CLAUDE.md` | Sabit kodlanmış anahtarlar, otomatik çalıştırma talimatları, İpucu enjeksiyon kalıpları |
| `settings.json` | Aşırı gevşek izin listeleri, eksik reddetme listeleri, tehlikeli baypas bayrakları |
| `mcp.json` | Riskli MCP sunucuları, sabit kodlanmış ortam anahtarları, npx tedarik zinciri riskleri |
| `hooks/` | Araç ile enterpolasyon komut enjeksiyonu, veri sızıntısı, sessiz hata bastırma |
| `agents/*.md` | Sınırsız araç erişimi, İpucu enjeksiyon yüzeyi, eksik model özellikleri |

### Kullanım Örneği

```bash
# Temel tarama
npx ecc-agentshield scan

# Belirtilen yolu tara
npx ecc-agentshield scan --path /path/to/.claude

# Minimum önem derecesi filtreleme
npx ecc-agentshield scan --min-severity medium

# JSON formatı (CI/CD için Kullanılan)
npx ecc-agentshield scan --format json

# Otomatik düzeltme
npx ecc-agentshield scan --fix

# Opus derinlemesine analiz (Gerekli ANTHROPIC_API_KEY)
export ANTHROPIC_API_KEY=your-key
npx ecc-agentshield scan --opus --stream
```

### Önem Derecesi Seviyeleri

| Seviye | Puan | Anlam |
|------|------|------|
| A | 90-100 | Güvenlik yapılandırması |
| B | 75-89 | Küçük sorunlar |
| C | 60-74 | Dikkat Gerekli |
| D | 40-59 | Önemli risk |
| F | 0-39 | Kritik güvenlik açığı |

---

## Django Güvenliği

### Kullanım Açıklaması
Django Güvenlik En İyi Uygulamaları, yaygın güvenlik açıklarından koruma sağlar.

### Kullanım Zamanı
- Django kimlik doğrulama ve yetkilendirme ayarlarken
- Kullanıcı izinlerini ve Rolünü Gerçekleştirirken
- Üretim güvenlik ayarlarını yapılandırırken
- Django uygulamalarının Güvenlik sorunlarını incelerken
- Django uygulamalarını üretim ortamına yayınlarken

### Çekirdek Güvenlik Ayarları

```python
# settings/production.py
DEBUG = False  # Kritik: Üretimde asla True kullanmayın

ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS', '').split(',')

# Güvenlik başlıkları
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
SECURE_HSTS_SECONDS = 31536000  # 1 yıl
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_BROWSER_XSS_FILTER = True
X_FRAME_OPTIONS = 'DENY'

# Çerez Güvenliği
SESSION_COOKIE_HTTPONLY = True
CSRF_COOKIE_HTTPONLY = True
SESSION_COOKIE_SAMESITE = 'Lax'

# Şifre doğrulama
AUTH_PASSWORD_VALIDATORS = [
    {'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator', 'OPTIONS': {'min_length': 12}},
    {'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator'},
]
```

### SQL Enjeksiyon Koruması

```python
# Güvenlik: Django ORM otomatik olarak parametreleri çevreler
def get_user(username):
    return User.objects.get(username=username)  # Güvenlik

# Güvenlik: Parametreli raw() kullanımı
def search_users(query):
    return User.objects.raw('SELECT * FROM users WHERE username = %s', [query])

# Tehlikeli: Asla doğrudan enterpolasyon kullanmayın
def get_user_bad(username):
    return User.objects.raw(f'SELECT * FROM users WHERE username = {username}')  # Açık!
```

### XSS Koruması

```django
{# Django değişkenleri otomatik olarak çevreler - Güvenlik #}
{{ user_input }}  {# HTML çevrelenecek #}

{# Güvenli yalnızca güvenilir içerik için işaretle #}
{{ trusted_html|safe }}  {# Çevrelemez #}

{# Şablon filtresiyle HTML'yi Güvenli bir şekilde işle #}
{{ user_input|striptags }}  {# Tüm HTML etiketlerini kaldırır #}
```

### CSRF Koruması

```django
{# Şablonda kullan #}
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Gönder</button>
</form>

{# AJAX isteği #}
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

### Hız Sınırlama

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

### Hızlı Güvenlik Kontrol Listesi

| Kontrol | Tanım |
|--------|------|
| `DEBUG = False` | Üretimde DEBUG ile asla çalıştırmayın |
| Yalnızca HTTPS | SSL zorla, Güvenlik çerezi |
| Güçlü anahtar | Ortam değişkenlerinden SECRET_KEY kullanın |
| Şifre doğrulama | Tüm şifre doğrulayıcıları etkinleştirin |
| CSRF koruması | Varsayılan olarak etkin, devre dışı bırakmayın |
| XSS koruması | Django varsayılan olarak çevreler, kullanıcı girdisi için `\|safe` kullanmayın |
| SQL enjeksiyon | ORM kullanın, sorgularda asla dize birleştirmeyin |
| Dosya yükleme | Dosya türünü ve boyutunu doğrulayın |
| Hız sınırlama | API uç noktalarını sınırlayın |
| Güvenlik başlıkları | CSP, X-Frame-Options, HSTS |
| Günlükleme | Güvenlik olaylarını günlüğe kaydedin |
| Güncelleme | Django ve bağımlılıkları güncel tutun |

---

## İlgili Beceriler

| Beceri | Kullanım |
|------|------|
| `security-review` | Kapsamlı Güvenlik kontrol listesi |
| `security-scan` | Claude Code yapılandırma taraması |
| `django-security` | Django Güvenlik En İyi Uygulamaları |
| `coding-standards` | Kodlama standartları ve Güvenlik temelleri |
