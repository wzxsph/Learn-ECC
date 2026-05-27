# Aracı Orkestrasyonu

## Kullanılabilir Aracılar

`~/.claude/agents/` dizininde bulunur:

| Aracı | Amaç | Kullanım Zamanı |
|-------|------|----------|
| planner | Uygulama planlama | Karmaşık özellikler, yeniden yapılandırma |
| architect | Sistem tasarımı | Mimari kararlar |
| tdd-guide | Test güdümlü geliştirme | Yeni özellikler, hata düzeltmeleri |
| code-reviewer | Kod inceleme | Kod yazıldıktan sonra |
| security-reviewer | Güvenlik analizi | Commit öncesi |
| build-error-resolver | Yapı hatalarını onarma | Yapı başarısız olduğunda |
| e2e-runner | Uçtan uca test | Kritik kullanıcı akışları |
| refactor-cleaner | Ölü kod temizleme | Kod bakımı |
| doc-updater | Dokümantasyon güncelleme | Dokümantasyon güncellenirken |
| rust-reviewer | Rust kod inceleme | Rust projeleri |
| harmonyos-app-resolver | HarmonyOS uygulama geliştirme | HarmonyOS/ArkTS projeleri |

## Anında Aracı Kullanımı

Kullanıcı ipucu gerekmez:
1. Karmaşık özellik istekleri → **planner** aracısını kullan
2. Kod yazıldıktan/Değiştirildikten sonra → **code-reviewer** aracısını kullan
3. Hata düzeltme veya yeni özellik → **tdd-guide** aracısını kullan
4. Mimari karar → **architect** aracısını kullan

## Paralel Görev Yürütme

Bağımsız işlemler için her zaman paralel görev yürütme kullanın:

```markdown
# İYİ: Paralel Yürütme
3 aracıyı paralel başlat:
1. Aracı 1: Auth modülü güvenlik analizi
2. Aracı 2: Önbellek sistemi performans incelemesi
3. Aracı 3: Araç türü kontrolü

# KÖTÜ: Gereksiz sıralı yürütme
Önce aracı 1, sonra aracı 2, sonra aracı 3
```

## Çok Perspektifli Analiz

Karmaşık problemler için rol bölünmüş alt aracılar kullanın:
- Gerçeklik inceleyicisi
- Kıdemli mühendis
- Güvenlik uzmanı
- Tutarlılık inceleyicisi
- Tekrarlama kontrolörü
