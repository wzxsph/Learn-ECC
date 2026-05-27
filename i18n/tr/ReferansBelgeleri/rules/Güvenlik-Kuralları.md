# Güvenlik Kuralları

## Kural Genel Bakış

ECC Rules Güvenlik Kuralları, kodun commit edilmeden önce en yüksek güvenlik standartlarına uymasını sağlamak için tasarlanmış zorunlu güvenlik kontrol sistemidir. Bu kural, secret yönetiminden MCP sunucu güvenliğine kadar her yönü kapsar ve tüm kod değişiklikleri için uyulması zorunlu olan taban çizgisini oluşturur.

## Temel Gereksinimler

### Commit Öncesi Zorunlu Kontrol Listesi

Herhangi bir kodu commit etmeden önce aşağıdaki güvenlik kontrollerinin tamamlanması zorunludur:

| Kontrol | Açıklama |
|--------|------|
| Sabit kodlanmış secret yok | API anahtarları, şifreler, tokenler vb. kaynak kodda sabit kodlanmamalı |
| Kullanıcı girdisi doğrulama | Tüm kullanıcı girdileri doğrulanmalı |
| SQL enjekisyon koruması | SQL enjekisyonunu önlemek için parametreli sorgular kullan |
| XSS koruması | Kullanıcı girdisi için HTML kaçış |
| CSRF koruması | CSRF korumasının etkin olduğundan emin ol |
| Kimlik doğrulama yetkilendirme doğrulama | Kimlik doğrulama ve yetkilendirme mantığının doğru olduğunu onayla |
| Hız sınırlama mekanizması | Tüm uç noktalarda hız sınırlama zorunlu |
| Hata mesajı güvenliği | Hata mesajları hassas veri sızdırmamalı |

## Secret Yönetimi

- Secret'leri asla kaynak kodda sabit kodlama
- Her zaman ortam değişkenleri veya secret yöneticisi kullan
- Başlangıçta gerekli secretların mevcut olduğunu doğrula
- Açığa çıkmış olabilecek secretları rotasyonla
