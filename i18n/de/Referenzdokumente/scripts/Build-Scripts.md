# 构建脚本

本文档介绍 ECC 项目的构建和发布脚本。

---

## release.sh

**路径**: `scripts/release.sh`

插件版本升级和发布脚本。

### 功能

1. **版本验证**
   - 确认提供了 semver 格式版本号 (X.Y.Z 或 X.Y.Z-prerelease)
   - 确认当前分支是 main
   - 确认工作区干净

2. **文件更新**
   - 更新所有包/插件清单中的版本号
   - 更新文档中的版本引用
   - 更新 agent 配置

3. **验证构建**
   - 运行 OpenCode 构建
   - 运行构建测试
   - 运行插件清单测试

4. **Git 操作**
   - 暂存所有更改
   - 创建提交
   - 创建并推送标签

### 使用方法

```bash
# 发布新版本
./scripts/release.sh 1.5.0

# 预发布版本
./scripts/release.sh 2.0.0-rc.1
```

### 更新的文件

| 文件 | 更新内容 |
|------|----------|
| `package.json` | version 字段 |
| `package-lock.json` | version 和 packages[""].version |
| `AGENTS.md` | Version 行 |
| `docs/tr/AGENTS.md` | Sürüm 行 |
| `docs/zh-CN/AGENTS.md` | 版本 行 |
| `agent.yaml` | version 字段 |
| `VERSION` | 版本文件 |
| `.claude-plugin/plugin.json` | version 字段 |
| `.claude-plugin/marketplace.json` | version 字段 |
| `.agents/plugins/marketplace.json` | ecc 插件版本 |
| `.codex-plugin/plugin.json` | version 字段 |
| `.opencode/package.json` | version 字段 |
| `.opencode/package-lock.json` | version 和 packages[""].version |
| `.opencode/plugins/ecc-hooks.ts` | 横幅版本 |
| `README.md` | 版本表格行 |
| `README.zh-CN.md` | 版本表格行 |
| `docs/tr/README.md` | 最新发布标题 |
| `docs/pt-BR/README.md` | 最新发布标题 |
| `docs/zh-CN/README.md` | 最新发布标题 |
| `docs/SELECTIVE-INSTALL-ARCHITECTURE.md` | repoVersion 示例 |

### 前提条件

- Git 工作区必须干净
- 必须位于 main 分支
- 所有清单文件必须存在

---

## build-opencode.js

**路径**: `scripts/build-opencode.js`

构建 OpenCode 插件的 TypeScript 代码。

### 构建流程

1. 清理 `dist` 目录
2. 查找 TypeScript 编译器 (`typescript/bin/tsc`)
3. 使用项目根目录的 tsconfig.json 编译 `.opencode` 目录

### 使用方法

```bash
# 构建 OpenCode
node scripts/build-opencode.js

# 作为 release 的一部分自动运行
./scripts/release.sh 1.5.0
```

### 前提条件

需要已安装根目录的开发依赖,确保 TypeScript 编译器可用。

---

## 其他脚本

### gan-harness.sh

**路径**: `scripts/gan-harness.sh`

GAN (General Agent Network) 测试工具相关。

### orchestrate-codex-worker.sh

**路径**: `scripts/orchestrate-codex-worker.sh`

编排 Codex worker 进程。

### sync-ecc-to-codex.sh

**路径**: `scripts/sync-ecc-to-codex.sh`

同步 ECC 到 Codex。

---

## 发布工作流程

```
1. 确保在 main 分支且工作区干净
2. 运行发布脚本并提供版本号
3. 脚本自动完成:
   ├── 验证前提条件
   ├── 更新所有版本引用
   ├── 构建 OpenCode
   ├── 运行测试验证
   ├── 创建 Git 提交
   └── 推送标签
4. GitHub Actions 检测到标签自动发布
```
