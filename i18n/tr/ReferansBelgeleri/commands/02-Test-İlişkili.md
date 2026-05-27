# Test İlişkili Komutlar

Bu doküman, ECC'de kullanılan çeşitli programlama dilleri ve test çerçevelerini destekleyen test komutlarını tanıtır.

---

## /go-test

**Kullanım Açıklaması**: Go dili için TDD iş akışı. Tablo güdümlü testler kullanır, önce test yazar, sonra uygular, %80+ kapsamı doğrular.

**Kullanım Yöntemi**:
```
/go-test [özellik tanımı]
```

**Kullanım Senaryosu**:
- Yeni Go fonksiyonları uygularken
- Mevcut Go koduna test kapsamı eklerken
- Hata düzeltirken (önce başarısız test yazarak)
- Go TDD metodolojisini öğrenirken

**Örnek İş Akışı**:
```go
// Step 1: Arayüzü tanımla
func ValidateEmail(email string) error {
    panic("not implemented")
}

// Step 2: Tablo güdümlü test yaz (KIRMIZI)
tests := []struct {
    name    string
    email   string
    wantErr bool
}{
    {"simple email", "user@example.com", false},
    {"empty string", "", true},
    {"no at sign", "userexample.com", true},
}

// Step 3: Testi çalıştır - başarısız olduğunu doğrula

// Step 4: Minimum kodu uygula (YEŞİL)
func ValidateEmail(email string) error {
    if email == "" {
        return ErrEmailEmpty
    }
    if !emailRegex.MatchString(email) {
        return ErrEmailInvalid
    }
    return nil
}

// Step 5: Testin geçtiğini doğrula

// Step 6: Kapsamı kontrol et
go test -cover ./...
// coverage: 100.0%
```

**Kapsam Komutları**:
```bash
go test -cover ./...                    # Temel kapsam
go test -coverprofile=coverage.out      # Kapsam dosyası oluştur
go tool cover -html=coverage.out        # Tarayıcıda görüntüle
go test -race -cover ./...              # Yarış tespiti
```

---

## /kotlin-test

**Kullanım Açıklaması**: Kotlin dili için TDD iş akışı. Kotest çerçevesi ve MockK kullanır, önce test yazar, sonra uygular, %80+ kapsamı doğrular (Kover).

**Kullanım Yöntemi**:
```
/kotlin-test [özellik tanımı]
```

**Kullanım Senaryosu**:
- Yeni Kotlin fonksiyonları veya sınıfları uygularken
- Android/KMP projelerine test kapsamı eklerken
- Hata düzeltirken (önce başarısız test yazarak)
- Kotlin TDD metodolojisini öğrenirken

**Kotest Test Stilleri**:
```kotlin
// StringSpec (en basit)
class CalculatorTest : StringSpec({
    "add two positive numbers" {
        Calculator.add(2, 3) shouldBe 5
    }
})

// FunSpec (standart birim testi)
class RegistrationValidatorTest : FunSpec({
    test("valid registration returns Valid") {
        val request = RegistrationRequest(
            name = "Alice",
            email = "alice@example.com",
            password = "SecureP@ss1"
        )
        val result = validateRegistration(request)
        result.shouldBeInstanceOf<ValidationResult.Valid>()
    }
})

// BehaviorSpec (BDD stili)
class OrderServiceTest : BehaviorSpec({
    Given("a valid order") {
        When("placed") {
            Then("should be confirmed") { /* ... */ }
        }
    }
})
```

**Korutin Testleri**:
```kotlin
test("concurrent fetch completes") {
    runTest {
        val result = service.fetchAll()
        result.shouldNotBeEmpty()
    }
}
```

**Kapsam Komutları**:
```bash
./gradlew koverHtmlReport     # HTML raporu
./gradlew koverVerify         # Kapsam eşiklerini doğrula
./gradlew koverXmlReport      # CI için XML raporu
./gradlew test                # Testleri çalıştır
```

---

## /rust-test

**Kullanım Açıklaması**: Rust dili için TDD iş akışı. `#[test]`, rstest, proptest ve mockall kullanır, önce test yazar, sonra uygular, %80+ kapsamı doğrular (cargo-llvm-cov).

**Kullanım Yöntemi**:
```
/rust-test [özellik tanımı]
```

**Kullanım Senaryosu**:
- Yeni Rust fonksiyonları, metodları veya traitleri uygularken
- Rust projelerine test kapsamı eklerken
- Hata düzeltirken (önce başarısız test yazarak)
- Rust TDD metodolojisini öğrenirken

**Test Desenleri**:
```rust
// Birim testleri
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn adds_two_numbers() {
        assert_eq!(add(2, 3), 5);
    }

    #[test]
    fn handles_error() -> Result<(), Box<dyn std::error::Error>> {
        let result = parse_config(r#"port = 8080"#)?;
        assert_eq!(result.port, 8080);
        Ok(())
    }
}

// Parametreli testler (rstest)
#[rstest]
#[case("hello", 5)]
#[case("", 0)]
#[case("rust", 4)]
fn test_string_length(#[case] input: &str, #[case] expected: usize) {
    assert_eq!(input.len(), expected);
}

// Asenkron testler
#[tokio::test]
async fn fetches_data_successfully() {
    let client = TestClient::new().await;
    let result = client.get("/data").await;
    assert!(result.is_ok());
}

// Özellik testleri (proptest)
proptest! {
    #[test]
    fn encode_decode_roundtrip(input in ".*") {
        let encoded = encode(&input);
        let decoded = decode(&encoded).unwrap();
        assert_eq!(input, decoded);
    }
}
```

**Kapsam Komutları**:
```bash
cargo llvm-cov                    # Kapsam özeti
cargo llvm-cov --html             # HTML raporu
cargo llvm-cov --fail-under-lines 80  # Eşik altında ise başarısız
cargo test                        # Testleri çalıştır
```

---

## /cpp-test

**Kullanım Açıklaması**: C++ dili için TDD iş akışı. GoogleTest/GoogleMock ve CMake/CTest kullanır, önce test yazar, sonra uygular, kapsamı doğrular (gcov/lcov).

**Kullanım Yöntemi**:
```
/cpp-test [özellik tanımı]
```

**Kullanım Senaryosu**:
- Yeni C++ sınıfları veya fonksiyonları uygularken
- Test kapsamı eklerken
- Hata düzeltirken (önce başarısız test yazarak)
- C++ TDD metodolojisini öğrenirken

**GoogleTest Desenleri**:
```cpp
// Temel testler
TEST(SuiteName, TestName) {
    EXPECT_EQ(add(2, 3), 5);
    EXPECT_NE(result, nullptr);
    EXPECT_TRUE(is_valid);
    EXPECT_THROW(func(), std::invalid_argument);
}

// Fixture
class DatabaseTest : public ::testing::Test {
protected:
    void SetUp() override { db_ = create_test_db(); }
    void TearDown() override { db_.reset(); }
    std::unique_ptr<Database> db_;
};

TEST_F(DatabaseTest, InsertsRecord) {
    db_->insert("key", "value");
    EXPECT_EQ(db_->get("key"), "value");
}

// Parametreli testler
class PrimeTest : public ::testing::TestWithParam<std::pair<int, bool>> {};

TEST_P(PrimeTest, ChecksPrimality) {
    auto [input, expected] = GetParam();
    EXPECT_EQ(is_prime(input), expected);
}

INSTANTIATE_TEST_SUITE_P(Primes, PrimeTest,
    ::testing::Values(
        std::make_pair(2, true),
        std::make_pair(4, false),
        std::make_pair(7, true)
    ));
```

**Kapsam Komutları**:
```bash
cmake -DCMAKE_CXX_FLAGS="--coverage" -B build
cmake --build build
ctest --test-dir build
lcov --capture --directory build --output-file coverage.info
genhtml coverage.info --output-directory coverage_html
```

---

## /flutter-test

**Kullanım Açıklaması**: Flutter/Dart testi. Başarısızlıkları raporlar ve test sorunlarını adım adım düzeltir, birim, bileşen, altın ve entegrasyon testlerini kapsar.

**Kullanım Yöntemi**:
```
/flutter-test                # Tüm testleri çalıştır
/flutter-test --coverage     # Kapsamla birlikte
/flutter-test test/file.dart # Belirli test dosyasını çalıştır
```

**Kullanım Senaryosu**:
- Uygulama sonrası hiçbir şeyin bozulmadığını doğrulama
- Yeni kodun test kapsamını kontrol etme
- Belirli test dosyalarını düzeltme
- PR işlemeden önce

**Yaygın Test Başarısızlıkları ve Düzeltmeleri**:

| Başarısızlık Türü | Tipik Düzeltme |
|----------|----------|
| `Expected: <X> Actual: <Y>` | Assert'i güncelle veya uygulamayı düzelt |
| `Widget not found` | Finder seçicisini veya widget adını düzelt |
| `Golden file not found` | Altınları oluşturmak için `flutter test --update-goldens` çalıştır |
| `MissingPluginException` | Test kurulumunda platform kanallarını mockla |
| `pumpAndSettle timed out` | Açık `pump(Duration)` ile değiştir |

---

## /e2e

**Kullanım Açıklaması**: Uçtan uca test iş akışı. Kritik kullanıcı akışlarının E2E testlerini oluşturur ve çalıştırır.

**Kullanım Yöntemi**:
```
/e2e [özellik tanımı]
```

**Kullanım Senaryosu**:
- Kritik kullanıcı akışlarını test etme
- Çapraz sistem entegrasyon testleri
- Kabul testleri
- Regresyon testleri

---

## /test-coverage

**Kullanım Açıklaması**: Test kapsamını analiz eder, boşlukları tanımlar ve hedef eşiğe ulaşmak için eksik testleri oluşturur (varsayılan %80).

**Kullanım Yöntemi**:
```
/test-coverage
```

**İş Akışı**:
1. **Test çerçevelerini algıla** - Proje türüne göre kapsam komutunu seç
2. **Kapsam raporunu analiz et** - %80'in altındaki dosyaları listele
3. **Eksik testleri oluştur** - Önceliğe göre testler oluştur
4. **Doğrula** - İyileştirmeleri onaylamak için kapsamı yeniden çalıştır

**Test Çerçevesi Algılama**:

| Gösterge | Kapsam Komutu |
|--------|----------|
| `jest.config.*` | `npx jest --coverage` |
| `vitest.config.*` | `npx vitest run --coverage` |
| `pytest.ini` | `pytest --cov=src --cov-report=json` |
| `Cargo.toml` | `cargo llvm-cov --json` |
| `go.mod` | `go test -coverprofile=coverage.out` |

---

## /python-testing

**Kullanım Açıklaması**: Python test iş akışı. pytest kullanır, kapsam analizini destekler.

**Kullanım Yöntemi**:
```
/python-testing [özellik tanımı]
```

**Kullanım Senaryosu**:
- Yeni Python özellikleri uygularken
- Test kapsamı eklerken
- Hata düzeltirken
- FastAPI/Django/Flask proje testleri

**Pytest Özellikleri**:
```python
# Parametreli testler
@pytest.mark.parametrize("input,expected", [
    ("hello", 5),
    ("", 0),
    ("world", 5),
])
def test_string_length(input, expected):
    assert len(input) == expected

# Fixture'ler
@pytest.fixture
def client():
    with TestClient(app) as c:
        yield c

# Asenkron testler
@pytest.mark.asyncio
async def test_async_fetch():
    result = await fetch_data()
    assert result is not None
```

---

## Komut Karşılaştırma Tablosu

| Komut | Dil | Test Çerçeveleri | Kapsam Araçları | TDD Döngüsü |
|------|------|----------|------------|----------|
| `/go-test` | Go | Yerleşik testing | go test -cover | ✓ |
| `/kotlin-test` | Kotlin | Kotest + MockK | Kover | ✓ |
| `/rust-test` | Rust | #[test], rstest, proptest | cargo-llvm-cov | ✓ |
| `/cpp-test` | C++ | GoogleTest | gcov/lcov | ✓ |
| `/flutter-test` | Dart | Flutter test | coverage | - |
| `/python-testing` | Python | pytest | pytest-cov | - |
| `/e2e` | Çoklu | Proje özgü | - | - |
| `/test-coverage` | Genel | Çoklu | Çoklu | - |
