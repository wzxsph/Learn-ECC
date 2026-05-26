# 01-MCP 開発

モデルコンテキストプロトコル（Model Context Protocol）拡張開発を学ぶ。

## MCPとは？

MCP（Model Context Protocol）は、Claude と外部ツールやサービスを統合するためのプロトコルです。MCP により、Claude の機能を拡張し、データベース、API、他のAIモデルなどに接続できます。

> 参考: [MCP 配置格式](../参考ドキュメント/mcp/MCP配置格式.md)

## 学習目標

- MCP アーキテクチャと原理の理解
- MCP サーバーの作成
- MCP ツールの実装
- MCP 拡張のテストとデバッグ

## コアコンセプト

### 1. MCP サーバー
MCP サーバーは、ツールとリソースを提供するプロセスです。

### 2. ツール（Tools）
Claude が呼び出して実行できる操作です。

### 3. リソース（Resources）
Claude が読み取れるデータソースです。

### 4. プロンプト（Prompts）
事前定義されたプロンプトテンプレートです。

## MCP 設定

MCP 設定ファイルは `.claude/mcp.json` に配置されます：

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["./mcp-server.js"],
      "env": {
        "API_KEY": "your-key"
      }
    }
  }
}
```

## 次のステップ

- [自律エージェント](./02-自律エージェント.md) - 自律型 Agent の構築
- [専門家之路ディレクトリに戻る](../README.md)