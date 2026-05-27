# Hookify Sistem Komutları

## Genel Bakış

Hookify sistemi, istenmeyen davranışları önlemek ve otomatik iş akışları için hook oluşturmak ve yönetmek için kullanılır.

## Komut Listesi

### /hookify

**Kullanım**: İstenmeyen davranışları önlemek için hook oluştur

**Tanım**: İstenmeyen davranışları önlemek için özel hook oluşturur.

**Kullanım Senaryosu**:
- İstem dışı silme işlemlerini önlemek
- Tehlikeli komutları engellemek
- Otomatik biçimlendirme
- Kalite kontrolleri

**Hook Türleri**:
- **PreToolUse**: Araç yürütülmeden önce
- **PostToolUse**: Araç yürütüldükten sonra
- **Stop**: Oturum sona erdiğinde

---

### /hookify-list

**Kullanım**: Tüm yapılandırılmış hookify kurallarını listele

**Tanım**: Mevcut tüm yapılandırılmış hookify kurallarını görüntüler.

---

### /hookify-configure

**Kullanım**: Hookify kurallarının etkinleştirme/devre dışı bırakma interaktif yapılandırması

**Tanım**: Hookify kurallarının etkinleştirme ve devre dışı bırakma durumlarını interaktif olarak yapılandırır.

---

### /hookify-help

**Kullanım**: Hookify sistemi yardımı

**Tanım**: Hookify sistemi hakkında detaylı yardım bilgisi alır.

---

## Hook Yapılandırma Örneği

```json
{
  "matcher": { "pattern": "Bash|Edit|Write" },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Running quality check'"
    }
  ]
}
```

---

## İlgili Komutlar

- `/hookify` - Hook oluştur
- `/hookify-list` - Kuralları listele
- `/hookify-configure` - Kuralları yapılandır
- `/hookify-help` - Yardım al
