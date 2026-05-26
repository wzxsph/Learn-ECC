# 内置Hooks

## 概要

ECCプラグイン提供了丰富的内置Hook実装，位于`ECC/scripts/hooks/`ディレクトリ中。

## PreToolUse 内置钩子

### pre:bash:dispatcher

**タイプ**: PreToolUse
**matcher**: Bash
**描述**: Bash预检分发器，整合了品質検査、tmux提醒、gitプッシュ提醒和GateGuard検査

**機能**:
- 開発サーバー阻止器 - 阻止在tmux外実行`npm run dev`等コマンド
- Tmux提醒 - 推奨对长时间実行的コマンド（npm test、cargo build、docker）使用tmux
- Gitプッシュ提醒 - 提醒在`git push`前レビュー更改
- Pre-commit品質検査 - 実行品質検査：lint暂存ファイル、検証コミット情報フォーマット、检测console.log/debugger/密钥

**終了码**:
- 0: 警告但継続
- 2: 阻止実行

---

### pre:write:doc-file-warning

**タイプ**: PreToolUse
**matcher**: Write
**描述**: 警告作成非標準ドキュメントファイル

**機能**:
- 許可標準ファイル: README, CLAUDE, CONTRIBUTING, CHANGELOG, LICENSE, SKILL
- 許可ディレクトリ: docs/, skills/
- 警告その他.md/.txtファイル
- 跨平台路径处理

**終了码**: 0（仅警告）

---

### pre:edit-write:suggest-compact

**タイプ**: PreToolUse
**matcher**: Edit|Write
**描述**: 在逻辑间隔（约每50次ツール调用）推奨手动`/compact`

**機能**:
- トレースツール调用次数
- 在适当间隔提醒上下文圧縮
- 非阻塞，仅提供推奨

**終了码**: 0（推奨）

---

### pre:observe:continuous-learning

**タイプ**: PreToolUse
**matcher**: *
**描述**: 記録ツール意图以サポート持续学習信号

**機能**:
- 捕获ツール使用观察
- 用于パターン提取和分析
- 非同期実行，不阻塞

**終了码**: 0

---

### pre:governance-capture

**タイプ**: PreToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**描述**: 捕获治理事件（密钥、戦略违规、审批リクエスト）

**機能**:
- 检测秘密/密钥泄露
- 捕获戦略违规
- 記録审批リクエスト

**あり効化条件**: `ECC_GOVERNANCE_CAPTURE=1`

**終了码**: 0

---

### pre:config-protection

**タイプ**: PreToolUse
**matcher**: Write|Edit|MultiEdit
**描述**: 阻止修改linter/formatter設定ファイル

**機能**:
- 阻止.eslintrc、.prettierrc等修改
- 引导修正コード而非削弱設定

**終了码**: 0

---

### pre:mcp-health-check

**タイプ**: PreToolUse
**matcher**: *
**描述**: MCPツール実行前検査MCPサーバー健康状态

**機能**:
- 検査MCPサーバー状态
- 阻止对不健康MCPサーバー的调用

**終了码**: 0

---

### pre:edit-write:gateguard-fact-force

**タイプ**: PreToolUse
**matcher**: Edit|Write|MultiEdit
**描述**: 事实强制GateGuard：阻止各ファイル的首次編集/書き込み，要件在許可前进行调查

**機能**:
- 阻止首次編集
- 要件確認：インポート、データパターン、用户指令
- 确保あり充分的理由再修改

**終了码**: 0

---

## PostToolUse 内置钩子

### post:bash:dispatcher

**タイプ**: PostToolUse
**matcher**: Bash
**描述**: Bash后检分发器，用于ログ記録、PR和ビルド通知

**機能**:
- PRログ - `gh pr create`后記録PR URL和レビューコマンド
- ビルド分析 - ビルドコマンド后的后台分析（非同期，非阻塞）

**終了码**: 0

---

### post:quality-gate

**タイプ**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**描述**: ファイル編集后実行快速品質検査

**機能**:
- コード品質チェック
- 非同期実行
- 不阻塞編集操作

**終了码**: 0

---

### post:edit:design-quality-check

**タイプ**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**描述**: 警告フロントエンド編集偏离为通用模板外观的UI

**機能**:
- 检测UI設計品質
- 防止过于通用的模板外观

**終了码**: 0

---

### post:edit:accumulator

**タイプ**: PostToolUse
**matcher**: Edit|Write|MultiEdit
**描述**: 記録編集的JS/TSファイル路径，以便在Stop时バッチフォーマット化和タイプ検査

**機能**:
- 累积編集的ファイル列表
- 供stop:format-typecheck使用

**終了码**: 0

---

### post:edit:console-warn

**タイプ**: PostToolUse
**matcher**: Edit
**描述**: 編集后警告console.log语句

**機能**:
- 检测コード中的console.log
- 警告開発者クリーンアップデバッグコード

**終了码**: 0

---

### post:governance-capture

**タイプ**: PostToolUse
**matcher**: Bash|Write|Edit|MultiEdit
**描述**: 从ツール输出捕获治理事件

**機能**:
- 分析ツール输出
- 检测潜在問題

**あり効化条件**: `ECC_GOVERNANCE_CAPTURE=1`

**終了码**: 0

---

### post:session-activity-tracker

**タイプ**: PostToolUse
**matcher**: *
**描述**: 記録各セッション的ツール调用和ファイル活动

**機能**:
- トレースツール使用統計
- ファイル活动記録
- 用于ECC2指標

**終了码**: 0

---

### post:observe:continuous-learning

**タイプ**: PostToolUse
**matcher**: *
**描述**: 記録ツール结果以サポート持续学習信号

**機能**:
- 捕获ツール実行结果
- サポートパターン分析

**終了码**: 0

---

### post:ecc-metrics-bridge

**タイプ**: PostToolUse
**matcher**: *
**描述**: 维护実行中的セッション指標聚合

**機能**:
- トレースtoken和成本指標
- 供状态栏和上下文监视器使用

**終了码**: 0

---

### post:ecc-context-monitor

**タイプ**: PostToolUse
**matcher**: *
**描述**: 在上下文耗尽、高成本、范围 creep 或ツールループ时注入プロキシ警告

**機能**:
- 上下文使用監視
- 成本警告
- 范围 creep 检测
- ツールループ检测

**終了码**: 0

---

### post:mcp-health-check

**タイプ**: PostToolUseFailure
**matcher**: *
**描述**: トレース失敗的MCPツール调用，标记不健康的サーバー并尝试重连

**機能**:
- 失敗トレース
- 健康状态标记
- 自動重连尝试

**終了码**: 0

---

## Stop 内置钩子

### stop:format-typecheck

**タイプ**: Stop
**matcher**: *
**描述**: バッチフォーマット化（Biome/Prettier）和タイプ検査（tsc）本次応答中編集的すべてJS/TSファイル

**機能**:
- Prettier/Biomeフォーマット化
- TypeScriptタイプ検査
- 在Stop时実行一次，而非每次編集后

**終了码**: 0

**タイムアウト**: 300秒

---

### stop:check-console-log

**タイプ**: Stop
**matcher**: *
**描述**: 每次応答后検査修改ファイル中的console.log

**機能**:
- 扫描修改的ファイル
- レポートconsole.log使用情况

**終了码**: 0

---

### stop:session-end

**タイプ**: Stop
**matcher**: *
**描述**: 每次応答后持久化セッション状态（Stop携带transcript_path）

**機能**:
- セッション状态保存
- 上下文持久化

**終了码**: 0

---

### stop:evaluate-session

**タイプ**: Stop
**matcher**: *
**描述**: 评估セッション以提取可学習的パターン

**機能**:
- パターン识别
- 持续学習サポート
- 非同期実行

**終了码**: 0

---

### stop:cost-tracker

**タイプ**: Stop
**matcher**: *
**描述**: トレース各セッション的token和成本指標

**機能**:
- Token使用統計
- 成本估算
- 轻量级実行成本遥测标记

**終了码**: 0

---

### stop:desktop-notify

**タイプ**: Stop
**matcher**: *
**描述**: 在Claude応答时送信macOS/WSL桌面通知，パッケージ含タスク摘要

**機能**:
- 桌面通知
- タスク摘要表示

**終了码**: 0

---

## SessionStart 内置钩子

### session:start

**タイプ**: SessionStart
**matcher**: *
**描述**: 加载先前上下文并检测新セッション的パッケージマネージャー

**機能**:
- 加载あり界限的先前上下文
- プロジェクト状态检测
- パッケージマネージャー检测（npm/pnpm/yarn/bun）

**环境変数**:
- `ECC_SESSION_START_MAX_CHARS`: 控制额外上下文サイズ（デフォルト8000字符）
- `ECC_SESSION_START_CONTEXT=off`: 完全無効化额外上下文

**終了码**: 0

---

## PreCompact 内置钩子

### pre:compact

**タイプ**: PreCompact
**matcher**: *
**描述**: 在上下文圧縮前保存状态

**機能**:
- セッション状态持久化
- 为圧縮准备上下文

**終了码**: 0

---

## SessionEnd 内置钩子

### session:end:marker

**タイプ**: SessionEnd
**matcher**: *
**描述**: セッション終了ライフタイム标记

**機能**:
- ライフタイム事件标记
- クリーンアップログ

**終了码**: 0

---

## 内置钩子速查表

### PreToolUse 钩子

| ID | Matcher | 機能 | 阻塞 |
|----|---------|------|------|
| pre:bash:dispatcher | Bash | 品質/tmux/プッシュ/GateGuard検査 | 可阻塞 |
| pre:write:doc-file-warning | Write | ドキュメントファイル警告 | 否 |
| pre:edit-write:suggest-compact | Edit\|Write | 推奨圧縮 | 否 |
| pre:observe:continuous-learning | * | 持续学習观察 | 否 |
| pre:governance-capture | Bash\|Write\|Edit\|MultiEdit | 治理事件捕获 | 否 |
| pre:config-protection | Write\|Edit\|MultiEdit | 設定保护 | 否 |
| pre:mcp-health-check | * | MCP健康検査 | 可阻塞 |
| pre:edit-write:gateguard-fact-force | Edit\|Write\|MultiEdit | 首次編集GateGuard | 可阻塞 |

### PostToolUse 钩子

| ID | Matcher | 機能 | 非同期 |
|----|---------|------|------|
| post:bash:dispatcher | Bash | PRログ/ビルド通知 | は |
| post:quality-gate | Edit\|Write\|MultiEdit | 品質门検査 | は |
| post:edit:design-quality-check | Edit\|Write\|MultiEdit | 設計品質検査 | 否 |
| post:edit:accumulator | Edit\|Write\|MultiEdit | 編集累积器 | 否 |
| post:edit:console-warn | Edit | console.log警告 | 否 |
| post:governance-capture | Bash\|Write\|Edit\|MultiEdit | 治理事件捕获 | 否 |
| post:session-activity-tracker | * | セッション活动トレース | 否 |
| post:observe:continuous-learning | * | 持续学習观察 | は |
| post:ecc-metrics-bridge | * | 指標桥接 | 否 |
| post:ecc-context-monitor | * | 上下文監視 | 否 |
| post:mcp-health-check | * (PostToolUseFailure) | MCP健康検査 | 否 |

### Stop 钩子

| ID | Matcher | 機能 | タイムアウト |
|----|---------|------|------|
| stop:format-typecheck | * | バッチフォーマット化和タイプ検査 | 300s |
| stop:check-console-log | * | console.log検査 | 30s |
| stop:session-end | * | セッション状态持久化 | 10s |
| stop:evaluate-session | * | セッション评估 | 10s |
| stop:cost-tracker | * | 成本トレース | 10s |
| stop:desktop-notify | * | 桌面通知 | 10s |

### ライフタイム钩子

| ID | Event | 機能 |
|----|-------|------|
| session:start | SessionStart | 加载上下文和检测パッケージマネージャー |
| pre:compact | PreCompact | 圧縮前状态保存 |
| session:end:marker | SessionEnd | セッション終了标记 |
