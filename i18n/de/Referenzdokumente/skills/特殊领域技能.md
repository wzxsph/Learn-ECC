# 特殊领域技能

本部分涵盖特定领域的专业技能，包括区块链、游戏开发、音视频处理等。

---

## 区块链与 Web3

### solidity

**用途**: Solidity 智能合约开发

**核心概念**:
- 合约结构
- 数据类型
- 函数修饰符
- 事件与日志
- 继承与库

**示例**:
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

**用途**: DeFi AMM 安全

**核心概念**:
- 无常损失
- 闪电贷攻击
- 预言机操纵
- 合约审计

### ethereum-development

**用途**: 以太坊开发

**工具**:
- Hardhat
- Foundry
- ethers.js
- web3.js

---

## 游戏开发

### unity

**用途**: Unity 游戏开发

**核心概念**:
- 场景管理
- 物理引擎
- 粒子系统
- 动画状态机
- UI 系统

### godot

**用途**: Godot 游戏引擎

**特点**:
- GDScript
- 场景系统
- 信号机制
- 跨平台导出

### unreal-engine

**用途**: Unreal Engine 开发

**核心概念**:
- Blueprint
- C++ 编程
- 材质系统
- 动画蓝图

### game-ai

**用途**: 游戏 AI

**技术**:
- 状态机
- 行为树
- 路径规划
- 决策树

---

## 音视频处理

### video-editing

**用途**: 视频编辑

**工具**:
- FFmpeg
- DaVinci Resolve
- Adobe Premiere

### audio-processing

**用途**: 音频处理

**技术**:
- 音频编码
- 降噪
- 混音

### streaming

**用途**: 流媒体处理

**协议**:
- HLS
- DASH
- RTMP

### media-processing

**用途**: 媒体处理管道

**工具**:
- GStreamer
- MediaConvert
- Transmux

---

## 物联网

### bluetooth-low-energy

**用途**: BLE 开发

**核心概念**:
- GATT 协议
- 广播与扫描
- 连接管理
- 功耗优化

### iot-development

**用途**: IoT 开发

**平台**:
- Arduino
- Raspberry Pi
- ESP32

### embedded-systems

**用途**: 嵌入式系统

**技能**:
- C/C++ 编程
- 裸机开发
- RTOS
- 驱动开发

---

## 数据库与存储

### sql-injection

**用途**: SQL 注入防御

**防御措施**:
- 参数化查询
- ORM 使用
- 输入验证
- 最小权限原则

### postgresql

**用途**: PostgreSQL 最佳实践

**核心概念**:
- 索引策略
- 查询优化
- 分区表
- JSONB 操作
- 复制配置

### sqlite

**用途**: SQLite 使用

**特点**:
- 轻量级
- 无服务器
- ACID 兼容
- 移动端应用

### snowflake

**用途**: Snowflake 数据仓库

**核心功能**:
- 数据分享
- 时间旅行
- 克隆
- 扩展微分区

---

## 消息队列

### kafka

**用途**: Apache Kafka

**核心概念**:
- Topic 与 Partition
- 生产者与消费者
- 消费者组
- 复制与一致性
- 流处理

**示例**:
```java
// 生产者
KafkaProducer<String, String> producer = new KafkaProducer<>(props);
ProducerRecord<String, String> record = new ProducerRecord<>("topic", "key", "value");
producer.send(record);

// 消费者
KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
consumer.subscribe(List.of("topic"));
while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record : records) {
        // 处理消息
    }
}
```

### nats

**用途**: NATS 消息系统

**特点**:
- 轻量级
- 低延迟
- 支持多种模式 (发布/订阅、请求/响应)

### mqtt

**用途**: MQTT 物联网协议

**QoS 级别**:
- QoS 0: 至多一次
- QoS 1: 至少一次
- QoS 2: 恰好一次

---

## 前端特定领域

### pwa

**用途**: Progressive Web App

**核心功能**:
- Service Worker
- Web App Manifest
- 离线支持
- 推送通知

### webgl

**用途**: WebGL 图形编程

**应用**:
- 3D 渲染
- 游戏
- 数据可视化

### webrtc

**用途**: WebRTC 实时通信

**核心 API**:
- getUserMedia
- RTCPeerConnection
- RTCDataChannel

---

## 文档与写作

### markdown

**用途**: Markdown 编写

**扩展**:
- GFM (GitHub Flavored Markdown)
- 表格
- 代码块高亮
- 任务列表

### latex

**用途**: LaTeX 文档排版

**用途**:
- 学术论文
- 数学公式
- 技术文档

### plantuml

**用途**: PlantUML 图表

**图表类型**:
- 类图
- 序列图
- 用例图
- 活动图

---

## 特殊工具

### vim

**用途**: Vim 编辑器

**核心技能**:
- 模式切换
- 移动命令
- 文本对象
- 宏与寄存器
- 插件管理

### shell-scripting

**用途**: Shell 脚本编写

**脚本类型**:
- Bash
- Zsh
- POSIX

### latex

**用途**: LaTeX 排版

**用途**:
- 学术论文
- 数学公式
- 复杂文档

---

## 相关技能

| 技能 | 用途 |
|------|------|
| `solidity` | 智能合约开发 |
| `unity` | Unity 游戏开发 |
| `video-editing` | 视频编辑 |
| `kafka` | Kafka 消息队列 |
| `pwa` | PWA 开发 |
| `webgl` | WebGL 图形 |