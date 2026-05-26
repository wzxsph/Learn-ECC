# 03 - Hooks カスタマイズ

ECC [Hook](../../参考ドキュメント/hooks/Hook类型.md) システムを学習し、ワークフロー自動化を実現します。

## 概要

ECC Hooks システムはイベント駆動の自動化メカニズムで、Claude Codeツール実行の前後でトリガーされ、コード品質の強制、早期エラー発見、自動化繰り返しチェックに使用されます。

```
ユーザー要求 → Claudeがツールを選択 → PreToolUseフック実行 → ツール実行 → PostToolUseフック実行
```

---

## 1. Hook タイプ详解

### 1.1 PreToolUse フック

ツール実行**前**に実行され、（exit code 2）或警告（stderr出力但不阻止）を生成できます。

#### トリガータイミング
- 任意のツール（Edit、Write、Bash、Readなど）の実行前
- すべてのツール操作の前処理チェックに適用

#### 終了コード
| 終了コード | 意味 | 動作 |
|--------|------|------|
| 0 | 成功 | 繼續執行，不輸出警告 |
| 2 | 阻止 | ツール実行を阻止 |
| 其他非零 | エラー | ログを記録但不阻止 |

#### 使用例

**開発サーバーブロッカー:**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/dev-server-check.js"
  }],
  "description": "tmux外で開発サーバーを実行することを阻止"
}
```

**ドキュメントファイル警告:**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/doc-file-warning.js"
  }],
  "description": "非標準ドキュメントファイルの作成を警告"
}
```

### 1.2 PostToolUse フック

ツール実行**後**に実行され、出力を分析できますがツール実行を阻止できません。

#### トリガータイミング
- 任意のツール完了後に実行
- ツールの出力結果にアクセス可能

#### 使用例

**PR ログ記録:**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/pr-logger.js",
    "async": true,
    "timeout": 30
  }],
  "description": "PR作成後URLとレビューコマンドを記録"
}
```

**品質ゲートチェック:**
```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/quality-gate.js",
    "async": true,
    "timeout": 30
  }],
  "description": "ファイル編集後に品質ゲートチェックを実行"
}
```

### 1.3 Stop フック

Claudeの每次応答後に実行され、バッチ処理、最終チェック、セッション状態永続化に使用されます。

#### 使用例

**コンソールログチェック:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/check-console-log.js"
  }],
  "description": "変更されたファイルにconsole.logがあるかどうかチェック"
}
```

**セッション要約永続化:**
```json
{
  "matcher": "*",
  "hooks": [{
    "type": "command",
    "command": "node scripts/hooks/session-end.js",
    "async": true,
    "timeout": 10
  }],
  "description": "セッション状態を保存"
}
```

### 1.4 SessionStart / SessionEnd フック

ライフサイクルフックで、セッション開始と終了時のコンテキストロードと状態永続化に使用されます。

---

## 2. Hook タイプ比較表

| タイプ | トリガータイミング | 阻止可能 | 阻塞可能 | 典型的な用途 |
|------|----------|----------|----------|----------|
| PreToolUse | ツール実行前 | はい（exit 2） | はい（同期） | 品質チェック、セキュリティ検証 |
| PostToolUse | ツール実行後 | いいえ | オプション（async） | ログ記録、分析 |
| Stop | 每次応答後 | いいえ | オプション（async） | バッチフォーマット、セッション要約 |
| SessionStart | セッション開始 | いいえ | いいえ | コンテキストロード、项目検出 |
| SessionEnd | セッション終了 | いいえ | いいえ | 要約永続化、清理 |

---

## 3. カスタム Hook 開発

### 3.1 基礎構造

```javascript
// my-custom-hook.js
let data = '';
process.stdin.on('data', chunk => data += chunk);
process.stdin.on('end', () => {
  const input = JSON.parse(data);

  // ツール情報にアクセス
  const toolName = input.tool_name;        // "Edit", "Bash", "Write"など
  const toolInput = input.tool_input;      // ツール特定パラメータ
  const toolOutput = input.tool_output;    // PostToolUseでのみ利用可能

  // 警告（非阻塞）：stderrに書き込み
  console.error('[Hook] 警告情報');

  // 阻止（PreToolUseのみ）：exit code 2
  // process.exit(2);

  // 常に元のデータをstdoutに出力
  console.log(data);
});
```

### 3.2 HookInput インターフェース

```typescript
interface HookInput {
  tool_name: string;          // ツール名称
  tool_input: {               // ツール入力パラメータ
    command?: string;         // Bash: コマンド
    file_path?: string;       // Edit/Write/Read: ファイルパス
    old_string?: string;      // Edit: 置換されたテキスト
    new_string?: string;      // Edit: 置換テキスト
    content?: string;         // Write: ファイル内容
  };
  tool_output?: {             // PostToolUseのみ
    output?: string;          // コマンド/ツール出力
  };
}
```

### 3.3 重要ルール: exit 0 on 非重要エラー

**重要**: すべてのフックは非重要エラー時にexit 0で終了し、予期しないツール実行の阻塞を回避する必要があります。

| 終了コード | 意味 | 使用シーン |
|--------|------|----------|
| 0 | 成功 | 継続実行、または非重要警告 |
| 2 | 阻止 | PreToolUseの重要阻塞のみに使用 |
| 其他非零 | エラー | ログのみ記録、**使用しない** |

### 3.4 正しいエラー処理

```javascript
// エラー例 - 这样做わない
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // 処理ロジック
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    process.exit(1);  // エラー：ツールを阻塞
  }
  console.log(data);
});

// 正しい例 - 这样做う
process.stdin.on('end', () => {
  try {
    const input = JSON.parse(data);
    // 処理ロジック
  } catch (e) {
    console.error('[Hook] Error:', e.message);
    // 非重要エラー、exit 0で継続実行
    console.log(data);
    return;
  }
  console.log(data);
});
```

---

## 4. 一般的なフックレシピ

### 4.1 TODO/FIXME コメントの警告

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const ns=i.tool_input?.new_string||'';if(/TODO|FIXME|HACK/.test(ns)){console.error('[Hook] New TODO/FIXME added - consider creating an issue')}console.log(d)})\""
  }],
  "description": "TODO/FIXMEコメントの追加を警告"
}
```

### 4.2 大きすぎるファイルの作成を阻止

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const c=i.tool_input?.content||'';const lines=c.split('\\n').length;if(lines>800){console.error('[Hook] BLOCKED: File exceeds 800 lines ('+lines+' lines)');process.exit(2)}console.log(d)})\""
  }],
  "description": "800行を超えるファイルの作成を阻止"
}
```

### 4.3 ruffでPythonファイルを自動フォーマット

```json
{
  "matcher": "Edit",
  "hooks": [{
    "type": "command",
    "command": "node -e \"let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/\\.py$/.test(p)){const{execFileSync}=require('child_process');try{execFileSync('ruff',['format',p],{stdio:'pipe'})}catch(e){}}console.log(d)})\""
  }],
  "description": "編集後にruffでPythonファイルを自動フォーマット"
}
```

### 4.4 新しいソースファイルにテストを添付要求

```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "node -e \"const fs=require('fs');let d='';process.stdin.on('data',c=>d+=c);process.stdin.on('end',()=>{const i=JSON.parse(d);const p=i.tool_input?.file_path||'';if(/src\\/.*\\.(ts|js)$/.test(p)&&!/\\.test\\.|\\.spec\\./.test(p)){const testPath=p.replace(/\\.(ts|js)$/,'.test.$1');if(!fs.existsSync(testPath)){console.error('[Hook] No test file found for: '+p)}}console.log(d)})\""
  }],
  "description": "新しいソースファイル追加時にテスト作成を促す"
}
```

---

## 5. 設定管理

### 5.1 run-with-flags.js の使用

```bash
# run-with-flags.js ラッパーを使用
node scripts/hooks/run-with-flags.js my-hook scripts/hooks/my-hook.js standard,strict
```

### 5.2 環境変数制御

```bash
# minimal | standard | strict (デフォルト: standard)
export ECC_HOOK_PROFILE=standard

# 特定のフックを無効化（カンマ区切り）
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# SessionStart追加コンテキストサイズを制御（デフォルト: 8000文字）
export ECC_SESSION_START_MAX_CHARS=4000

# SessionStart追加コンテキストを完全に無効化
export ECC_SESSION_START_CONTEXT=off
```

### 5.3 hooks.json への統合

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "node scripts/hooks/my-new-hook.js"
          }
        ],
        "description": "私の新しいフック説明",
        "id": "pre:custom:my-new-hook"
      }
    ]
  }
}
```

---

## 6. 非同期フック

メインフローを阻塞すべきでないフック（バックグラウンド分析など）の場合：

```json
{
  "matcher": "Edit|Write|MultiEdit",
  "hooks": [{
    "type": "command",
    "command": "node my-slow-hook.js",
    "async": true,
    "timeout": 30
  }]
}
```

**非同期フックに関する注意事項**:
- 非同期フックはバックグラウンドで実行
- ツール実行を阻塞できない
- 30秒以内に完了すべき
- 適用対象：ログ、分析、テレメトリ

---

## 7. 性能ベストプラクティス

| フックタイプ | 性能要件 |
|----------|----------|
| PreToolUse | <200ms |
| PostToolUse（同期） | <1秒 |
| 非同期フック | <30秒 |

### 阻塞操作の回避

```javascript
// 悪い：同期ファイル読み取り
const content = fs.readFileSync('large-file.txt', 'utf8');

// 良い：非同期または遅延ロード
fs.readFile('large-file.txt', 'utf8', (err, content) => {
  // 処理
});
```

---

## 8. フックのデバッグ

### デバッグ出力の追加

```javascript
const DEBUG = process.env.DEBUG_HOOKS === '1';

process.stdin.on('end', () => {
  if (DEBUG) {
    console.error('[DEBUG] Received input:', data);
  }
  // 処理ロジック
  if (DEBUG) {
    console.error('[DEBUG] Sending output');
  }
  console.log(data);
});
```

### 一般的な問題

| 問題 | 原因 | 解決策 |
|------|------|------|
| ツールが意図せず阻止された | フックがゼロ以外的exit | exit 0とstderr警告を使用するように変更 |
| 、ハング | フックが終了しない | 常に出力をstdoutに必ず出力 |
| データ破損 | stdoutに非JSON出力 | 元のデータのみを出力 |
| 性能問題 | フックが遅すぎる | 最適化または非同期に変更 |

---

## 演習

[演習](./exercises/演習.md) のタスクを完了します。

---

[コア能力ディレクトリに戻る](../README.md)