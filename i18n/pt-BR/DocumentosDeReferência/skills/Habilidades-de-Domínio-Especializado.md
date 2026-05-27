# Habilidades de Domínio Especializado

Esta seção abrange habilidades especializadas em domínios específicos, incluindo blockchain, desenvolvimento de jogos, processamento de áudio/vídeo e mais.

---

## Blockchain e Web3

### solidity

**Propósito**: Desenvolvimento de smart contracts Solidity

**Conceitos Centrais**:
- Estrutura de contratos
- Tipos de dados
- Modificadores de funções
- Eventos e logs
- Herança e bibliotecas

**Exemplo**:
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

**Propósito**: Segurança DeFi AMM

**Conceitos Centrais**:
- Perda impermanente
- Ataques de flash loan
- Manipulação de oráculos
- Auditoria de contratos

### ethereum-development

**Propósito**: Desenvolvimento Ethereum

**Ferramentas**:
- Hardhat
- Foundry
- ethers.js
- web3.js

---

## Desenvolvimento de Jogos

### unity

**Propósito**: Desenvolvimento de jogos Unity

**Conceitos Centrais**:
- Gerenciamento de cenas
- Motor físico
- Sistemas de partículas
- Máquinas de estado de animação
- Sistema de UI

### godot

**Propósito**: Engine de jogos Godot

**Características**:
- GDScript
- Sistema de cenas
- Mecanismo de sinais
- Exportação cross-platform

### unreal-engine

**Propósito**: Desenvolvimento Unreal Engine

**Conceitos Centrais**:
- Blueprint
- Programação C++
- Sistema de materiais
- Blueprints de animação

### game-ai

**Propósito**: AI de jogos

**Tecnologias**:
- Máquinas de estado
- Árvores de comportamento
- Planejamento de caminhos
- Árvores de decisão

---

## Processamento de Áudio/Vídeo

### video-editing

**Propósito**: Edição de vídeo

**Ferramentas**:
- FFmpeg
- DaVinci Resolve
- Adobe Premiere

### audio-processing

**Propósito**: Processamento de áudio

**Tecnologias**:
- Codificação de áudio
- Redução de ruído
- Mixagem

### streaming

**Propósito**: Processamento de streaming

**Protocolos**:
- HLS
- DASH
- RTMP

### media-processing

**Propósito**: Pipeline de processamento de mídia

**Ferramentas**:
- GStreamer
- MediaConvert
- Transmux

---

## Internet das Coisas

### bluetooth-low-energy

**Propósito**: Desenvolvimento BLE

**Conceitos Centrais**:
- Protocolo GATT
- Broadcasting e scanning
- Gerenciamento de conexões
- Otimização de consumo de energia

### iot-development

**Propósito**: Desenvolvimento IoT

**Plataformas**:
- Arduino
- Raspberry Pi
- ESP32

### embedded-systems

**Propósito**: Sistemas embarcados

**Habilidades**:
- Programação C/C++
- Desenvolvimento bare-metal
- RTOS
- Desenvolvimento de drivers

---

## Banco de Dados e Armazenamento

### sql-injection

**Propósito**: Defesa contra injeção SQL

**Medidas de Defesa**:
- Queries parametrizadas
- Uso de ORM
- Validação de input
- Princípio de privilégio mínimo

### postgresql

**Propósito**: Melhores-Práticas PostgreSQL

**Conceitos Centrais**:
- Estratégias de indexação
- Otimização de queries
- Tabelas particionadas
- Operações JSONB
- Configuração de replicação

### sqlite

**Propósito**: Uso de SQLite

**Características**:
- Leve
- Sem servidor
- Compatível com ACID
- Aplicações móveis

### snowflake

**Propósito**: Data warehouse Snowflake

**Funcionalidades Centrais**:
- Compartilhamento de dados
- Viagem no tempo
- Clonagem
- Extensão de micropartição

---

## Message Queue

### kafka

**Propósito**: Apache Kafka

**Conceitos Centrais**:
- Tópico e Partição
- Produtor e Consumidor
- Grupo de consumidores
- Replicação e consistência
- Processamento de streams

**Exemplo**:
```java
// Produtor
KafkaProducer<String, String> producer = new KafkaProducer<>(props);
ProducerRecord<String, String> record = new ProducerRecord<>("topic", "key", "value");
producer.send(record);

// Consumidor
KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(List.of("topic"));
while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : records) {
        // Processar mensagem
    }
}
```

### nats

**Propósito**: Sistema de mensagens NATS

**Características**:
- Leve
- Baixa latência
- Suporte a múltiplos padrões (publicação/assinatura, request/response)

### mqtt

**Propósito**: Protocolo MQTT IoT

**Níveis de QoS**:
- QoS 0: No máximo uma vez
- QoS 1: Pelo menos uma vez
- QoS 2: Exatamente uma vez

---

## Domínios Frontend Específicos

### pwa

**Propósito**: Progressive Web App

**Funcionalidades Centrais**:
- Service Worker
- Web App Manifest
- Suporte offline
- Notificações push

### webgl

**Propósito**: Programação gráfica WebGL

**Aplicações**:
- Renderização 3D
- Jogos
- Visualização de dados

### webrtc

**Propósito**: Comunicação em tempo real WebRTC

**APIs Centrais**:
- getUserMedia
- RTCPeerConnection
- RTCDataChannel

---

## Documentação e Escrita

### markdown

**Propósito**: Escrita Markdown

**Extensões**:
- GFM (GitHub Flavored Markdown)
- Tabelas
- Destaque de código em blocos
- Listas de tarefas

### latex

**Propósito**: Tipografia LaTeX

**Propósito**:
- Papéis acadêmicos
- Fórmulas matemáticas
- Documentação técnica

### plantuml

**Propósito**: Diagramas PlantUML

**Tipos de Diagramas**:
- Diagramas de classe
- Diagramas de sequência
- Diagramas de caso de uso
- Diagramas de atividade

---

## Ferramentas Especiais

### vim

**Propósito**: Editor Vim

**Habilidades Centrais**:
- Troca de modos
- Comandos de movimento
- Objetos de texto
- Macros e registros
- Gerenciamento de plugins

### shell-scripting

**Propósito**: Escrita de scripts shell

**Tipos de Scripts**:
- Bash
- Zsh
- POSIX

---

## Habilidades Relacionadas

| Habilidade | Propósito |
|------|------|
| `solidity` | Desenvolvimento de smart contracts |
| `unity` | Desenvolvimento de jogos Unity |
| `video-editing` | Edição de vídeo |
| `kafka` | Message queue Kafka |
| `pwa` | Desenvolvimento PWA |
| `webgl` | Gráficos WebGL |