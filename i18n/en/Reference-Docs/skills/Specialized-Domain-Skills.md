# Specialized Domain Skills

This section covers specialized domain skills including blockchain, game development, audio/video processing, and more.

---

## Blockchain and Web3

### solidity

**Purpose**: Solidity smart contract development

**Core concepts**:
- Contract structure
- Data types
- Function modifiers
- Events and logs
- Inheritance and libraries

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

**Purpose**: DeFi AMM security

**Core concepts**:
- Impermanent loss
- Flash loan attacks
- Oracle manipulation
- Contract auditing

### ethereum-development

**Purpose**: Ethereum development

**Tools**:
- Hardhat
- Foundry
- ethers.js
- web3.js

---

## Game Development

### unity

**Purpose**: Unity game development

**Core concepts**:
- Scene management
- Physics engine
- Particle systems
- Animation state machines
- UI system

### godot

**Purpose**: Godot game engine

**Features**:
- GDScript
- Scene system
- Signal mechanism
- Cross-platform export

### unreal-engine

**Purpose**: Unreal Engine development

**Core concepts**:
- Blueprint
- C++ programming
- Material system
- Animation blueprints

### game-ai

**Purpose**: Game AI

**Techniques**:
- State machines
- Behavior trees
- Path planning
- Decision trees

---

## Audio and Video Processing

### video-editing

**Purpose**: Video editing

**Tools**:
- FFmpeg
- DaVinci Resolve
- Adobe Premiere

### audio-processing

**Purpose**: Audio processing

**Techniques**:
- Audio encoding
- Noise reduction
- Mixing

### streaming

**Purpose**: Streaming media processing

**Protocols**:
- HLS
- DASH
- RTMP

### media-processing

**Purpose**: Media processing pipeline

**Tools**:
- GStreamer
- MediaConvert
- Transmux

---

## Internet of Things

### bluetooth-low-energy

**Purpose**: BLE development

**Core concepts**:
- GATT protocol
- Broadcasting and scanning
- Connection management
- Power consumption optimization

### iot-development

**Purpose**: IoT development

**Platforms**:
- Arduino
- Raspberry Pi
- ESP32

### embedded-systems

**Purpose**: Embedded systems

**Skills**:
- C/C++ programming
- Bare-metal development
- RTOS
- Driver development

---

## Database and Storage

### sql-injection

**Purpose**: SQL injection defense

**Defense measures**:
- Parameterized queries
- Using ORM
- Input validation
- Principle of least privilege

### postgresql

**Purpose**: PostgreSQL best practices

**Core concepts**:
- Index strategies
- Query optimization
- Partitioned tables
- JSONB operations
- Replication configuration

### sqlite

**Purpose**: SQLite usage

**Features**:
- Lightweight
- Serverless
- ACID compliant
- Mobile application

### snowflake

**Purpose**: Snowflake data warehouse

**Core features**:
- Data sharing
- Time travel
- Cloning
- Extended micro-partitions

---

## Message Queues

### kafka

**Purpose**: Apache Kafka

**Core concepts**:
- Topic and Partition
- Producer and Consumer
- Consumer groups
- Replication and consistency
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
        // process message
    }
}
```

### nats

**Purpose**: NATS messaging system

**Features**:
- Lightweight
- Low latency
- Supports multiple patterns (pub/sub, request/reply)

### mqtt

**Purpose**: MQTT IoT protocol

**QoS levels**:
- QoS 0: At most once
- QoS 1: At least once
- QoS 2: Exactly once

---

## Frontend-Specific Domains

### pwa

**Purpose**: Progressive Web App

**Core features**:
- Service Worker
- Web App Manifest
- Offline support
- Push notifications

### webgl

**Purpose**: WebGL graphics programming

**Applications**:
- 3D rendering
- Gaming
- Data visualization

### webrtc

**Purpose**: WebRTC real-time communication

**Core APIs**:
- getUserMedia
- RTCPeerConnection
- RTCDataChannel

---

## Documentation and Writing

### markdown

**Purpose**: Markdown authoring

**Extensions**:
- GFM (GitHub Flavored Markdown)
- Tables
- Code block highlighting
- Task lists

### latex

**Purpose**: LaTeX document typesetting

**Uses**:
- Academic papers
- Mathematical formulas
- Technical documentation

### plantuml

**Purpose**: PlantUML diagrams

**Diagram types**:
- Class diagrams
- Sequence diagrams
- Use case diagrams
- Activity diagrams

---

## Specialized Tools

### vim

**Purpose**: Vim editor

**Core skills**:
- Mode switching
- Movement commands
- Text objects
- Macros and registers
- Plugin management

### shell-scripting

**Purpose**: Shell script authoring

**Script types**:
- Bash
- Zsh
- POSIX

---

## Related Skills

| Skill | Purpose |
|-------|---------|
| `solidity` | Smart contract development |
| `unity` | Unity game development |
| `video-editing` | Video editing |
| `kafka` | Kafka message queue |
| `pwa` | PWA development |
| `webgl` | WebGL graphics |