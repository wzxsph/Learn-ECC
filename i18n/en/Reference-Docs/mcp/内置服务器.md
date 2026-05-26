# Built-in MCP Servers

ECC project pre-configures multiple commonly used MCP server configurations in `mcp-configs/mcp-servers.json`.

## Categorized by Function

### Development Tools

| Server | Type | Description |
|--------|------|------|
| `github` | process | GitHub operations - PRs, issues, repos |
| `jira` | process | Jira issue tracking - search, create, update, comment, transition issues |
| `confluence` | process | Confluence Cloud integration - search pages, get content, explore spaces |
| `filesystem` | process | Filesystem operations (requires path configuration) |
| `memory` | process | Cross-session persistent memory |
| `omega-memory` | process | Enhanced persistent agent memory with semantic search, multi-agent collaboration, and knowledge graphs |
| `sequential-thinking` | process | Chain-of-thought reasoning |

### Deployment and Infrastructure

| Server | Type | Description |
|--------|------|------|
| `vercel` | http | Vercel deployment and projects |
| `railway` | process | Railway deployment |
| `cloudflare-docs` | http | Cloudflare documentation search |
| `cloudflare-workers-builds` | http | Cloudflare Workers builds |
| `cloudflare-workers-bindings` | http | Cloudflare Workers bindings |
| `cloudflare-observability` | http | Cloudflare observability/logging |

### Database

| Server | Type | Description |
|--------|------|------|
| `supabase` | process | Supabase database operations |
| `clickhouse` | http | ClickHouse analytical queries |

### AI and Machine Learning

| Server | Type | Description |
|--------|------|------|
| `fal-ai` | process | fal.ai AI image/video/audio generation |
| `exa-web-search` | process | Web search and research via Exa API |
| `context7` | process | Real-time documentation query - used with /docs command and documentation-lookup skill |
| `magic` | process | Magic UI components |
| `evalview` | process | AI agent regression testing - snapshot behavior, detect regressions in tool calls and output quality |

### Browser Automation

| Server | Type | Description |
|--------|------|------|
| `playwright` | process | Browser automation and testing via Playwright |
| `browserbase` | process | Browserbase cloud browser sessions |
| `browser-use` | http | AI browser agent, executes web tasks |

### Other Tools

| Server | Type | Description |
|--------|------|------|
| `firecrawl` | process | Web scraping and crawling |
| `longhand` | process | Lossless Claude Code session history - indexes raw tool calls to local SQLite + ChromaDB |
| `token-optimizer` | process | Token optimization - 95%+ context reduction via content deduplication and compression |
| `devfleet` | http | Multi-agent orchestration - schedule parallel Claude Code agents in isolated worktrees |
| `laraplugins` | http | Laravel plugin discovery - search packages by keyword, health scores, Laravel/PHP version compatibility |

## Quick Enable

Copy needed server configuration to `mcpServers` section of `~/.claude.json`, replacing `YOUR_*_HERE` placeholders with actual values.

**Example - Enable GitHub server:**

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

## Environment Variable Requirements

| Server | Required Environment Variables |
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