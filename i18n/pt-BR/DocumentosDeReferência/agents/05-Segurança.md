# 安全类 Agent

安全类 Agent 专门负责安全漏洞检测、合规性审查和敏感信息保护。

## Agent 列表

| Agent 名称 | 用途 | 使用模型 | 核心工具 |
|------------|------|----------|----------|
| security-reviewer | 安全漏洞检测和修复专家 | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| silent-failure-hunter | 静默失败检测专家 | sonnet | Read, Grep, Glob, Bash |
| healthcare-reviewer | 医疗应用代码审查（PHI/临床安全） | opus | Read, Grep, Glob |
| opensource-sanitizer | 开源项目泄露检查专家 | sonnet | Read, Grep, Glob, Bash |

---

## security-reviewer

### 名称和用途
安全漏洞检测和修复专家。在编写处理用户输入、认证、API 端点或敏感数据的代码后主动使用。标记 secrets、SSRF、注入、不安全加密和 OWASP Top 10 漏洞。

### 能力说明
- 漏洞检测 - 识别 OWASP Top 10 和常见安全问题
- Secrets 检测 - 发现硬编码 API 密钥、密码、tokens
- 输入验证 - 确保所有用户输入被正确清理
- 认证/授权 - 验证适当的访问控制
- 依赖安全 - 检查有漏洞的 npm 包
- 安全Melhores-Práticas - 强制执行安全编码模式

### 适用场景
- 新 API 端点
- 认证代码变更
- 用户输入处理
- 数据库查询变更
- 文件上传
- 支付代码
- 外部 API 集成
- 依赖更新

### 使用的工具列表
- Read: 读取代码
- Write: 写入安全修复
- Edit: 编辑安全修复
- Bash: 运行 npm audit, eslint
- Grep: 搜索敏感模式
- Glob: 查找文件

### 与其他 Agent 的协作方式
- code-reviewer 共享代码质量检查
- healthcare-reviewer 处理医疗特定安全问题
- opensource-sanitizer 处理开源发布前检查

### 初始扫描

```bash
npm audit --audit-level=high
npx eslint . --plugin security
```

搜索硬编码 secrets，审查高风险区域：auth、API 端点、DB 查询、文件上传、支付、webhooks。

### OWASP Top 10 检查

1. **注入** - 查询是否参数化？用户输入是否清理？ORM 是否安全使用？
2. **认证破坏** - 密码是否哈希 (bcrypt/argon2)？JWT 是否验证？会话是否安全？
3. **敏感数据** - HTTPS 是否强制？Secrets 是否在 env vars？PII 是否加密？日志是否清理？
4. **XXE** - XML 解析器是否安全配置？外部实体是否禁用？
5. **访问控制破坏** - 每个路由是否检查 auth？CORS 是否正确配置？
6. **安全配置错误** - 默认凭据是否更改？生产环境 debug 模式是否关闭？安全头是否设置？
7. **XSS** - 输出是否转义？CSP 是否设置？框架是否自动转义？
8. **不安全反序列化** - 用户输入是否安全反序列化？
9. **已知漏洞** - 依赖是否最新？npm audit 是否通过？
10. **日志和监控不足** - 安全事件是否记录？是否配置警报？

### 代码模式审查

立即标记这些模式：

| 模式 | 严重性 | 修复 |
|------|--------|------|
| 硬编码 secrets | CRITICAL | 使用 `process.env` |
| 用户输入的 shell 命令 | CRITICAL | 使用安全 API 或 execFile |
| 字符串拼接 SQL | CRITICAL | 参数化查询 |
| `innerHTML = userInput` | HIGH | 使用 `textContent` 或 DOMPurify |
| `fetch(userProvidedUrl)` | HIGH | 白名单允许的域名 |
| 明文密码比较 | CRITICAL | 使用 `bcrypt.compare()` |
| 路由无 auth 检查 | CRITICAL | 添加认证中间件 |
| 无锁的余额检查 | CRITICAL | 在事务中使用 `FOR UPDATE` |
| 无速率限制 | HIGH | 添加 `express-rate-limit` |
| 记录密码/secrets | MEDIUM | 清理日志输出 |

### 关键原则

1. **纵深防御** - 多层Segurança
2. **最小权限** - 所需最小权限
3. **安全失败** - 错误不应暴露数据
4. **不信任输入** - 验证和清理一切
5. **定期更新** - 保持依赖最新

### 常见误报

- `.env.example` 中的环境变量（不是实际 secrets）
- 测试文件中的测试凭据（如果明确标记）
- 实际公开的公钥 API 密钥
- 用于校验和的 SHA256/MD5（不是密码）

**始终在标记前验证上下文。**

### 紧急响应

如果发现 CRITICAL 漏洞：
1. 详细报告记录
2. 立即通知项目负责人
3. 提供安全代码示例
4. 验证修复有效
5. 如果凭据暴露则轮换 secrets

---

## silent-failure-hunter

### 名称和用途
审查代码中的静默失败、被吞掉的错误、错误的回退和缺失的错误传播。

### 能力说明
- 空 catch 块检测
- 不充分日志检测
- 危险回退检测
- 错误传播问题检测
- 缺失错误处理检测

### 适用场景
- 代码审查时
- 发现奇怪 bug 时
- 管道看似绿色但跳过数据时
- 异常处理审查时

### 使用的工具列表
- Read: 读取代码
- Grep: 搜索错误处理模式
- Glob: 查找源文件
- Bash: 运行测试

### 与其他 Agent 的协作方式
- code-reviewer 共享代码质量检查
- security-reviewer 处理安全相关问题
- mle-reviewer 处理 ML 管道问题

### 狩猎目标

#### 1. 空 Catch 块
- `catch {}` 或被忽略的异常
- 转换为 `null` / 空数组但无上下文的错误

#### 2. 不充分日志
- 日志上下文不足
- 错误严重性不对
- log-and-forget 处理

#### 3. 危险回退
- 隐藏真实失败的默认值
- `.catch(() => [])`
- 优雅但使下游 bug 更难诊断的路径

#### 4. 错误传播问题
- 丢失的堆栈跟踪
- 泛型 rethrows
- 缺失 async 处理

#### 5. 缺失错误处理
- 网络/文件/数据库路径无超时或错误处理
- 事务工作无回滚

### 输出格式

每个发现：
- 位置
- 严重性
- 问题
- 影响
- 修复建议

---

## healthcare-reviewer

### 名称和用途
审查医疗应用代码的临床安全、CDSS 准确性、PHI 合规性和医疗数据完整性。专为 EMR/EHR、临床决策支持和健康信息系统而设计。

### 能力说明
- CDSS 准确性 - 验证药物相互作用逻辑、剂量验证规则和临床评分实现
- PHI/PII 保护 - 扫描患者数据在日志、错误、响应、URL 和客户端存储中的暴露
- 临床数据完整性 - 确保审计跟踪、锁定记录和级联保护
- 医疗数据正确性 - 验证 ICD-10/SNOMED 映射、实验室参考范围和药物数据库条目
- 集成合规性 - 验证 HL7/FHIR 消息处理和错误恢复

### 适用场景
- EMR/EHR 系统开发
- 临床决策支持系统
- 健康信息系统
- 医疗数据处理应用

### 使用的工具列表
- Read: 读取医疗代码
- Grep: 搜索 PHI 模式
- Glob: 查找医疗相关文件

### 与其他 Agent 的协作方式
- security-reviewer 处理通用安全漏洞
- code-reviewer 处理代码质量问题
- silent-failure-hunter 处理静默失败问题

### 关键检查

#### CDSS 引擎
- [ ] 所有药物相互作用对产生正确警报（双向）
- [ ] 剂量验证规则在超出范围值时触发
- [ ] 临床评分匹配发布规范
- [ ] 无假阴性（漏掉的相互作用 = 患者安全事件）
- [ ] 格式错误的输入产生错误，而非静默通过

#### PHI 保护
- [ ] 患者数据不在 `console.log`、`console.error` 或错误消息中
- [ ] PHI 不在 URL 参数或查询字符串中
- [ ] PHI 不在浏览器 localStorage/sessionStorage 中
- [ ] 客户端代码中无 `service_role` 密钥
- [ ] 所有患者数据表启用 RLS
- [ ] 跨设施数据隔离已验证

#### 临床工作流
- [ ] 诊疗锁定防止编辑（仅附录）
- [ ] 每次临床数据 CRUD 都有审计跟踪条目
- [ ] 关键警报不可忽略（不是 toast 通知）
- [ ] 临床医生继续进行关键警报时记录覆盖原因
- [ ] 红旗症状触发可见警报

#### 数据完整性
- [ ] 患者记录无 CASCADE DELETE
- [ ] 并发编辑检测（乐观锁或冲突解决）
- [ ] 临床表无孤立记录
- [ ] 时间戳使用一致的时区

### 输出格式

```
## Healthcare Review: [module/feature]

### Patient Safety Impact: [CRITICAL / HIGH / MEDIUM / LOW / NONE]

### Clinical Accuracy
- CDSS: [检查通过/失败]
- Drug DB: [验证/问题]
- Scoring: [匹配规范/偏离]

### PHI Compliance
- 暴露向量检查: [列表]
- 发现问题: [列表或无]

### Issues
1. [PATIENT SAFETY / CLINICAL / PHI / TECHNICAL] 描述
   - Impact: [潜在危害或暴露]
   - Fix: [所需变更]

### Verdict: [SAFE TO DEPLOY / NEEDS FIXES / BLOCK — PATIENT SAFETY RISK]
```

### 规则

- 对临床准确性有疑问时，标记为 NEEDS REVIEW - 绝不批准不确定的临床逻辑
- 一次漏掉的药物相互作用比一百个假警报更糟糕
- PHI 暴露始终是 CRITICAL 严重性，无论泄露多小
- 绝不批准静默捕获 CDSS 错误的代码

---

## opensource-sanitizer

### 名称和用途
在发布前验证开源 fork 已完全清理。扫描泄露的 secrets、PII、内部引用和危险文件。使用 20+ 正则表达式模式。生成 PASS/FAIL/PASS-WITH-WARNINGS 报告。

### 能力说明
- Secrets 扫描 - 扫描 API 密钥、密码、tokens
- PII 扫描 - 扫描邮箱、IP 地址
- 内部引用扫描 - 扫描绝对路径、内部域名
- 危险文件检查 - 检查 .env、credentials.json 等
- 配置完整性验证 - 验证 .env.example
- Git 历史审计 - 检查泄露的凭据

### 适用场景
- 开源 fork 发布前
- 第三方代码审计前
- 代码发布前的安全检查

### 使用的工具列表
- Read: 读取文件
- Grep: 搜索敏感模式
- Glob: 查找文件
- Bash: 运行 git log 等

### 与其他 Agent 的协作方式
- security-reviewer 处理通用安全漏洞
- code-reviewer 处理代码质量问题
- doc-updater 更新文档

### 工作流

#### 步骤 1: Secrets 扫描 (CRITICAL)

扫描每个文本文件（排除 `node_modules`、`.git`、`__pycache__`、`*.min.js`、二进制文件）：

```
# API 密钥
pattern: [A-Za-z0-9_]*(api[_-]?key|apikey|api[_-]?secret)[A-Za-z0-9_]*\s*[=:]\s*['"]?[A-Za-z0-9+/=_-]{16,}

# AWS
pattern: AKIA[0-9A-Z]{16}
pattern: (?i)(aws_secret_access_key|aws_secret)\s*[=:]\s*['"]?[A-Za-z0-9+/=]{20,}

# 数据库 URL（带凭据）
pattern: (postgres|mysql|mongodb|redis)://[^:]+:[^@]+@[^\s'"]+

# JWT tokens (3-segment)
pattern: eyJ[A-Za-z0-9_-]{20,}\.eyJ[A-Za-z0-9_-]{20,}\.[A-Za-z0-9_-]+

# 私钥
pattern: -----BEGIN\s+(RSA\s+|EC\s+|DSA\s+|OPENSSH\s+)?PRIVATE KEY-----

# GitHub tokens
pattern: gh[pousr]_[A-Za-z0-9_]{36,}
pattern: github_pat_[A-Za-z0-9_]{22,}
```

#### 步骤 2: PII 扫描 (CRITICAL)

```
# 个人邮箱（不是通用邮箱如 noreply@、info@）
pattern: [a-zA-Z0-9._%+-]+@(gmail|yahoo|hotmail|outlook|protonmail|icloud)\.(com|net|org)

# 私有 IP 地址（表示内部基础设施）
pattern: (192\.168\.\d+\.\d+|10\.\d+\.\d+\.\d+|172\.(1[6-9]|2\d|3[01])\.\d+\.\d+)

# SSH 连接字符串
pattern: ssh\s+[a-z]+@[0-9.]+
```

#### 步骤 3: 内部引用扫描 (CRITICAL)

```
# 绝对路径到特定用户主目录
pattern: /home/[a-z][a-z0-9_-]*/
pattern: /Users/[A-Za-z][A-Za-z0-9_-]*/
pattern: C:\\Users\\[A-Za-z]

# 内部 secret 文件引用
pattern: \.secrets/
pattern: source\s+~/\.secrets/
```

#### 步骤 4: 危险文件检查 (CRITICAL)

验证这些**不存在**：
```
.env (任何变体: .env.local, .env.production, .env.*.local)
*.pem, *.key, *.p12, *.pfx, *.jks
credentials.json, service-account*.json
.secrets/, secrets/
.claude/settings.json
sessions/
*.map (source maps 暴露原始源结构)
node_modules/, __pycache__/, .venv/, venv/
```

#### 步骤 5: Git 历史审计

```bash
# 应该是单一初始提交
cd PROJECT_DIR
git log --oneline | wc -l
# 如果 > 1，历史未清理 — FAIL

# 搜索可能的 secrets
git log -p | grep -iE '(password|secret|api.?key|token)' | head -20
```

### 输出格式

生成项目目录中的 `SANITIZATION_REPORT.md`：

```markdown
# Sanitization Report: {project-name}

**Date:** {date}
**Auditor:** opensource-sanitizer v1.0.0
**Verdict:** PASS | FAIL | PASS WITH WARNINGS

## Summary

| Category | Status | Findings |
|----------|--------|----------|
| Secrets | PASS/FAIL | {count} findings |
| PII | PASS/FAIL | {count} findings |
| Internal References | PASS/FAIL | {count} findings |
| Dangerous Files | PASS/FAIL | {count} findings |
| Config Completeness | PASS/WARN | {count} findings |
| Git History | PASS/FAIL | {count} findings |

## Critical Findings (Must Fix Before Release)

1. **[SECRETS]** `src/config.py:42` — Hardcoded database password: `DB_P...` (truncated)

## Warnings (Review Before Release)

1. **[CONFIG]** `src/app.py:8` — Port 8080 hardcoded, should be configurable

## .env.example Audit

- Variables in code but NOT in .env.example: {list}
- Variables in .env.example but NOT in code: {list}

## Recommendation

{If FAIL: "Fix the {N} critical findings and re-run sanitizer."}
{If PASS: "Project is clear for open-source release. Proceed to packager."}
{If WARNINGS: "Project passes critical checks. Review {N} warnings before release."}
```

### 规则

- **绝不**显示完整 secret 值 - 截断为前 4 个字符 + "..."
- **绝不**修改源文件 - 只生成报告 (SANITIZATION_REPORT.md)
- **始终**扫描每个文本文件，不只是已知扩展名
- **始终**检查 git 历史，即使是新仓库
- **保持偏执** - 误报可接受，漏报不可
- 任何类别的单一 CRITICAL 发现 = 整体 FAIL
- 单独警告 = PASS WITH WARNINGS（用户决定）
[返回 Agent 索引](../README.md)
