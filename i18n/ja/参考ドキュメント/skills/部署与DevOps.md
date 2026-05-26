# デプロイ与 DevOps 技能

## 概要

デプロイ与 DevOps 技能用于コンテナ化、インフラ即コード、持续統合和デプロイ自動化。

## コア技能

### docker-patterns

**用途**: Docker コンテナベストプラクティス

**コア概念**:
- 多フェーズビルド
- 最小化镜像サイズ
- 健康検査
- ログ管理
- 资源限制

---

### deployment-patterns

**用途**: デプロイ戦略与パターン

**コア概念**:
- 蓝绿デプロイ
- 金丝雀发布
- Rolling Update
- ロールバック戦略
- 零停机デプロイ

---

### production-audit

**用途**: 生产环境审计

**チェック項目**:
- セキュリティ設定
- パフォーマンス指標
- 可用性検査
- 監視アラート検証

---

### production-scheduling

**用途**: 生产调度管理

**使用场景**:
- 定时タスク管理
- 批处理ジョブ
- 维护窗口計画

---

### canary-watch

**用途**: 金丝雀发布監視

**機能**:
- 流量分割監視
- エラー率トレース
- パフォーマンス对比
- 自動ロールバック触发

---

## インフラ

### network-config-validation

**用途**: 网络設定検証

**チェック項目**:
- 防火墙ルール
- 負荷分散設定
- DNS 設定
- SSL/TLS 证书

---

### network-interface-health

**用途**: 网络インターフェース健康検査

**監視指標**:
- 带宽使用
- 遅延
- 丢パッケージ率
- 接続数

---

## 関連スキル

- `docker-patterns` - Docker パターン
- `deployment-patterns` - デプロイパターン
- `production-audit` - 生产审计
- `production-scheduling` - 生产调度
- `canary-watch` - 金丝雀監視
- `network-config-validation` - 网络設定検証
- `network-interface-health` - 网络インターフェース健康
