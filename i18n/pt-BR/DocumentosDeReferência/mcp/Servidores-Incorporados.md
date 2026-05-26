# 内置 MCP 服务器

ECC 项目在 `mcp-configs/mcp-servers.json` 中预置了多个常用 MCP 服务器配置。

## 按功能分类

### 开发工具

| 服务器 | 类型 | 描述 |
|--------|------|------|
| `github` | process | GitHub 操作 - PRs, issues, repos |
| `jira` | process | Jira 问题跟踪 - 搜索、创建、更新、评论、流转问题 |
| `confluence` | process | Confluence Cloud 集成 - 搜索页面、获取内容、探索空间 |
| `filesystem` | process | 文件系统操作（需配置路径） |
| `memory` | process | 跨会话持久化内存 |
| `omega-memory` | process | 增强版持久化代理内存，支持语义搜索、多代理协作和知识图谱 |
| `sequential-thinking` | process | 链式思维推理 |

### 部署与基础设施

| 服务器 | 类型 | 描述 |
|--------|------|------|
| `vercel` | http | Vercel 部署和项目 |
| `railway` | process | Railway 部署 |
| `cloudflare-docs` | http | Cloudflare 文档搜索 |
| `cloudflare-workers-builds` | http | Cloudflare Workers 构建 |
| `cloudflare-workers-bindings` | http | Cloudflare Workers 绑定 |
| `cloudflare-observability` | http | Cloudflare 可观测性/日志 |

### 数据库

| 服务器 | 类型 | 描述 |
|--------|------|------|
| `supabase` | process | Supabase 数据库操作 |
| `clickhouse` | http | ClickHouse 分析查询 |

### AI 与机器学习

| 服务器 | 类型 | 描述 |
|--------|------|------|
| `fal-ai` | process | fal.ai 模型的 AI 图片/视频/音频生成 |
| `exa-web-search` | process | 通过 Exa API 进行网页搜索和研究 |
| `context7` | process | 实时文档查询 - 与 /docs 命令和 documentation-lookup skill 配合使用 |
| `magic` | process | Magic UI 组件 |
| `evalview` | process | AI agent 回归测试 - 快照行为，检测工具调用和输出质量的回归 |

### 浏览器自动化

| 服务器 | 类型 | 描述 |
|--------|------|------|
| `playwright` | process | 通过 Playwright 进行浏览器自动化和测试 |
| `browserbase` | process | Browserbase 云浏览器会话 |
| `browser-use` | http | AI 浏览器代理，执行网页任务 |

### 其他工具

| 服务器 | 类型 | 描述 |
|--------|------|------|
| `firecrawl` | process | 网页抓取和爬取 |
| `longhand` | process | 无损 Claude Code 会话历史 - 将原始工具调用索引到本地 SQLite + ChromaDB |
| `token-optimizer` | process | Token 优化 - 通过内容去重和压缩实现 95%+ 上下文缩减 |
| `devfleet` | http | 多Orquestração-de-Agentes - 在隔离的 worktrees 中调度并行 Claude Code agents |
| `laraplugins` | http | Laravel 插件发现 - 按关键字搜索包、健康分数、Laravel/PHP 版本兼容性 |

## 快速启用

复制需要的服务器配置到 `~/.claude.json` 的 `mcpServers` 节，替换 `YOUR_*_HERE` 占位符为实际值。

**示例 - 启用 GitHub 服务器：**

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_actual_token"
      },
      "description": "GitHub operations - PRs, issues, repos"
    }
  }
}
```

## 环境变量要求

| 服务器 | 必需的环境变量 |
|--------|----------------|
| `github` | `GITHUB_PERSONAL_ACCESS_TOKEN` |
| `jira` | `JIRA_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN` |
| `firecrawl` | `FIRECRAWL_API_KEY` |
| `supabase` | `--project-ref` 参数 |
| `exa-web-search` | `EXA_API_KEY` |
| `fal-ai` | `FAL_KEY` |
| `browserbase` | `BROWSERBASE_API_KEY` |
| `browser-use` | `x-browser-use-api-key` 请求头 |
| `confluence` | `CONFLUENCE_BASE_URL`, `CONFLUENCE_EMAIL`, `CONFLUENCE_API_TOKEN` |
| `evalview` | `OPENAI_API_KEY`（可选） |
