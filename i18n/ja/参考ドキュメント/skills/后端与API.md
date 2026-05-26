# バックエンド与 API 技能

## 概要

バックエンド与 API 関連スキル用于ビルドサーバー端应用、API 設計和データベース操作。

## コア技能

### backend-patterns

**用途**: バックエンド開発ベストプラクティス

**コア概念**:
- 分层アーキテクチャ（Controller → Service → Repository）
- 依赖注入
- エラー処理戦略
- キャッシュパターン
- 消息キュー統合

---

### api-design

**用途**: API 設計与RESTful 仕様

**コア概念**:
- RESTful ルーティング設計
- リクエスト/応答フォーマット
- 認証与認可
- バージョン控制戦略
- 限流与キャッシュ

---

### api-connector-builder

**用途**: API 接続器ビルドツール

**使用场景**:
- 第三方 API 統合
- Webhook 处理
- OAuth 流程実装
- API エラー再試行机制

---

## データベース技能

### database-migrations

**用途**: データベース移行管理

**コア概念**:
- 移行ファイル组织
- データロールバック戦略
- 种子データ管理
- 零停机デプロイ

---

### mysql-patterns

**用途**: MySQL データベースベストプラクティス

**コア概念**:
- インデックス最適化
- 查询最適化
- 分表戦略
- 主从コピー

---

### postgres-patterns

**用途**: PostgreSQL データベースベストプラクティス

**コア概念**:
- JSONB 操作
- 全文搜索
- 分区表
- 高级インデックス戦略

---

### redis-patterns

**用途**: Redis キャッシュパターン

**コア概念**:
- キャッシュ戦略（Cache-Aside、Write-Through）
- 分布式锁
- セッション存储
- 消息发布/購読

---

### clickhouse-io

**用途**: ClickHouse 列式存储

**使用场景**:
- 分析型查询
- ログ存储
- リアルタイム分析
- 大データ管道

---

## 消息キュー

### logistics-exception-management

**用途**: 物流异常管理

**使用场景**:
- 订单异常处理
- 退货流程
- 库存同期

---

## 関連スキル

- `backend-patterns` - バックエンドパターン
- `api-design` - API 設計
- `database-migrations` - データベース移行
- `mysql-patterns` - MySQL パターン
- `postgres-patterns` - PostgreSQL パターン
- `redis-patterns` - Redis パターン
- `clickhouse-io` - ClickHouse
