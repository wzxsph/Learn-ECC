# デプロイ與 DevOps 技能

## 概要

デプロイ與 DevOps 技能使用コンテナ化、インフラ即コード、継続統合和デプロイ自動化。

## コア技能

### docker-patterns

**用途**: Docker コンテナベストプラクティス

**コア概念**:
- 多フェーズビルド
- 最小化镜像サイズ
- 健康検査
- ログ管理
- 資源限制

---

### deployment-patterns

**用途**: デプロイ戦略與パターン

**コア概念**:
- 藍綠デプロイ
- 金丝雀發布
- Rolling Update
- ロールバック戦略
- 零停机デプロイ

---

### production-audit

**用途**: 本番環境審計

**チェック項目**:
- セキュリティ設定
- パフォーマンス指標
- 可用性検査
- 監視アラート検証

---

### production-scheduling

**用途**: 本番調度管理

**使用场景**:
- 定時タスク管理
- 批處理ジョブ
- 維護視窗計画

---

### canary-watch

**用途**: 金丝雀發布監視

**機能**:
- 流量分割監視
- エラー率トレース
- パフォーマンス對比
- 自動ロールバック觸發

---

## インフラ

### network-config-validation

**用途**: 網路設定検証

**チェック項目**:
- 防火牆ルール
- 負荷分散設定
- DNS 設定
- SSL/TLS 憑證

---

### network-interface-health

**用途**: 網路インターフェース健康検査

**監視指標**:
- 頻寬使用
- 遅延
- 丢含む率
- 接続数

---

## 関連スキル

- `docker-patterns` - Docker パターン
- `deployment-patterns` - デプロイパターン
- `production-audit` - 本番審計
- `production-scheduling` - 本番調度
- `canary-watch` - 金丝雀監視
- `network-config-validation` - 網路設定検証
- `network-interface-health` - 網路インターフェース健康
