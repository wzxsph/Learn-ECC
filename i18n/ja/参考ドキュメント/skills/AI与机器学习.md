# AI 与機械学習技能

本一部涵盖機械学習、ディープ ラーニング、モデル訓練与デプロイ的技能。

---

## 機械学習基础

### neural-network

**用途**: 神经网络設計与実装

**コア概念**:
- 前馈神经网络
- 卷积神经网络 (CNN)
- ループ神经网络 (RNN)
- アクティブ化関数与损失関数
- 反向传播算法

### deep-learning

**用途**: ディープ ラーニングモデル開発

**コアフレームワーク**:
- PyTorch
- TensorFlow
- JAX

**应用场景**:
- 图像识别
- 自然言語处理
- 语音识别
- 推奨システム

### gradient-descent

**用途**: 梯度下降最適化算法

**コア概念**:
- バッチ梯度下降
- 随机梯度下降 (SGD)
- Adam、RMSprop 等最適化器
- 学習率调度
- 梯度裁剪

### normalization

**用途**: データ归一化技術

**コアメソッド**:
- Min-Max 归一化
- Z-score 標準化
- L2 归一化
- 批归一化 (Batch Normalization)
- 层归一化 (Layer Normalization)

---

## PyTorch 生态

### pytorch-training

**用途**: PyTorch モデル訓練

**コア技能**:
- データ集与 DataLoader
- モデル定义与初期化
- 訓練ループ编写
- 検証与评估
- 検査点保存与再開

### pytorch-optimization

**用途**: PyTorch モデル最適化

**最適化技術**:
- 混合精度訓練 (AMP)
- 梯度累积
- 分布式訓練 (DDP)
- モデル剪枝
- 量化

### pytorch-deployment

**用途**: PyTorch モデルデプロイ

**デプロイ方式**:
- TorchScript エクスポート
- ONNX エクスポート
- LibTorch C++ API
- TorchServe
- mobile/iOS デプロイ

### pytorch-patterns

**用途**: PyTorch 惯用パターン

**コアパターン**:
- Module 子クラス化
- 损失関数自定义
- 回调システム
- 訓練辅助ツール

---

## 機械学習フレームワーク

### mxnet

**用途**: Apache MXNet ディープ ラーニングフレームワーク

**特点**:
- 自動微分
- Gluon API
- 多 GPU サポート
- 分布式訓練

### transformers

**用途**: Hugging Face Transformers 库

**コア機能**:
- 预訓練モデル加载
- モデル微调 (Fine-tuning)
- 文本分類与生成
- 问答システム
- 翻译与摘要

### spacy

**用途**: spaCy 自然言語处理

**機能**:
- 词性标注
- 命名实体识别 (NER)
- 依存分析
- 句子分割

### object-detection

**用途**: 目标检测モデル

**经典モデル**:
- YOLO
- Faster R-CNN
- SSD
- CenterNet

**应用场景**:
- 自動驾驶
- 视频監視
- 医学影像

---

## モデル与サービス

### model-route

**用途**: モデルルーティング与負荷分散

**コア概念**:
- モデルバージョン管理
- 流量分配戦略
- A/B テスト
- 灰度发布

### on-device-ai

**用途**: 端侧 AI デプロイ

**デプロイ平台**:
- 移動设备 (iOS/Android)
- 嵌入式システム
- WebGL/WebAssembly

**最適化技術**:
- モデル剪枝
- 量化
- 知识蒸馏
- 推論最適化

### onnx

**用途**: ONNX モデルフォーマット

**コア機能**:
- モデルフォーマット转换
- 跨フレームワーク互操作
- 推論加速

### seldon

**用途**: Seldon モデルサービスフレームワーク

**機能**:
- Kubernetes 原生デプロイ
- A/B テスト与金丝雀
- 异常检测
- モデル解释

---

## 特定タスク

### named-entity-recognition

**用途**: 命名实体识别

**技術方案**:
- BiLSTM-CRF
- Transformer-based
- 预訓練言語モデル微调

**应用场景**:
- 情報抽取
- 知识图谱ビルド
- 问答システム

### text-classification

**用途**: 文本分類

**メソッド**:
- 传统機械学習 (TF-IDF + SVM)
- ディープ ラーニング (CNN/RNN)
- 预訓練モデル (BERT)

### text-generation

**用途**: 文本生成

**モデル**:
- GPT 系列
- T5
- BART

**应用**:
- 对话システム
- 内容创作
- コード生成

### text-to-sql

**用途**: 文本转 SQL

**技術**:
- Seq2Seq モデル
- 预訓練言語モデル
- 强化学習微调

### speech-to-text

**用途**: 语音转文本

**技術**:
- Whisper
- Wav2Vec
- 端到端モデル

### sentiment-analysis

**用途**: 情感分析

**メソッド**:
- 基于词典
- 传统機械学習
- ディープ ラーニング

---

## 科学计算

### scientific-computing

**用途**: 科学计算

**コア库**:
- NumPy
- SciPy
- Pandas
- Matplotlib

### linear-algebra

**用途**: 线性代数

**コア概念**:
- 矩阵运算
- 特征值分解
- 奇异值分解 (SVD)
- 线性方程组求解

### signal-processing

**用途**: 信号处理

**应用**:
- 音频处理
- 图像处理
- 时间序列分析

### optimization

**用途**: 最適化理论

**メソッド**:
- 凸最適化
- 约束最適化
- 组合最適化

---

## 関連スキル

| 技能 | 用途 |
|------|------|
| `pytorch-training` | PyTorch モデル訓練 |
| `transformers` | Hugging Face Transformers |
| `onnx` | ONNX モデルフォーマット |
| `mlops` | ML 运维 |
| `inferencing` | モデル推論最適化 |