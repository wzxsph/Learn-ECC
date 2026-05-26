# AI 与机器学习技能

本部分涵盖机器学习、深度学习、模型训练与部署的技能。

---

## 机器学习基础

### neural-network

**Назначение**: 神经网络Спроектировать与Реализовать

**Основные концепции**:
- 前馈神经网络
- 卷积神经网络 (CNN)
- 循环神经网络 (RNN)
- 激活函数与损失函数
- 反向传播算法

### deep-learning

**Назначение**: 深度学习模型开发

**核心Фреймворки**:
- PyTorch
- TensorFlow
- JAX

**应用Сценарий**:
- 图像识别
- 自然语言处理
- 语音识别
- 推荐系统

### gradient-descent

**Назначение**: 梯度下降优化算法

**Основные концепции**:
- 批量梯度下降
- 随机梯度下降 (SGD)
- Adam、RMSprop 等优化器
- 学习率调度
- 梯度裁剪

### normalization

**Назначение**: 数据归一化技术

**核心方法**:
- Min-Max 归一化
- Z-score 标准化
- L2 归一化
- 批归一化 (Batch Normalization)
- 层归一化 (Layer Normalization)

---

## PyTorch 生态

### pytorch-training

**Назначение**: PyTorch 模型训练

**核心技能**:
- 数据集与 DataLoader
- 模型定义与初始化
- 训练循环编写
- Проверка与评估
- 检查点保存与恢复

### pytorch-optimization

**Назначение**: PyTorch 模型优化

**优化技术**:
- 混合精度训练 (AMP)
- 梯度累积
- 分布式训练 (DDP)
- 模型剪枝
- 量化

### pytorch-deployment

**Назначение**: PyTorch 模型部署

**部署方式**:
- TorchScript 导出
- ONNX 导出
- LibTorch C++ API
- TorchServe
- mobile/iOS 部署

### pytorch-patterns

**Назначение**: PyTorch 惯用模式

**核心模式**:
- Module 子类化
- 损失函数自定义
- 回调系统
- 训练辅助工具

---

## 机器学习Фреймворки

### mxnet

**Назначение**: Apache MXNet 深度学习Фреймворки

**Особенности**:
- 自动微分
- Gluon API
- 多 GPU Поддерживать
- 分布式训练

### transformers

**Назначение**: Hugging Face Transformers 库

**核心功能**:
- 预训练模型加载
- 模型微调 (Fine-tuning)
- 文本分类与生成
- 问答系统
- 翻译与摘要

### spacy

**Назначение**: spaCy 自然语言处理

**功能**:
- 词性标注
- 命名实体识别 (NER)
- 依存分析
- 句子分割

### object-detection

**Назначение**: Цель检测模型

**经典模型**:
- YOLO
- Faster R-CNN
- SSD
- CenterNet

**应用Сценарий**:
- 自动驾驶
- 视频监控
- 医学影像

---

## 模型与服务

### model-route

**Назначение**: 模型路由与Балансировка нагрузки

**Основные концепции**:
- 模型版本Управлять
- 流量分配策略
- A/B Тестирование
- 灰度发布

### on-device-ai

**Назначение**: 端侧 AI 部署

**部署平台**:
- 移动设备 (iOS/Android)
- 嵌入式系统
- WebGL/WebAssembly

**优化技术**:
- 模型剪枝
- 量化
- 知识蒸馏
- 推理优化

### onnx

**Назначение**: ONNX 模型格式

**核心功能**:
- 模型格式转换
- 跨Фреймворки互操作
- 推理加速

### seldon

**Назначение**: Seldon 模型服务Фреймворки

**功能**:
- Kubernetes 原生部署
- A/B Тестирование与金丝雀
- 异常检测
- 模型解释

---

## 特定Задача

### named-entity-recognition

**Назначение**: 命名实体识别

**技术方案**:
- BiLSTM-CRF
- Transformer-based
- 预训练语言模型微调

**应用Сценарий**:
- 信息抽取
- 知识图谱构建
- 问答系统

### text-classification

**Назначение**: 文本分类

**方法**:
- 传统机器学习 (TF-IDF + SVM)
- 深度学习 (CNN/RNN)
- 预训练模型 (BERT)

### text-generation

**Назначение**: 文本生成

**模型**:
- GPT 系列
- T5
- BART

**应用**:
- 对话系统
- 内容创作
- 代码生成

### text-to-sql

**Назначение**: 文本转 SQL

**技术**:
- Seq2Seq 模型
- 预训练语言模型
- 强化学习微调

### speech-to-text

**Назначение**: 语音转文本

**技术**:
- Whisper
- Wav2Vec
- 端到端模型

### sentiment-analysis

**Назначение**: 情感分析

**方法**:
- 基于词典
- 传统机器学习
- 深度学习

---

## 科学计算

### scientific-computing

**Назначение**: 科学计算

**核心库**:
- NumPy
- SciPy
- Pandas
- Matplotlib

### linear-algebra

**Назначение**: 线性代数

**Основные концепции**:
- 矩阵运算
- 特征值分解
- 奇异值分解 (SVD)
- 线性方程组求解

### signal-processing

**Назначение**: 信号处理

**应用**:
- 音频处理
- 图像处理
- 时间序列分析

### optimization

**Назначение**: 优化理论

**方法**:
- 凸优化
- 约束优化
- 组合优化

---

## 相关技能

| 技能 | Назначение |
|------|------|
| `pytorch-training` | PyTorch 模型训练 |
| `transformers` | Hugging Face Transformers |
| `onnx` | ONNX 模型格式 |
| `mlops` | ML 运维 |
| `inferencing` | 模型推理优化 |