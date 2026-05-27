# Servidores MCP Embutidos

O projeto ECC pré-configura vários servidores MCP comuns no `mcp-configs/mcp-servers.json`.

## Classificação por Funcionalidade

### Ferramentas de Desenvolvimento

| Servidor | Tipo | Descrição |
|---------|------|----------|
| `github` | process | Operações GitHub - PRs, issues, repos |
| `jira` | process | rastreamento de problemas Jira - buscar, criar, atualizar, comentar, transitar problemas |
| `confluence` | process | Integração Confluence Cloud - buscar páginas, obter conteúdo, explorar espaços |
| `filesystem` | process | Operações de sistema de arquivos (requer configuração de caminho) |
| `memory` | process | Memória persistente entre sessões |
| `omega-memory` | process | Memória de agente persistente avançada, suporta busca semântica, colaboração multi-agente e grafo de conhecimento |
| `sequential-thinking` | process | Raciocínio em cadeia de pensamento |

### Deploy e Infraestrutura

| Servidor | Tipo | Descrição |
|---------|------|----------|
| `vercel` | http | Deploy e projetos Vercel |
| `railway` | process | Deploy Railway |
| `cloudflare-docs` | http | Busca de documentos Cloudflare |
| `cloudflare-workers-builds` | http | Builds de Cloudflare Workers |
| `cloudflare-workers-bindings` | http | Bindings de Cloudflare Workers |
| `cloudflare-observability` | http | Observabilidade/logging Cloudflare |

### Banco de Dados

| Servidor | Tipo | Descrição |
|---------|------|----------|
| `supabase` | process | Operações de banco de dados Supabase |
| `clickhouse` | http | Queries analíticas ClickHouse |

### IA e Machine Learning

| Servidor | Tipo | Descrição |
|---------|------|----------|
| `fal-ai` | process | Geração de imagem/video/áudio AI via modelos fal.ai |
| `exa-web-search` | process | Busca e pesquisa web via API Exa |
| `context7` | process | Consulta de documentação em tempo real - usado com comando /docs e skill documentation-lookup |
| `magic` | process | Componentes Magic UI |
| `evalview` | process | Testes de regressão de agente AI - captura comportamento, detecta regressões em chamadas de ferramenta e qualidade de output |

### Automação de Navegador

| Servidor | Tipo | Descrição |
|---------|------|----------|
| `playwright` | process | Automação de navegador e testes via Playwright |
| `browserbase` | process | Sessões de navegador em nuvem Browserbase |
| `browser-use` | http | Proxy de navegador AI, executa tarefas web |

### Outras Ferramentas

| Servidor | Tipo | Descrição |
|---------|------|----------|
| `firecrawl` | process | Crawling e raspagem web |
| `longhand` | process | Histórico de sessão Claude Code sem perda - indexa chamadas de ferramenta originais para SQLite local + ChromaDB |
| `token-optimizer` | process | Otimização de token - alcança 95%+ de redução de contexto via deduplicação e compressão de conteúdo |
| `devfleet` | http | Orquestração multi-agente - agenda agentes Claude Code paralelos em worktrees isolados |
| `laraplugins` | http | Descoberta de plugins Laravel - buscar pacotes por palavra-chave, scores de saúde, compatibilidade Laravel/PHP |

## Início Rápido

Copiar configuração de servidor necessária para a seção `mcpServers` de `~/.claude.json`, substituindo placeholders `YOUR_*_HERE` com valores reais.

**Exemplo - Habilitar servidor GitHub:**

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_your_actual_token"
      },
      "description": "Operações GitHub - PRs, issues, repos"
    }
  }
}
```

## Requisitos de Variáveis de Ambiente

| Servidor | Variáveis de ambiente necessárias |
|---------|-----------------------------------|
| `github` | `GITHUB_PERSONAL_ACCESS_TOKEN` |
| `jira` | `JIRA_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN` |
| `firecrawl` | `FIRECRAWL_API_KEY` |
| `supabase` | Parâmetro `--project-ref` |
| `exa-web-search` | `EXA_API_KEY` |
| `fal-ai` | `FAL_KEY` |
| `browserbase` | `BROWSERBASE_API_KEY` |
| `browser-use` | Header `x-browser-use-api-key` request |
| `confluence` | `CONFLUENCE_BASE_URL`, `CONFLUENCE_EMAIL`, `CONFLUENCE_API_TOKEN` |
| `evalview` | `OPENAI_API_KEY` (opcional) |