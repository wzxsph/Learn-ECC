---
name: opensource-pipeline
description: "开源流水线：fork、脱敏、打包私有项目以实现安全公开发布。串联 3 个 agent（forker、sanitizer、packager）。触发词：'/opensource'、'open source this'、'make this public'、'prepare for open source'。"
origin: ECC
---

# 开源流水线 Skill

通过 3 阶段流水线安全地将任何项目开源：**Fork**（剥离密钥）→ **Sanitize**（验证清洁）→ **Package**（CLAUDE.md + setup.sh + README）。

## 激活时机

- 用户说"open source this project"或"make this public"
- 用户希望将私有仓库准备公开发布
- 用户需要在推送到 GitHub 前剥离密钥
- 用户调用 `/opensource fork`、`/opensource verify` 或 `/opensource package`

## 命令

| 命令 | 操作 |
|---------|--------|
| `/opensource fork PROJECT` | 完整流水线：fork + sanitize + package |
| `/opensource verify PROJECT` | 在现有仓库上运行 sanitizer |
| `/opensource package PROJECT` | 生成 CLAUDE.md + setup.sh + README |
| `/opensource list` | 显示所有已暂存的项目 |
| `/opensource status PROJECT` | 显示已暂存项目的报告 |

## 协议

### /opensource fork PROJECT

**完整流水线 — 主工作流程。**

#### 步骤 1：收集参数

解析项目路径。如果 PROJECT 包含 `/`，则视为路径（绝对或相对）。否则检查：当前工作目录、`$HOME/PROJECT`，然后询问用户。

```
SOURCE_PATH="<解析后的绝对路径>"
STAGING_PATH="$HOME/opensource-staging/${PROJECT_NAME}"
```

询问用户：
1. "哪个项目？"（如果未找到）
2. "许可证？（MIT / Apache-2.0 / GPL-3.0 / BSD-3-Clause）"
3. "GitHub 组织或用户名？"（默认：通过 `gh api user -q .login` 检测）
4. "GitHub 仓库名称？"（默认：项目名称）
5. "README 描述？"（分析项目以提供建议）

#### 步骤 2：创建暂存目录

```bash
mkdir -p $HOME/opensource-staging/
```

#### 步骤 3：运行 Forker Agent

生成 `opensource-forker` agent：

```
Agent(
  description="Fork {PROJECT} for open-source",
  subagent_type="opensource-forker",
  prompt="""
Fork project for open-source release.

Source: {SOURCE_PATH}
Target: {STAGING_PATH}
License: {chosen_license}

Follow the full forking protocol:
1. Copy files (exclude .git, node_modules, __pycache__, .venv)
2. Strip all secrets and credentials
3. Replace internal references with placeholders
4. Generate .env.example
5. Clean git history
6. Generate FORK_REPORT.md in {STAGING_PATH}/FORK_REPORT.md
"""
)
```

等待完成。读取 `{STAGING_PATH}/FORK_REPORT.md`。

#### 步骤 4：运行 Sanitizer Agent

生成 `opensource-sanitizer` agent：

```
Agent(
  description="Verify {PROJECT} sanitization",
  subagent_type="opensource-sanitizer",
  prompt="""
Verify sanitization of open-source fork.

Project: {STAGING_PATH}
Source (for reference): {SOURCE_PATH}

Run ALL scan categories:
1. Secrets scan (CRITICAL)
2. PII scan (CRITICAL)
3. Internal references scan (CRITICAL)
4. Dangerous files check (CRITICAL)
5. Configuration completeness (WARNING)
6. Git history audit

Generate SANITIZATION_REPORT.md inside {STAGING_PATH}/ with PASS/FAIL verdict.
"""
)
```

等待完成。读取 `{STAGING_PATH}/SANITIZATION_REPORT.md`。

**如果 FAIL：** 向用户展示发现的问题。询问："修复这些问题并重新扫描，还是中止？"
- 如果修复：应用修复，重新运行 sanitizer（最多 3 次重试 — 3 次 FAIL 后，展示所有发现并让用户手动修复）
- 如果中止：清理暂存目录

**如果 PASS 或 PASS WITH WARNINGS：** 继续步骤 5。

#### 步骤 5：运行 Packager Agent

生成 `opensource-packager` agent：

```
Agent(
  description="Package {PROJECT} for open-source",
  subagent_type="opensource-packager",
  prompt="""
Generate open-source packaging for project.

Project: {STAGING_PATH}
License: {chosen_license}
Project name: {PROJECT_NAME}
Description: {description}
GitHub repo: {github_repo}

Generate:
1. CLAUDE.md (commands, architecture, key files)
2. setup.sh (one-command bootstrap, make executable)
3. README.md (or enhance existing)
4. LICENSE
5. CONTRIBUTING.md
6. .github/ISSUE_TEMPLATE/ (bug_report.md, feature_request.md)
"""
)
```

#### 步骤 6：最终审查

向用户展示：
```
开源 Fork 已就绪: {PROJECT_NAME}

位置: {STAGING_PATH}
许可证: {license}
生成的文件:
  - CLAUDE.md
  - setup.sh (可执行)
  - README.md
  - LICENSE
  - CONTRIBUTING.md
  - .env.example ({N} 个变量)

脱敏结果: {sanitization_verdict}

后续步骤:
  1. 审查: cd {STAGING_PATH}
  2. 创建仓库: gh repo create {github_org}/{github_repo} --public
  3. 推送: git remote add origin ... && git push -u origin main

是否继续创建 GitHub 仓库？（是/否/先审查）
```

#### 步骤 7：GitHub 发布（需用户批准）

```bash
cd "{STAGING_PATH}"
gh repo create "{github_org}/{github_repo}" --public --source=. --push --description "{description}"
```

---

### /opensource verify PROJECT

独立运行 sanitizer。解析路径：如果 PROJECT 包含 `/`，则视为路径。否则检查 `$HOME/opensource-staging/PROJECT`、`$HOME/PROJECT`，然后当前目录。

```
Agent(
  subagent_type="opensource-sanitizer",
  prompt="Verify sanitization of: {resolved_path}. Run all 6 scan categories and generate SANITIZATION_REPORT.md."
)
```

---

### /opensource package PROJECT

独立运行 packager。询问"许可证？"和"描述？"，然后：

```
Agent(
  subagent_type="opensource-packager",
  prompt="Package: {resolved_path} ..."
)
```

---

### /opensource list

```bash
ls -d $HOME/opensource-staging/*/
```

显示每个项目及其流水线进度（FORK_REPORT.md、SANITIZATION_REPORT.md、CLAUDE.md 的存在情况）。

---

### /opensource status PROJECT

```bash
cat $HOME/opensource-staging/${PROJECT}/SANITIZATION_REPORT.md
cat $HOME/opensource-staging/${PROJECT}/FORK_REPORT.md
```

## 暂存目录布局

```
$HOME/opensource-staging/
  my-project/
    FORK_REPORT.md           # 来自 forker agent
    SANITIZATION_REPORT.md   # 来自 sanitizer agent
    CLAUDE.md                # 来自 packager agent
    setup.sh                 # 来自 packager agent
    README.md                # 来自 packager agent
    .env.example             # 来自 forker agent
    ...                      # 脱敏后的项目文件
```

## 反模式

- **绝不**在未经用户批准的情况下推送到 GitHub
- **绝不**跳过 sanitizer — 它是安全门
- **绝不**在 sanitizer FAIL 后不修复所有关键问题就继续
- **绝不**在暂存目录中留下 `.env`、`*.pem` 或 `credentials.json`

## 最佳实践

- 新发布时始终运行完整流水线（fork → sanitize → package）
- 暂存目录持续保留直到明确清理 — 用于审查
- 手动修复后重新运行 sanitizer 再发布
- 参数化密钥而非删除 — 保留项目功能

## 相关 Skills

参见 `security-review` 了解 sanitizer 使用的密钥检测模式。