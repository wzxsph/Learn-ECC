# Özel Alan Becerileri

Bu bölüm belirli alan uzmanlık becerilerini kapsar, İçerir blockchain, oyun geliştirme, ses ve video işleme vb.

---

## Blockchain ve Web3

### solidity

**Kullanım**: Akıllı sözleşme geliştirme

**Temel Kavramlar**:
- Sözleşme yapısı
- Veri türleri
- Fonksiyon değiştiricileri
- Olaylar ve günlükler
- Kalıtım ve kütüphaneler

**Örnek**:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Token {
    mapping(address => uint256) public balances;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    
    function transfer(address to, uint256 amount) public {
        require(balanceOf(msg.sender) >= amount);
        balances[msg.sender] -= amount;
        balances[to] += amount;
        emit Transfer(msg.sender, to, amount);
    }
}
```

### defi-amm-security

**Kullanım**: DeFi AMM Güvenlik

**Temel Kavramlar**:
- Kalıcı kayıp
- Flash loan saldırıları
- Oracle manipülasyonu
- Sözleşme denetimi

### ethereum-development

**Kullanım**: Ethereum geliştirme

**Araçlar**:
- Hardhat
- Foundry
- ethers.js
- web3.js

---

## Oyun Geliştirme

### unity

**Kullanım**: Unity oyun geliştirme

**Temel Kavramlar**:
- Sahne yönetimi
- Fizik motoru
- Parçacık sistemleri
- Animasyon durum makineleri
- UI sistemi

### godot

**Kullanım**: Godot oyun motoru

**Özellikler**:
- GDScript
- Komut dosyası sistemi
- Sinyal mekanizması
- Çapraz platform dışa aktarma

### unreal-engine

**Kullanım**: Unreal Engine geliştirme

**Temel Kavramlar**:
- Blueprint
- C++ programlama
- Materyal sistemi
- Animasyon blueprintleri

### game-ai

**Kullanım**: Oyun AI

**Teknoloji**:
- Durum makineleri
- Davranış ağaçları
- Yol planlama
- Karar ağaçları

---

## Ses ve Video İşleme

### video-editing

**Kullanım**: Video düzenleme

**Araçlar**:
- FFmpeg
- DaVinci Resolve
- Adobe Premiere

### audio-processing

**Kullanım**: Ses işleme

**Teknoloji**:
- Ses kodlama
- Gürültü azaltma
- Miksaj

### streaming

**Kullanım**: Medya akışı işleme

**Protokoller**:
- HLS
- DASH
- RTMP

### media-processing

**Kullanım**: Medya işleme boru hattı

**Araçlar**:
- GStreamer
- MediaConvert
- Transmux

---

## Nesnelerin İnterneti

### bluetooth-low-energy

**Kullanım**: BLE geliştirme

**Temel Kavramlar**:
- GATT protokolü
- Yayın ve tarama
- Bağlantı yönetimi
- Güç tüketimi optimizasyonu

### iot-development

**Kullanım**: IoT geliştirme

**Platform**:
- Arduino
- Raspberry Pi
- ESP32

### embedded-systems

**Kullanım**: Gömülü sistemler

**Beceriler**:
- C/C++ programlama
- Bare metal geliştirme
- RTOS
- Sürücü geliştirme

---

## Veritabanı ve Depolama

### sql-injection

**Kullanım**: SQL Enjeksiyonu savunması

**Savunma önlemleri**:
- Parametreli sorgular
- ORM kullanımı
- Girdi doğrulama
- En az yetki prensibi

### postgresql

**Kullanım**: PostgreSQL En İyi Uygulamalar

**Temel Kavramlar**:
- İndeks stratejileri
- Sorgu optimizasyonu
- Bölümlendirilmiş tablolar
- JSONB işlemleri
- Çoğaltma yapılandırması

### sqlite

**Kullanım**: SQLite kullanımı

**Özellikler**:
- Hafif
- Sunsuz
- ACID uyumlu
- Mobil uygulamalar

### snowflake

**Kullanım**: Snowflake veri ambarı

**Çekirdek işlevler**:
- Veri paylaşımı
- Zaman yolculuğu
- Klonlama
- Genişletilmiş mikro bölümleme

---

## Mesaj Kuyrukları

### kafka

**Kullanım**: Apache Kafka

**Temel Kavramlar**:
- Konu ve Bölüm
- Üretici ve tüketici
- Tüketici grupları
- Çoğaltma ve tutarlılık
- Akış işleme

**Örnek**:
```java
// Üretici
KafkaProducer<String, String> producer = new KafkaProducer<>(props);
ProducerRecord<String, String> record = new ProducerRecord<>("topic", "key", "value");
producer.send(record);

// Tüketici
KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(List.of("topic"));
while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : records) {
        // Mesajları işle
    }
}
```

### nats

**Kullanım**: NATS mesaj sistemi

**Özellikler**:
- Hafif
- Düşük gecikme
- Birden fazla modu Destekle (pub/sub, istek/yanıt)

### mqtt

**Kullanım**: MQTT nesnelerin interneti protokolü

**QoS seviyeleri**:
- QoS 0: En fazla bir kez
- QoS 1: En az bir kez
- QoS 2: Tam olarak bir kez

---

## Ön Uygulama Belirli Alanları

### pwa

**Kullanım**: Progressive Web App

**Çekirdek işlevler**:
- Service Worker
- Web App Manifest
- Çevrimdışı Destekle
- Push bildirimleri

### webgl

**Kullanım**: WebGL grafik programlama

**Uygulamalar**:
- 3D render
- Oyunlar
- Veri görselleştirme

### webrtc

**Kullanım**: WebRTC gerçek zamanlı iletişim

**Çekirdek API**:
- getUserMedia
- RTCPeerConnection
- RTCDataChannel

---

## Dokümantasyon ve Yazma

### markdown

**Kullanım**: Markdown yazma

**Uzantılar**:
- GFM (GitHub Flavored Markdown)
- Tablolar
- Kod bloğu vurgulama
- Görev listeleri

### latex

**Kullanım**: LaTeX dokümantasyon düzenleme

**Kullanım Alanları**:
- Akademik makaleler
- Matematik formülleri
- Teknik dokümantasyon

### plantuml

**Kullanım**: PlantUML diyagramları

**Diyagram türleri**:
- Sınıf diyagramları
- Sıra diyagramları
- Kullanım durumu diyagramları
- Etkinlik diyagramları

---

## Özel Araçlar

### vim

**Kullanım**: Vim düzenleyici

**Çekirdek beceriler**:
- Mod değiştirme
- Hareket komutları
- Metin nesneleri
- Makrolar ve kayıt defterleri
- Eklenti yönetimi

### shell-scripting

**Kullanım**: Shell betik yazma

**Betik türleri**:
- Bash
- Zsh
- POSIX

### latex

**Kullanım**: LaTeX düzenleme

**Kullanım Alanları**:
- Akademik makaleler
- Matematik formülleri
- Karmaşık dokümantasyon

---

## İlgili Beceriler

| Beceri | Kullanım |
|------|------|
| `solidity` | Akıllı sözleşme geliştirme |
| `unity` | Unity oyun geliştirme |
| `video-editing` | Video düzenleme |
| `kafka` | Kafka mesaj kuyruğu |
| `pwa` | PWA geliştirme |
| `webgl` | WebGL grafik |