# Scripts de Utilitários

Este documento apresenta os principais Scripts de Utilitários no projeto ECC (Everything Claude Code).

## Índice

- [ecc.js](#eccjs) - Ponto de entrada principal de CLI
- [install-apply.js](#install-applyjs) - Executor de instalação
- [claw.js](#clawjs) - NanoClaw REPL
- [harness-audit.js](#harness-auditjs) - Auditoria de toolchain
- [catalog.js](#catalogjs) - Visualizador de catálogo de componentes
- [build-opencode.js](#build-opencodejs) - Build OpenCode

---

## ecc.js

**Caminho**: `scripts/ecc.js`

Ponto de entrada CLI para instalação seletiva do ECC, escalona todos os subcomandos de forma unificada.

### Lista de Comandos

| Comando | Script | Descrição |
|---------|--------|----------|
| `install` | install-apply.js | Instalar conteúdo ECC no alvo |
| `plan` | install-plan.js | Verificar lista de instalação e plano de parsing |
| `catalog` | catalog.js | Descobrir arquivos de configuração de instalação e IDs de componentes |
| `consult` | consult.js | Recomendar componentes baseado em consulta em linguagem natural |
| `list-installed` | list-installed.js | Verificar status de instalação do contexto atual |
| `doctor` | doctor.js | Diagnosticar arquivos de gerenciamento ECC faltantes ou drift |
| `repair` | repair.js | Restaurar arquivos de gerenciamento ECC drift ou faltantes |
| `auto-update` | auto-update.js | Pull das últimas mudanças ECC e reinstalar |
| `status` | status.js | Consultar resumo de armazenamento de estado SQLite |
| `platform-audit` | platform-audit.js | Auditar fila GitHub, discussões, roadmap etc. |
| `security-ioc-scan` | ci/scan-supply-chain-iocs.js | Escanear dependências e persistência de ferramentas AI para IOC de supply chain |
| `sessions` | sessions-cli.js | Listar ou verificar sessões no armazenamento de estado SQLite |
| `work-items` | work-items.js | Rastrear Linked Linear, itens de trabalho GitHub |
| `session-inspect` | session-inspect.js | Emitir snapshot de sessão especificada de dmux ou histórico Claude target |
| `loop-status` | loop-status.js | Verificar loops de wake-up antigos e resultados de ferramenta pendentes em scripts Claude |
| `uninstall` | uninstall.js | Excluir arquivos de gerenciamento ECC registrados em install-state |

### Exemplos de Uso

```bash
# Instalação direta de linguagem
ecc typescript

# Instalação com arquivo de configuração
ecc install --profile developer --target claude

# Ver plano de instalação
ecc plan --profile core --target cursor

# Listar componentes disponíveis
ecc catalog profiles
ecc catalog components --family language

# Mostrar detalhes do componente
ecc catalog show framework:nextjs

# Consultar recomendações
ecc consult "security reviews"

# Verificar instalado
ecc list-installed --json

# Diagnosticar problemas
ecc doctor --target cursor
ecc repair --dry-run

# Auto atualização
ecc auto-update --dry-run

# Consultar status
ecc status --json
ecc status --exit-code
ecc status --markdown --write status.md

# Auditoria de plataforma
ecc platform-audit --json --allow-untracked docs/drafts/

# Varredura de segurança
ecc security-ioc-scan --home

# Gerenciamento de sessão
ecc sessions
ecc sessions session-active --json

# Rastreamento de itens de trabalho
ecc work-items upsert linear-ecc-20 --source linear --source-id ECC-20 --title "Review control-plane contract" --status blocked
ecc work-items sync-github --repo affaan-m/ECC

# Ver status de loop
ecc loop-status --json

# Desinstalar
ecc uninstall --target antigravity --dry-run
```

---

## install-apply.js

**Caminho**: `scripts/install-apply.js`

Runtime de instalação ECC, mantém pontos de entrada de instalação de linguagem tradicionais intactos enquanto move lógica de mudança específica de alvo para código Node testável.

### Alvos Suportados

| Alvo | Descrição |
|------|----------|
| `claude` | Instalar em ~/.claude/, regras/skills em rules/ecc e skills/ecc |
| `claude-project` | Instalar em ./.claude/ (nível de projeto) |
| `cursor` | Instalar regras, hooks, configuração cursor em ./.cursor/ |
| `antigravity` | Instalar regras, workflows, skills, agents em ./.agent/ |
| `codex` | Instalar agents/configuração compartilhados em ~/.codex/ |
| `gemini` | Instalar configuração Gemini nível de projeto em ./.gemini/ |
| `opencode` | Instalar comandos/hooks/configuração compartilhados em ~/.opencode/ |
| `codebuddy` | Instalar comandos, agents, skills em ./.codebuddy/ |
| `joycode` | Instalar comandos, agents, skills em ./.joycode/ |
| `qwen` | Instalar em ~/.qwen/ |
| `zed` | Instalar em ./.zed/ |

### Modos de Instalação

1. **Modo linguagem tradicional**: `install.sh <language>` (ex: `ecc-install typescript`)
2. **Modo Profile**: `install.sh --profile <name> [--with <component>]... [--without <component>]...`
3. **Modo módulo**: `install.sh --modules <id,id,...>`
4. **Modo skill**: `install.sh --skills <skill-id[,skill-id...]>`
5. **Modo localização**: `install.sh --target claude|claude-project --locale <locale-code>`

### Opções

- `--dry-run` - Mostrar plano de instalação sem copiar arquivos
- `--json` - Emitir formato JSON legível por máquina
- `--config <path>` - Carregar intenção de instalação de arquivo

---

## claw.js

**Caminho**: `scripts/claw.js`

NanoClaw v2 - REPL zero-dependency, com suporte a sessão, baseado em `claude -p` para Everything Claude Code.

### Funcionalidades

- Persistência de histórico de sessão armazenado em `~/.claude/claw/`
- Suporte a carregamento dinâmico de skills como contexto
- Compressão de sessão para gerenciar comprimento de contexto
- Branch e exportação de sessão
- Busca cross-sessão

### Comandos REPL

| Comando | Descrição |
|---------|----------|
| `/help` | Mostrar ajuda |
| `/clear` | Limpar histórico de sessão atual |
| `/history` | Imprimir histórico de conversa completo |
| `/sessions` | Listar sessões salvas |
| `/model [name]` | Mostrar/configurar modelo |
| `/load <skill-name>` | Carregar skill no contexto ativo |
| `/branch <session-name>` | Branch de sessão atual para nova sessão |
| `/search <query>` | Busca cross-sessão |
| `/compact` | Manter rodadas recentes, comprimir contexto antigo |
| `/export <md\|json\|txt> [path]` | Exportar sessão |
| `/metrics` | Mostrar métricas de sessão |
| `exit` | Sair do REPL |

### Variáveis de Ambiente

| Variável | Padrão | Descrição |
|---------|--------|----------|
| `CLAW_SESSION` | `default` | Nome da sessão inicial |
| `CLAW_MODEL` | `sonnet` | Modelo padrão |
| `CLAW_SKILLS` | vazio | Lista de skills a carregar (separados por vírgula) |

### Exemplos de Uso

```bash
# Iniciar com sessão padrão
node scripts/claw.js

# Iniciar com sessão especificada
CLAW_SESSION=my-session node scripts/claw.js

# Iniciar com skills carregados
CLAW_SKILLS="javascript-patterns,python-testing" node scripts/claw.js
```

---

## harness-audit.js

**Caminho**: `scripts/harness-audit.js`

Ferramenta de auditoria de toolchain determinística, baseada em verificação explícita de arquivo/regras. Detecta automaticamente padrão ECC repo vs padrão projeto consumidor.

### Categorias de Auditoria

| Categoria | Descrição |
|-----------|-----------|
| Tool Coverage | Completude de cobertura de ferramenta |
| Context Efficiency | Eficiência de contexto |
| Quality Gates | Quality gates |
| Memory Persistence | Persistência de memória |
| Eval Coverage | Cobertura de avaliação |
| Security Guardrails | Rails de segurança |
| Cost Efficiency | Eficiência de custo |
| GitHub Integration | Integração GitHub |
| Vercel/Netlify/Cloudflare/Fly Integration | Integração de várias plataformas |

### Exemplos de Uso

```bash
# Auditar diretório atual
node scripts/harness-audit.js

# Especificar escopo
node scripts/harness-audit.js --scope repo
node scripts/harness-audit.js --scope hooks
node scripts/harness-audit.js --scope skills

# Especificar diretório raiz
node scripts/harness-audit.js --root /path/to/project

# Formato de saída JSON
node scripts/harness-audit.js --format json

# Combinar uso
node scripts/harness-audit.js repo --format json --root .
```

---

## catalog.js

**Caminho**: `scripts/catalog.js`

Ferramenta para descobrir componentes e arquivos de configuração instalados do ECC.

### Comandos

#### profiles

Listar todos os perfis de instalação.

```bash
node scripts/catalog.js profiles
node scripts/catalog.js profiles --json
```

#### components

Listar componentes instalados, pode filtrar por família e alvo.

```bash
node scripts/catalog.js components
node scripts/catalog.js components --family language
node scripts/catalog.js components --family framework
node scripts/catalog.js components --target claude
node scripts/catalog.js components --json
```

#### show

Mostrar detalhes de componente específico.

```bash
node scripts/catalog.js show <component-id>
node scripts/catalog.js show framework:nextjs --json
```

### Famílias de Componentes

- `baseline` - Componentes baseline
- `language` - Componentes de linguagem (javascript, typescript, python, etc.)
- `framework` - Componentes de framework (nextjs, react, vue, etc.)
- `capability` - Componentes de capability
- `agent` - Componentes de agent
- `skill` - Componentes de skill

---

## build-opencode.js

**Caminho**: `scripts/build-opencode.js`

Compila código TypeScript do plugin OpenCode.

### Funcionalidades

1. Limpa diretório `dist`
2. Compila código no diretório `.opencode` usando compilador TypeScript
3. Saída para `.opencode/dist`

### Pré-requisitos

Requer dependências de desenvolvimento do diretório raiz instaladas, garantir que compilador TypeScript está disponível.

### Uso

```bash
node scripts/build-opencode.js
```