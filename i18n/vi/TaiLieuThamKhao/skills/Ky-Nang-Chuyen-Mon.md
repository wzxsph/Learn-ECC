# Kỹ Năng Chuyên Ngành

Phần này cover các chuyên ngành đặc thù, bao gồm blockchain, game development, audio/video processing, etc.

---

## Blockchain và Web3

### solidity

**Mục đích**: Solidity smart contract development

**Core concept**:
- Contract structure
- Data type
- Function modifier
- Event và log
- Inheritance và library

**Example**:
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

**Mục đích**: DeFi AMM security

**Core concept**:
- Impermanent loss
- Flash loan attack
- Oracle manipulation
- Contract audit

### ethereum-development

**Mục đích**: Ethereum development

**Tool**:
- Hardhat
- Foundry
- ethers.js
- web3.js

---

## Game Development

### unity

**Mục đích**: Unity game development

**Core concept**:
- Scene management
- Physics engine
- Particle system
- Animation state machine
- UI system

### godot

**Mục đích**: Godot game engine

**Feature**:
- GDScript
- Scene system
- Signal mechanism
- Cross-platform export

### unreal-engine

**Mục đích**: Unreal Engine development

**Core concept**:
- Blueprint
- C++ programming
- Material system
- Animation blueprint

### game-ai

**Mục đích**: Game AI

**Technology**:
- State machine
- Behavior tree
- Path planning
- Decision tree

---

## Audio/Video Processing

### video-editing

**Mục đích**: Video editing

**Tool**:
- FFmpeg
- DaVinci Resolve
- Adobe Premiere

### audio-processing

**Mục đích**: Audio processing

**Technology**:
- Audio encoding
- Noise reduction
- Mixing

### streaming

**Mục đích**: Streaming processing

**Protocol**:
- HLS
- DASH
- RTMP

### media-processing

**Mục đích**: Media processing pipeline

**Tool**:
- GStreamer
- MediaConvert
- Transmux

---

## IoT

### bluetooth-low-energy

**Mục đích**: BLE development

**Core concept**:
- GATT protocol
- Broadcast và scan
- Connection management
- Power consumption optimization

### iot-development

**Mục đích**: IoT development

**Platform**:
- Arduino
- Raspberry Pi
- ESP32

### embedded-systems

**Mục đích**: Embedded system

**Skill**:
- C/C++ programming
- Bare metal development
- RTOS
- Driver development

---

## Database và Storage

### sql-injection

**Mục đích**: SQL injection defense

**Defense measure**:
- Parameterized query
- ORM usage
- Input validation
- Least privilege principle

### postgresql

**Mục đích**: PostgreSQL best practice

**Core concept**:
- Index strategy
- Query optimization
- Partitioned table
- JSONB operation
- Replication configuration

### sqlite

**Mục đích**: SQLite usage

**Feature**:
- Lightweight
- Serverless
- ACID compatible
- Mobile application

### snowflake

**Mục đích**: Snowflake data warehouse

**Core function**:
- Data sharing
- Time travel
- Clone
- Extended micro-partition

---

## Message Queue

### kafka

**Mục đích**: Apache Kafka

**Core concept**:
- Topic và Partition
- Producer và consumer
- Consumer group
- Replication và consistency
- Stream processing

**Example**:
```java
// Producer
KafkaProducer<String, String> producer = new KafkaProducer<>(props);
ProducerRecord<String, String> record = new ProducerRecord<>("topic", "key", "value");
producer.send(record);

// Consumer
KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(List.of("topic"));
while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : records) {
        // Process message
    }
}
```

### nats

**Mục đích**: NATS messaging system

**Feature**:
- Lightweight
- Low latency
- Support multiple pattern (pub/sub, request/response)

### mqtt

**Mục đích**: MQTT IoT protocol

**QoS level**:
- QoS 0: At most once
- QoS 1: At least once
- QoS 2: Exactly once

---

## Frontend Specific Domain

### pwa

**Mục đích**: Progressive Web App

**Core function**:
- Service Worker
- Web App Manifest
- Offline support
- Push notification

### webgl

**Mục đích**: WebGL graphics programming

**Application**:
- 3D rendering
- Game
- Data visualization

### webrtc

**Mục đích**: WebRTC real-time communication

**Core API**:
- getUserMedia
- RTCPeerConnection
- RTCDataChannel

---

## Documentation và Writing

### markdown

**Mục đích**: Markdown writing

**Extension**:
- GFM (GitHub Flavored Markdown)
- Table
- Code block highlight
- Task list

### latex

**Mục đích**: LaTeX document typesetting

**Usage**:
- Academic paper
- Math formula
- Technical document

### plantuml

**Mục đích**: PlantUML diagram

**Diagram type**:
- Class diagram
- Sequence diagram
- Use case diagram
- Activity diagram

---

## Special Tool

### vim

**Mục đích**: Vim editor

**Core skill**:
- Mode switching
- Movement command
- Text object
- Macro và register
- Plugin management

### shell-scripting

**Mục đích**: Shell script writing

**Script type**:
- Bash
- Zsh
- POSIX

### latex

**Mục đích**: LaTeX typesetting

**Usage**:
- Academic paper
- Math formula
- Complex document

---

## Related Skill

| Skill | Usage |
|------|------|
| `solidity` | Smart contract development |
| `unity` | Unity game development |
| `video-editing` | Video editing |
| `kafka` | Kafka message queue |
| `pwa` | PWA development |
| `webgl` | WebGL graphics |