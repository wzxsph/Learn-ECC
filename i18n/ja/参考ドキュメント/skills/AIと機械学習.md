# AI 與機械学習技能

本一部涵盖機械学習、ディープ ラーニング、モデル訓練與デプロイ的技能。

---

## 機械学習基础

### neural-network

**用途**: 神经網路設計與実装

**コア概念**:
- 前馈神经網路
- 卷积神经網路 (CNN)
- ループ神经網路 (RNN)
- アクティブ化関数與損失関数
- 反向传播算法

### deep-learning

**用途**: ディープ ラーニングモデル開発

**コアフレームワーク**:
- PyTorch
- TensorFlow
- JAX

**アプリケーション场景**:
- 圖像识别
- 自然言語處理
- 语音识别
- 推奨システム

### gradient-descent

**用途**: 梯度下降最適化算法

**コア概念**:
- バッチ梯度下降
- 随机梯度下降 (SGD)
- Adam、RMSprop 等最適化器
- 学習率調度
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
- データ集與 DataLoader
- モデル定義與初期化
- 訓練ループ编写
- 検証與評估
- 検査点儲存與再開

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
- 損失関数自定義
- 回呼システム
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

**用途**: Hugging Face Transformers 庫

**コア機能**:
- 预訓練モデル載入
- モデル微调 (Fine-tuning)
- 文本分類與生成
- 问答システム
- 翻译與摘要

### spacy

**用途**: spaCy 自然言語處理

**機能**:
- 词性标注
- 命名实体识别 (NER)
- 依存分析
- 句子分割

### object-detection

**用途**: 目標检测モデル

**经典モデル**:
- YOLO
- Faster R-CNN
- SSD
- CenterNet

**アプリケーション场景**:
- 自動驾驶
- 视频監視
- 医学影像

---

## モデル與サービス

### model-route

**用途**: モデルルーティング與負荷分散

**コア概念**:
- モデルバージョン管理
- 流量分配戦略
- A/B テスト
- 灰度發布

### on-device-ai

**用途**: 端侧 AI デプロイ

**デプロイ平台**:
- 移動设备 (iOS/Android)
- 嵌入式システム
- WebGL/WebAssembly

**最適化技術**:
- モデル剪枝
- 量化
- 知識蒸馏
- 推論最適化

### onnx

**用途**: ONNX モデルフォーマット

**コア機能**:
- モデルフォーマット轉換
- 跨フレームワーク互操作
- 推論加速

### seldon

**用途**: Seldon モデルサービスフレームワーク

**機能**:
- Kubernetes 原生デプロイ
- A/B テスト與金丝雀
- 例外检测
- モデル解释

---

## 特定タスク

### named-entity-recognition

**用途**: 命名实体识别

**技術方案**:
- BiLSTM-CRF
- Transformer-based
- 预訓練言語モデル微调

**アプリケーション场景**:
- 情報抽取
- 知識圖谱ビルド
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

**アプリケーション**:
- 對话システム
- 內容创作
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
- 基づいて词典
- 传统機械学習
- ディープ ラーニング

---

## 科学计算

### scientific-computing

**用途**: 科学计算

**コア庫**:
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

**用途**: 信号處理

**アプリケーション**:
- 音频處理
- 圖像處理
- 時間序列分析

### optimization

**用途**: 最適化理论

**メソッド**:
- 凸最適化
- 约束最適化
- 組合最適化

---

## 関連スキル

| 技能 | 用途 |
|------|------|
| `pytorch-training` | PyTorch モデル訓練 |
| `transformers` | Hugging Face Transformers |
| `onnx` | ONNX モデルフォーマット |
| `mlops` | ML 运维 |
| `inferencing` | モデル推論最適化 |