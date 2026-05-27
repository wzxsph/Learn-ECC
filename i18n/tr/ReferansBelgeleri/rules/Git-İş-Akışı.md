# Git İş Akışı

## Commit Mesaj Formatı

```
<type>: <açıklama>

<opsiyonel gövde>
```

**Tür (type)**:
- `feat`: Yeni özellik
- `fix`: Hata düzeltme
- `refactor`: Yeniden yapılandırma
- `docs`: Dokümantasyon
- `test`: Test
- `chore`: Çeşitli işler
- `perf`: Performans optimizasyonu
- `ci`: CI/CD

> Not: Atıf (attribution) `~/.claude/settings.json` üzerinden global olarak devre dışı bırakılmıştır

## Pull Request İş Akışı

PR oluştururken:
1. Tam commit geçmişini analiz edin (sadece en son commit değil)
2. Tüm değişiklikleri görmek için `git diff [base-branch]...HEAD` kullanın
3. Kapsamlı bir PR özeti taslağı oluşturun
4. Test planı ile TODOs ekleyin
5. Yeni dal ise `-u` bayrağıyla push yapın

## Dal Adlandırma

Standart git flow dal adlandırmasını kullanın:
- `feature/`: Yeni özellik
- `fix/`: Düzeltme
- `refactor/`: Yeniden yapılandırma
- `docs/`: Dokümantasyon

## Geliştirme Süreci

Tam geliştirme süreci:
1. **Araştırma & Yeniden Kullanım** - GitHub araması, kütüphane dokümantasyonu, paket kayıt defterleri
2. **Planlama** - Uygulama planı oluşturmak için planner aracısını kullanın
3. **TDD Yaklaşımı** - Önce test yaz, sonra gerçekleştir
4. **Kod İnceleme** - code-reviewer aracısını kullan
5. **Commit & Push** - Ayrıntılı commit mesajları, conventional commits'e uygun
