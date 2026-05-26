# AI 与머신러닝技能

本일부涵盖머신러닝、딥러닝、모델훈련与배포的技能。

---

## 머신러닝基础

### neural-network

**용도**: 神经网络설계与구현

**코어개념**:
- 前馈神经网络
- 卷积神经网络 (CNN)
- 루프神经网络 (RNN)
- 활성화함수与损失함수
- 反向传播算法

### deep-learning

**용도**: 딥러닝모델개발

**코어프레임워크**:
- PyTorch
- TensorFlow
- JAX

**应用场景**:
- 图像识别
- 自然언어处理
- 语音识别
- 권장시스템

### gradient-descent

**용도**: 梯度下降최적화算法

**코어개념**:
- 배치梯度下降
- 随机梯度下降 (SGD)
- Adam、RMSprop 等최적화器
- 학습率调度
- 梯度裁剪

### normalization

**용도**: 데이터归一化기술

**코어메서드**:
- Min-Max 归一化
- Z-score 표준화
- L2 归一化
- 批归一化 (Batch Normalization)
- 层归一化 (Layer Normalization)

---

## PyTorch 生态

### pytorch-training

**용도**: PyTorch 모델훈련

**코어技能**:
- 데이터集与 DataLoader
- 모델定义与초기화
- 훈련루프编写
- 검증与评估
- 검사点저장与복구

### pytorch-optimization

**용도**: PyTorch 모델최적화

**최적화기술**:
- 混合精度훈련 (AMP)
- 梯度累积
- 分布式훈련 (DDP)
- 모델剪枝
- 量化

### pytorch-deployment

**용도**: PyTorch 모델배포

**배포方式**:
- TorchScript 내보내기
- ONNX 내보내기
- LibTorch C++ API
- TorchServe
- mobile/iOS 배포

### pytorch-patterns

**용도**: PyTorch 惯用패턴

**코어패턴**:
- Module 子클래스化
- 损失함수自定义
- 回调시스템
- 훈련辅助도구

---

## 머신러닝프레임워크

### mxnet

**용도**: Apache MXNet 딥러닝프레임워크

**特点**:
- 자동微分
- Gluon API
- 多 GPU 지원
- 分布式훈련

### transformers

**용도**: Hugging Face Transformers 库

**코어기능**:
- 预훈련모델加载
- 모델微调 (Fine-tuning)
- 文本분류与生成
- 问答시스템
- 翻译与摘要

### spacy

**용도**: spaCy 自然언어处理

**기능**:
- 词性标注
- 命名实体识别 (NER)
- 依存분석
- 句子分割

### object-detection

**용도**: 目标检测모델

**经典모델**:
- YOLO
- Faster R-CNN
- SSD
- CenterNet

**应用场景**:
- 자동驾驶
- 视频모니터링
- 医学影像

---

## 모델与서비스

### model-route

**용도**: 모델라우팅与로드 밸런싱

**코어개념**:
- 모델버전管理
- 流量分配전략
- A/B 테스트
- 灰度게시

### on-device-ai

**용도**: 端侧 AI 배포

**배포平台**:
- 이동设备 (iOS/Android)
- 嵌入式시스템
- WebGL/WebAssembly

**최적화기술**:
- 모델剪枝
- 量化
- 知识蒸馏
- 추론최적화

### onnx

**용도**: ONNX 모델형식

**코어기능**:
- 모델형식转换
- 跨프레임워크互操作
- 추론加速

### seldon

**용도**: Seldon 모델서비스프레임워크

**기능**:
- Kubernetes 原生배포
- A/B 테스트与金丝雀
- 异常检测
- 모델解释

---

## 特定태스크

### named-entity-recognition

**용도**: 命名实体识别

**기술方案**:
- BiLSTM-CRF
- Transformer-based
- 预훈련언어모델微调

**应用场景**:
- 정보抽取
- 知识图谱빌드
- 问答시스템

### text-classification

**용도**: 文本분류

**메서드**:
- 传统머신러닝 (TF-IDF + SVM)
- 딥러닝 (CNN/RNN)
- 预훈련모델 (BERT)

### text-generation

**용도**: 文本生成

**모델**:
- GPT 系列
- T5
- BART

**应用**:
- 对话시스템
- 内容创作
- 코드生成

### text-to-sql

**용도**: 文本转 SQL

**기술**:
- Seq2Seq 모델
- 预훈련언어모델
- 强化학습微调

### speech-to-text

**용도**: 语音转文本

**기술**:
- Whisper
- Wav2Vec
- 端到端모델

### sentiment-analysis

**용도**: 情感분석

**메서드**:
- 基于词典
- 传统머신러닝
- 딥러닝

---

## 科学计算

### scientific-computing

**용도**: 科学计算

**코어库**:
- NumPy
- SciPy
- Pandas
- Matplotlib

### linear-algebra

**용도**: 线性代数

**코어개념**:
- 矩阵运算
- 特征值分解
- 奇异值分解 (SVD)
- 线性方程组求解

### signal-processing

**용도**: 信号处理

**应用**:
- 音频处理
- 图像处理
- 时间序列분석

### optimization

**용도**: 최적화理论

**메서드**:
- 凸최적화
- 约束최적화
- 组合최적화

---

## 관련スキル

| 技能 | 용도 |
|------|------|
| `pytorch-training` | PyTorch 모델훈련 |
| `transformers` | Hugging Face Transformers |
| `onnx` | ONNX 모델형식 |
| `mlops` | ML 运维 |
| `inferencing` | 모델추론최적화 |