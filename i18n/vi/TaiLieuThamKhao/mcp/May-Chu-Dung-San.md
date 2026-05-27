# Built-in MCP Server

## Tổng quan

ECC project pre-configure nhiều MCP server phổ biến trong `mcp-configs/mcp-servers.json`.

## Phân loại theo chức năng

### Development Tools

| Server | Loại | Mô tả |
|--------|------|------|
| `github` | process | GitHub operation - PRs, issues, repos |
| `jira` | process | Jira issue tracking - search, create, update, comment, transition issue |
| `confluence` | process | Confluence Cloud integration - search page, get content, explore space |
| `filesystem` | process | File system operation (cần configure path) |
| `memory` | process | Cross-session persistent memory |
| `omega-memory` | process | Enhanced persistent agent memory, hỗ trợ semantic search, multi-agent collaboration và knowledge graph |
| `sequential-thinking` | process | Chain-of-thought reasoning |

### Deployment và Infrastructure

| Server | Loại | Mô tả |
|--------|------|------|
| `vercel` | http | Vercel deployment và project |
| `railway` | process | Railway deployment |
| `cloudflare-docs` | http | Cloudflare document search |
| `cloudflare-workers-builds` | http | Cloudflare Workers build |
| `cloudflare-workers-bindings` | http | Cloudflare Workers binding |
| `cloudflare-observability` | http | Cloudflare observability/log |

### Database

| Server | Loại | Mô tả |
|--------|------|------|
| `supabase` | process | Supabase database operation |
| `clickhouse` | http | ClickHouse analytical query |

### AI và Machine Learning

| Server | Loại | Mô tả |
|--------|------|------|
| `fal-ai` | process | fal.ai model AI image/video/audio generation |
| `exa-web-search` | process | Web search và research qua Exa API |
| `context7` | process | Real-time document query - phối hợp với `/docs` command và documentation-lookup skill |
| `magic` | process | Magic UI component |
| `evalview` | process | AI agent regression testing - snapshot behavior, detect regression trong tool call và output quality |

### Browser Automation

| Server | Loại | Mô tả |
|--------|------|------|
| `playwright` | process | Browser automation và testing qua Playwright |
| `browserbase` | process | Browserbase cloud browser session |
| `browser-use` | http | AI browser agent, execute web task |

### Other Tools

| Server | Loại | Mô tả |
|--------|------|------|
| `firecrawl` | process | Web scraping và crawling |
| `longhand` | process | Lossless Claude Code session history - index raw tool call vào local SQLite + ChromaDB |
| `token-optimizer` | process | Token optimization - đạt được 95%+ context reduction qua content dedup và compression |
| `devfleet` | http | Multi-agent orchestration - schedule parallel Claude Code agents trong isolated worktrees |
| `laraplugins` | http | Laravel plugin discovery - search package theo keyword, health score, Laravel/PHP version compatibility |

## Quick Enable

Copy needed server configuration vào `~/.claude.json` `mcpServers` section, thay thế `YOUR_*_HERE` placeholder bằng actual value.

**Ví dụ - Enable GitHub server:**

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

## Environment Variable Requirement

| Server | Required environment variable |
|--------|----------------|
| `github` | `GITHUB_PERSONAL_ACCESS_TOKEN` |
| `jira` | `JIRA_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN` |
| `firecrawl` | `FIRECRAWL_API_KEY` |
| `supabase` | `--project-ref` parameter |
| `exa-web-search` | `EXA_API_KEY` |
| `fal-ai` | `FAL_KEY` |
| `browserbase` | `BROWSERBASE_API_KEY` |
| `browser-use` | `x-browser-use-api-key` request header |
| `confluence` | `CONFLUENCE_BASE_URL`, `CONFLUENCE_EMAIL`, `CONFLUENCE_API_TOKEN` |
| `evalview` | `OPENAI_API_KEY` (optional) |