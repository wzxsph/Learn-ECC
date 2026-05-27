# Hookタイプ

## 概要

ECC Hooksシステムはイベント驱动的自動化机制，在Claude Codeツール実行的前后觸發，使用强制コード品質、早期发现エラー和自動化重复検査。

```
ユーザーリクエスト → Claude选择ツール → PreToolUse钩子実行 → ツール実行 → PostToolUse钩子実行
```

## 钩子タイプ详解

### 1. PreToolUse 钩子

#### タイプ説明
PreToolUse钩子在ツール実行**之前**実行，可能阻止（exit code 2）或警告（stderr输出但不阻止）。

#### 觸發时机
- 在任意ツール（Edit、Write、Bash、Read等）実行之前
- 适使用すべてツール操作的前置検査

#### 參數説明
```typescript
interface HookInput {
  tool_name: string;          // ツール名前: "Bash", "Edit", "Write", "Read"等
  tool_input: {
    command?: string;         // Bash: 要実行的コマンド
    file_path?: string;        // Edit/Write/Read: 目標ファイルパス
    old_string?: string;       // Edit: 被置換的文本
    new_string?: string;       // Edit: 置換文本
    content?: string;          // Write: ファイル內容
  };
}
```

#### 終了码
| 終了码 | 含义 | 行為 |
|--------|------|------|
| 0 | 成功 | 継続実行，不输出警告 |
| 2 | 阻止 | 阻止ツール実行 |
| その他非零 | エラー | 記録ログ但不阻止 |

#### 使用サンプル

**開発サーバー阻止器（阻止在tmux外実行npm run dev）：**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "阻止在tmux外実行開発サーバー"
}
```

**ドキュメントファイル警告（阻止非標準.mdファイル）：**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "警告非標準ドキュメントファイル的作成"
}
```

#### 注意事项
- PreToolUseは**阻塞型**钩子，must快速実行（<200ms）
- 不要在PreToolUse钩子中进行網路調用
- 阻止操作（exit 2）应谨慎使用，仅使用重要セキュリティ検査

---

### 2. PostToolUse 钩子

#### タイプ説明
PostToolUse钩子在ツール実行**之后**実行，可能分析输出但できない阻止ツール実行。

#### 觸發时机
- 在任意ツール完了之后実行
- 可能訪問ツール的输出結果

#### 參數説明
```typescript
interface HookInput {
  tool_name: string;          // ツール名前
  tool_input: {               // 與PreToolUse相同
    command?: string;
    file_path?: string;
    old_string?: string;
    new_string?: string;
    content?: string;
  };
  tool_output?: {             // 仅在PostToolUse中可用
    output?: string;          // コマンド/ツール的输出
  };
}
```

#### 終了码
| 終了码 | 含义 |
|--------|------|
| 0 | 成功，継続実行 |
| 非零 | エラー，記録ログ但不阻止ツール実行 |

#### 使用サンプル

**PRログ記録（gh pr create后記録PR URL）：**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "PR作成后記録URL和レビューコマンド"
}
```

**品質门検査（編集后実行品質検査）：**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "ファイル編集后実行品質门検査"
}
```

#### 注意事项
- PostToolUse钩子**できない阻止**ツール実行
- 可能設定為非同期（async: true）以避免阻塞
- 非同期钩子应在30秒内完了

---

### 3. Stop 钩子

#### タイプ説明
Stop钩子在每次Claude応答后実行，使用バッチ處理、最终検査和セッション狀態持久化。

#### 觸發时机
- 在各ユーザーリクエスト處理完了、Claude生成応答后
- 发生在セッション的最後1応答之后

#### 參數説明
Stop钩子受信與PostToolUse相同的输入，含む含完整的ツール調用历史。

#### 使用サンプル

**控制台ログ検査（応答后検査修改ファイル中的console.log）：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "検査修改ファイル中は否ありconsole.log"
}
```

**バッチフォーマット化和タイプ検査：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/stop-format-typecheck.js",
    "timeout": 300
  }],
  "description": "バッチフォーマット化すべて編集的JS/TSファイル並进行タイプ検査"
}
```

**セッション摘要持久化：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "儲存セッション狀態"
}
```

#### 注意事项
- Stop钩子在每次応答后実行，适合做バッチ操作
- 可能使用asyncパターン进行非阻塞操作
- フォーマット化/タイプ検査等适合在Stop时バッチ実行

---

### 4. SessionStart 钩子

#### タイプ説明
SessionStart钩子在セッションライフタイム開始时觸發，使用載入先前上下文和检测プロジェクト狀態。

#### 觸發时机
- 新セッション開始时
- Claude Code起動或作成新セッション时

#### 使用サンプル

**載入先前上下文並检测含むマネージャー：**
```json
{
  "event": "SessionStart",
  "id": "session:start",
  "script": "scripts/hooks/session-start-bootstrap.js",
  "purpose": "在セッション開始时載入あり界限的先前上下文並检测プロジェクト狀態",
  "blocking": false
}
```

#### 注意事项
- ライフタイム钩子，must快速完了（<30秒）
- 可能通過`ECC_SESSION_START_MAX_CHARS`環境変数控制额外上下文サイズ
- 可能用`ECC_SESSION_START_CONTEXT=off`完全無効化

---

### 5. SessionEnd 钩子

#### タイプ説明
SessionEnd钩子在セッション終了时觸發，使用持久化セッション終了摘要和クリーンアップ。

#### 觸發时机
- セッション終了时
- 當セッション transcript 元データ可用时

#### 使用サンプル

**セッション終了标记：**
```json
{
  "event": "SessionEnd",
  "id": "session:end",
  "script": "scripts/hooks/session-end.js",
  "purpose": "當transcript元データ可用时持久化セッション終了摘要",
  "blocking": false
}
```

#### 注意事项
- 非同期実行為主，不应阻塞セッション終了
- ライフタイム标记使用指標收集和クリーンアップ

---

### 6. PreCompact 钩子

#### タイプ説明
PreCompact钩子在上下文圧縮前觸發，使用儲存狀態。

#### 觸發时机
- 在上下文圧縮/精简之前
- 當上下文視窗接近满时

#### 使用サンプル

**上下文圧縮前儲存狀態：**
```json
{
  "event": "PreCompact",
  "id": "pre:compact",
  "script": "scripts/hooks/pre-compact.js",
  "purpose": "在上下文圧縮前持久化セッション狀態",
  "blocking": false
}
```

#### 注意事项
- 适合儲存长タスク的狀態
- 非阻塞，不影响圧縮流程

---

### 7. PostToolUseFailure 钩子

#### タイプ説明
PostToolUseFailure钩子在ツール実行失敗后觸發。

#### 觸發时机
- 任意ツール調用失敗后

#### 使用サンプル

**MCP健康検査（トレース失敗的MCP調用）：**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/mcp-health-check.js"
  }],
  "description": "トレース失敗的MCPツール調用，标记不健康的サーバー並尝试重连"
}
```

#### 注意事项
- 可使用再試行逻辑或健康狀態トレース
- 不会改变ツール失敗的狀態

---

## 钩子タイプ對比表

| タイプ | 觸發时机 | 能否阻止 | 能否阻塞 | 典型用途 |
|------|----------|----------|----------|----------|
| PreToolUse | ツール実行前 | は（exit 2） | は（同期） | 品質検査、セキュリティ検証 |
| PostToolUse | ツール実行后 | 否 | オプション（async） | ログ記録、分析 |
| Stop | 每次応答后 | 否 | オプション（async） | バッチフォーマット化、セッション摘要 |
| SessionStart | セッション開始 | 否 | 否 | 載入上下文、检测プロジェクト |
| SessionEnd | セッション終了 | 否 | 否 | 持久化摘要、クリーンアップ |
| PreCompact | 圧縮前 | 否 | 否 | 狀態儲存 |
| PostToolUseFailure | ツール失敗后 | 否 | 否 | 健康検査、再試行 |

---

## 環境変数控制

可能通過環境変数控制钩子行為：

```bash
# minimal | standard | strict (デフォルト: standard)
export ECC_HOOK_PROFILE=standard

# 無効化特定钩子ID（逗号分隔）
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# 無効化GateGuard
export ECC_GATEGUARD=off

# 控制SessionStart额外上下文サイズ（デフォルト: 8000字符）
export ECC_SESSION_START_MAX_CHARS=4000

# 完全無効化SessionStart额外上下文
export ECC_SESSION_START_CONTEXT=off
```
