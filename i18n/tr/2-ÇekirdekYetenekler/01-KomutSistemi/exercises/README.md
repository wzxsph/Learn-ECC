# Komut Sistemi Alistirmalari

## Alistirma Hedefleri

Claude Code komut sistemini olusturma, yapilandirma ve hata ayiklama becerilerini kazanin.

## Alistirma Icerigi

### Alistirma 1: Temel Komut Olusturma
`/mycommand` komutunu olusturun ve su islevleri gerceklestirin:
- Mevcut zamani ve proje yolunu ciktisin
- `--verbose` secenegini desteklesin

### Alistirma 2: Parametreli Komut Gerceklestirme
`/search` komutunu olusturun ve su islevleri gerceklestirin:
- Arama anahtar kelimesini kabul etsin
- `--limit` parametresiyle sonuc sayisini kontrol etsin
- Formatlanmis cikti gosterisin

### Alistirma 3: Komut Hata Ayiklama
Mevcut komutlari hata ayiklayin, hata yonetimi ve log ciktisi ekleyin.

## Sablon

```markdown
# Benim Komutum

## Amaci
Komutun ana islevini acikla.

## Kullanim Sekli
`/mycommand [secenekler] <parametreler>`

## Secenekler
- `--option`: Secenek aciklamasi

## Ornekler
`/mycommand --option value`
```

## Dogrulama
Alistirmalari tamamladiktan sonra, islevlerin dogru calisip calismadigini dogrulamak icin komutlari calistirin.