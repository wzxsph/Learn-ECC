# Hooks Sistemi

## Hook Türleri

- **PreToolUse**: Araç yürütülmeden önce (doğrulama, parametre değişikliği)
- **PostToolUse**: Araç yürütüldükten sonra (otomatik biçimlendirme, kontroller)
- **Stop**: Oturum sona erdiğinde (son doğrulama)

## Otomatik Kabul İzinleri

Dikkatli kullanın:
- Güvenilir, iyi tanımlanmış planlar için etkinleştir
- Keşifsel çalışmalar için devre dışı bırak
- dangerously-skip-permissions bayrağını asla kullanmayın
- Bunun yerine `~/.claude.json` içinde `allowedTools` yapılandırın

## TodoWrite En İyi Uygulamaları

TodoWrite aracını şu amaçlarla kullanın:
- Çok adımlı görevlerin ilerlemesini takip et
- Talimatları anlamayı doğrula
- Gerçek zamanlı yönlendirmeyi etkinleştir
- Granüler uygulama adımlarını göster

Todo listesi şunları ortaya çıkarır:
- Sırasız adımlar
- Eksik kalemeler
- Gereksiz fazla kalemeler
- Yanlış granül
- Yanlış anlaşılan gereksinimler
