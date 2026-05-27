# Performans Optimizasyonu

## Model Seçim Stratejisi

**Haiku 4.5** (Sonnet'un %90 yeteneği, 1/3 maliyeti):
- Sık çağrılan hafif aracılar
- Eşli programlama ve kod üretimi
- Çoklu aracı sistemlerinde worker aracıları

**Sonnet 4.6** (En iyi kodlama modeli):
- Ana geliştirme işleri
- Çoklu aracı iş akışlarını orkestre etme
- Karmaşık kodlama görevleri

**Opus 4.5** (En derin muhakeme):
- Karmaşık mimari kararlar
- Maksimum muhakeme gereksinimleri
- Araştırma ve analiz görevleri

## Bağlam Penceresi Yönetimi

Bağlam penceresinin son %20'sinden kaçının:
- Büyük ölçekli yeniden yapılandırma
- Birden fazla dosyayı kapsayan özellik gerçekleştirimi
- Karmaşık etkileşimleri hata ayıklama

Düşük bağlam duyarlılığı gerektiren görevler:
- Tek dosya düzenlemeleri
- Bağımsız araç oluşturma
- Dokümantasyon güncellemeleri
- Basit hata düzeltmeleri

## Genişletilmiş Düşünce + Plan Modu

Genişletilmiş düşünce varsayılan olarak etkindir, dahili muhakeme için 31.999 tokene kadar ayrılmıştır.

Genişletilmiş düşünceyi kontrol etme:
- **Geçiş**: Option+T (macOS) / Alt+T (Windows/Linux)
- **Yapılandırma**: `~/.claude/settings.json` içinde `alwaysThinkingEnabled` ayarla
- **Bütçe üst limiti**: `export MAX_THINKING_TOKENS=10000`
- **Ayrıntılı mod**: Düşünce çıktısını görmek için Ctrl+O

Derin muhakeme gerektiren karmaşık görevler için:
1. Genişletilmiş düşüncenin etkin olduğundan emin ol (varsayılan)
2. Yapılandırılmış yaklaşım için**plan modunu** etkinleştir
3. Kapsamlı analiz için çoklu eleştiri turları kullan
4. Farklı perspektifler için rol bölünmüş alt aracılar kullan

## Yapı Hata Giderme

Yapı başarısız olursa:
1. **build-error-resolver** aracısını kullan
2. Hata mesajlarını analiz et
3. Kademeli olarak düzelt
4. Her düzeltmeden sonra doğrula
