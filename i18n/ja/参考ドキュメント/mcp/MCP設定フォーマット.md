# MCP 設定フォーマット

## 概要

MCP (Model Context Protocol) サーバー通過 `~/.claude.json` ファイル中的 `mcpServers` 节进行設定。Claude Code サポート两种タイプ的 MCP サーバー：

- **プロセスパターン (process)**：通過本地コマンド起動的サーバー
- **HTTP パターン (http)**：通過 HTTP/HTTPS 訪問的远程サーバー

## JSON 設定構造

```json
{
  "mcpServers": {
    "サーバー名前": {
      "type": "http",           // オプション，デフォルト process
      "command": "npx",        // プロセスパターン必需
      "url": "https://...",    // HTTP パターン必需
      "args": ["參數1", "參數2"],  // オプション
      "env": {                 // オプション，環境変数
        "KEY": "value"
      },
      "headers": {             // オプション，HTTP リクエスト头
        "Header-Name": "value"
      },
      "description": "サーバー記述"  // オプション
    }
  }
}
```

## 欄位説明

| 欄位 | タイプ | 必需 | 説明 |
|------|------|------|------|
| `type` | string | 否 | 接続タイプ：`process`（デフォルト）或 `http` |
| `command` | string | type=process 时必需 | 起動コマンド（如 `npx`, `uvx`, `python3`） |
| `url` | string | type=http 时必需 | MCP サーバー的 HTTP/HTTPS URL |
| `args` | array | 否 | 传递給コマンド的參數陣列 |
| `env` | object | 否 | 環境変数键值對 |
| `headers` | object | 否 | HTTP リクエスト头（仅 http パターン） |
| `description` | string | 否 | サーバー機能記述 |

## 設定サンプル

### プロセスパターンサンプル

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_xxxx"
      },
      "description": "GitHub 操作 - PRs, issues, repos"
    }
  }
}
```

### HTTP パターンサンプル

```json
{
  "mcpServers": {
    "vercel": {
      "type": "http",
      "url": "https://mcp.vercel.com",
      "description": "Vercel デプロイ和プロジェクト"
    }
  }
}
```

### 带認証头的 HTTP パターン

```json
{
  "mcpServers": {
    "browser-use": {
      "type": "http",
      "url": "https://api.browser-use.com/mcp",
      "headers": {
        "x-browser-use-api-key": "YOUR_KEY_HERE"
      },
      "description": "AI 浏览器プロキシ"
    }
  }
}
```

## ベストプラクティス

1. **環境変数管理**：敏感情報（API keys, tokens）must使用環境変数，不要硬エンコード
2. **上下文視窗**：推奨保持あり効化不超过 10 个 MCP サーバー，以保留上下文視窗
3. **無効化サーバー**：使用 `ECC_DISABLED_MCPS=server1,server2` 環境変数可無効化打含む的 MCP サーバー

## 故障排除

- 確保コマンド可実行（npx, uvx 等已インストール）
- 検査環境変数は否正确設定
- 查看 Claude Code ログ中的サーバー接続エラー
