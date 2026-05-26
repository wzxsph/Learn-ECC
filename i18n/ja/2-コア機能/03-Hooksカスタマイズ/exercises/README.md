# 演習：Hooks カスタマイズ

## 演習 1: PreToolUse Hook を作成

### 目標
最初の PreToolUse Hook を作成し、危険なコマンド警告を実装。

### シーン
ユーザーが危険なコマンド（例：`rm -rf`）を実行しようとした場合、警告提示を表示。

### 手順

1. **Hook 設定を設計**

   ```json
   {
     "name": "dangerous-command-warning",
     "matcher": {
       "tool": "Bash"
     },
     "condition": "command.includes('rm -rf') || command.includes('drop table')",
     "hooks": [
       {
         "type": "notification",
         "when": "before",
         "message": "警告: 危険なコマンドが検出されました！操作の安全性を確認してください。",
         "continue": true
       }
     ]
   }
   ```

2. **Hook ファイルを作成**
   プロジェクトで `hooks/dangerous-warning.json` を作成

3. **Hook をテスト**
   テストシーンを設計：

   | コマンド | 予想動作 |
   |------|----------|
   | `rm -rf /tmp/test` | 警告を表示 |
   | `rm -rf ./project` | 警告を表示 |
   | `ls -la` | 警告なし |
   | `git commit` | 警告なし |

4. **Hook 機能を拡張**
   より多くの危険なコマンド検出を追加试着：

   ```json
   "condition": "command.includes('rm -rf') || 
                 command.includes('drop table') || 
                 command.includes('truncate') ||
                 command.includes('shutdown')"
   ```

5. **ホワイトリストを追加**
   某些情况下では危険なコマンドを許可する必要があるため、ホワイトリストメカニズムを追加：

   ```json
   "condition": "dangerousCommand && !whitelisted"
   ```

### 完了基準
- [ ] PreToolUse Hook 設定ファイルを作成
- [ ] 危険なコマンド条件を定義
- [ ] テストシーンを設計
- [ ] 少なくとも5種類の危険なコマンド検出を追加

---

## 演習 2: PostToolUse Hook を作成

### 目標
PostToolUse Hook を作成し、テスト完了後の自動通知を実装。

### シーン
テスト実行完了後、自動的にテスト結果をチェックして通知を送信。

### 手順

1. **Hook 設定を設計**

   ```json
   {
     "name": "test-result-notifier",
     "matcher": {
       "tool": "Bash"
     },
     "condition": "command.includes('npm test') || command.includes('pytest')",
     "hooks": [
       {
         "type": "notification",
         "when": "after",
         "message": "テスト実行完了",
         "continue": true
       }
     ]
   }
   ```

2. **機能を強化：テスト結果を解析**
   JavaScript 処理スクリプト `scripts/parse-test-result.js` を作成:

   ```javascript
   module.exports = function parseTestResult(output) {
     // テスト出力を解析
     const passed = output.match(/(\d+) passed/);
     const failed = output.match(/(\d+) failed/);
     
     return {
       passed: passed ? parseInt(passed[1]) : 0,
       failed: failed ? parseInt(failed[1]) : 0,
       status: failed ? 'FAILED' : 'PASSED'
     };
   };
   ```

3. **完全な Hook 設定を作成**
   スクリプトを組み合わせて完全設定を作成：

   ```json
   {
     "name": "test-result-checker",
     "matcher": {
       "tool": "Bash"
     },
     "condition": "command.includes('npm test')",
     "hooks": [
       {
         "type": "command",
         "when": "after",
         "command": "node scripts/parse-test-result.js",
         "continue": true
       }
     ]
   }
   ```

4. **複数のテストフレームワーク适配を設計**
   異なるテストフレームワークをサポート：

   | フレームワーク | 出力キーワード |
   |------|----------|
   | Jest | `Tests:` |
   | Pytest | `passed` |
   | Go test | `ok` |
   | RSpec | `examples` |

5. **持続的統合シーンを追加**
   CI 環境での動作を設計：

   ```json
   {
     "condition": "isCI && testCommand",
     "hooks": [
       {
         "type": "command",
         "when": "after",
         "command": "node scripts/ci-reporter.js",
         "continue": true
       }
     ]
   }
   ```

### 完了基準
- [ ] PostToolUse Hook を作成
- [ ] テスト結果解析を実装
- [ ] 複数のテストフレームワークをサポート
- [ ] CI シーン処理を設計

---

## 演習 3: 品質ゲート Hook を構成

### 目標
完全な品質ゲートシステムを作成し、コード提出前に多次元チェックを実行。

### シーン
`git commit` 前に自動的に一系列の質チェックを実行し、コード品質を確保。

### 手順

1. **品質ゲートチェック項目を定義**

   | チェック項目 | チェック内容 | 失敗動作 |
   |--------|----------|----------|
   | テストカバレッジ | >= 80% | 提出を阻止 |
   | Lint チェック | エラーなし | 提出を阻止 |
   | セキュリティスキャン | 脆弱性なし | 警告+阻止 |
   | フォーマットチェック | 規範に準拠 | 自動修復 |

2. **Hook 実行フローを設計**

   ```
   git commit
      ↓
   [PreToolUse Hook]
      ↓
   品質チェックを実行
      ├── テストカバレッジチェック
      ├── Lint チェック  
      ├── セキュリティスキャン
      └── フォーマットチェック
      ↓
   [結果判断]
      ↓
   ├── 全部通過 → 提出を許可
   ├── 警告あり → 警告を表示、提出を継続
   └── エラーあり → 提出を阻止
   ```

3. **Hook 設定ファイルを作成**

   ```json
   {
     "name": "quality-gate",
     "matcher": {
       "tool": "Bash"
     },
     "condition": "command.includes('git commit')",
     "hooks": [
       {
         "type": "command",
         "when": "before",
         "command": "npm test && npm run lint && npm run security-scan",
         "continue": false
       }
     ]
   }
   ```

4. **チェックスクリプトを作成**

   `scripts/quality-gate.js`:

   ```javascript
   const { execSync } = require('child_process');
   
   function runChecks() {
     const checks = [
       { name: 'テスト', command: 'npm test', required: true },
       { name: 'Lint', command: 'npm run lint', required: true },
       { name: 'カバレッジ', command: 'npm run coverage', required: true },
       { name: 'セキュリティ', command: 'npm run security-scan', required: false }
     ];
   
     let allPassed = true;
     for (const check of checks) {
       try {
         execSync(check.command, { stdio: 'inherit' });
         console.log(`✓ ${check.name} 通過`);
       } catch (error) {
         console.error(`✗ ${check.name} 失敗`);
         if (check.required) allPassed = false;
       }
     }
     return allPassed;
   }
   
   process.exit(runChecks() ? 0 : 1);
   ```

5. **漸進的品質ゲートを追加**

   ```json
   {
     "name": "progressive-quality-gate",
     "stages": [
       { "name": "快速チェック", "commands": ["npm run lint"], "blockOnFail": true },
       { "name": "テスト", "commands": ["npm test"], "blockOnFail": true },
       { "name": "セキュリティスキャン", "commands": ["npm run security-scan"], "blockOnFail": false },
       { "name": "カバレッジ", "commands": ["npm run coverage"], "blockOnFail": true }
     ]
   }
   ```

6. **スキップメカニズムを構成**

   ```json
   {
     "condition": "command.includes('git commit') && !command.includes('--no-verify')",
     "message": "品質ゲートが有効です。--no-verify を使用してチェックをスキップ（不推奨）。"
   }
   ```

### 完了基準
- [ ] 完全なチェック項目リストを定義
- [ ] Hook 設定ファイルを作成
- [ ] チェックスクリプトを実装
- [ ] スキップメカニズムをサポート
- [ ] 使用方法を文書化

---

## 深入学習

- 完全な Hooks ドキュメントを阅读: [../../../参考ドキュメント/hooks/Hook类型.md](../../../参考ドキュメント/hooks/Hook类型.md)
- プロジェクト内の Hook 例を表示
- Hook をデバッグする方法を学習