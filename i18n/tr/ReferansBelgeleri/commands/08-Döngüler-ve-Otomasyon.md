# Döngü ve Otomasyon Komutları

## Genel Bakış

Döngü ve otomasyon komutları, otonom döngü çalışma modlarını başlatmak ve yönetmek için kullanılır.

## Komut Listesi

### /loop-start

**Kullanım**: Güvenlik varsayılan ayarları ve net durdurma koşullarıyla yönetilen otonom döngü modunu başlatır

**Tanım**: Güvenlik varsayılan ayarları ve net durdurma koşullarıyla yönetilen otonom döngü modunu başlatır.

**Kullanım**: `/loop-start [pattern] [--mode safe|fast]`

**Parametreler**:

| Parametre | Değer | Açıklama |
|---|---|---|
| `pattern` | `sequential` | Görevleri ardışık çalıştır, biri bittikten sonra diğeri |
| | `continuous-pr` | Sürekli PR oluşturma ve birleştirme döngüsü |
| | `rfc-dag` | RFC tabanlı yönlendirilmiş çevrimsiz grafik iş akışı |
| | `infinite` | Sonsuz döngü (net durdurma koşulları gerektirir) |
| `--mode` | `safe` (varsayılan) | Katı kalite kapıları ve kontrol noktaları |
| | `fast` | Hız için azaltılmış kapılar |

**İş Akışı**:
1. Depo durumunu ve dal stratejisini doğrula
2. Döngü modunu ve ilgili model seviyesi stratejisini seç
3. Seçilen mod için gerekli hook/profili etkinleştir
4. Döngü planı oluştur ve `.claude/plans/` altına runbook yaz
5. Başlatma ve izleme komutlarını yazdır

**Zorunlu Güvenlik Kontrolleri**:
- İlk döngüden önce testlerin geçtiğini doğrula
- `ECC_HOOK_PROFILE`'in globally devre dışı olmadığından emin ol
- Döngünün net durdurma koşulları olduğundan emin ol

**Parametre Geçirme**:
- `$ARGUMENTS`: `[pattern]` ve isteğe bağlı `[--mode safe|fast]`
- Mod isteğe bağlı (varsayılan: sequential)
- Mode bayrağı isteğe bağlı (varsayılan: safe)

**En İyi Uygulamalar**:
- Infinite moddan önce net durdurma koşulları belirle
- Fast modu yalnızca yeterince test edilmiş kod üzerinde kullan
- Döngü ilerlemesini izle, durumu kontrol etmek için `/loop-status` kullan

**Yaygın Tuzaklar**:
- Testler doğrulanmadan döngü başlatma
- Net durdurma koşulları olmadan infinite mod kullanma
- Güvenlik modu kullanırken hookları global olarak devre dışı bırakma

**Diğer Komutlarla Entegrasyon**:
- Çalışan döngüleri izlemek için `/loop-status` kullan
- Santa tarzı döngü için `/santa-loop` kullan
- Kritik düğümlerde kontrol noktası oluşturmak için `/checkpoint` kullan

---

### /loop-status

**Kullanım**: Çalışan döngünün durumunu kontrol et

**Tanım**: Mevcut çalışan döngünün durumunu ve ilerlemesini görüntüler.

---

### /santa-loop

**Kullanım**: Santa tarzı otonom döngü

**Tanım**: Özel bir tarzda otonom döngü modu.

---

## İlgili Komutlar

- `/loop-start` - Döngü başlat
- `/loop-status` - Durumu kontrol et
- `/santa-loop` - Santa tarzı
