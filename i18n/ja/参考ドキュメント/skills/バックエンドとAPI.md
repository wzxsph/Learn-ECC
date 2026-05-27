# バックエンド與 API 技能

## 概要

バックエンド與 API 関連スキル使用ビルドサーバー端アプリケーション、API 設計和データベース操作。

## コア技能

### backend-patterns

**用途**: バックエンド開発ベストプラクティス

**コア概念**:
- 分层アーキテクチャ（Controller → Service → Repository）
- 依賴注入
- エラー処理戦略
- キャッシュパターン
- 消息キュー統合

---

### api-design

**用途**: API 設計與RESTful 仕様

**コア概念**:
- RESTful ルーティング設計
- リクエスト/応答フォーマット
- 認証與認可
- バージョン控制戦略
- 限流與キャッシュ

---

### api-connector-builder

**用途**: API 接続器ビルドツール

**使用场景**:
- 第三方 API 統合
- Webhook 處理
- OAuth 流程実装
- API エラー再試行机制

---

## データベース技能

### database-migrations

**用途**: データベース移行管理

**コア概念**:
- 移行ファイル組織
- データロールバック戦略
- 种子データ管理
- 零停机デプロイ

---

### mysql-patterns

**用途**: MySQL データベースベストプラクティス

**コア概念**:
- インデックス最適化
- 查詢最適化
- 分表戦略
- 主從コピー

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
- セッション存儲
- 消息發布/購読

---

### clickhouse-io

**用途**: ClickHouse 列式存儲

**使用场景**:
- 分析型查詢
- ログ存儲
- リアルタイム分析
- 大データ管道

---

## 消息キュー

### logistics-exception-management

**用途**: 物流例外管理

**使用场景**:
- 订单例外處理
- 退货流程
- 庫存同期

---

## 関連スキル

- `backend-patterns` - バックエンドパターン
- `api-design` - API 設計
- `database-migrations` - データベース移行
- `mysql-patterns` - MySQL パターン
- `postgres-patterns` - PostgreSQL パターン
- `redis-patterns` - Redis パターン
- `clickhouse-io` - ClickHouse
