# Outros Comandos

## Visão Geral

Outros comandos diversos, incluindo integração Jira, operações GAN, scanning de segurança e mais.

## Lista de Comandos

### /jira

**Propósito**: Interagir com tickets Jira - obter, analisar, comentar, alterar status, buscar

**Descrição**: Integrar com ferramenta de gerenciamento de projetos Jira, suportando fluxo de trabalho direto com tickets Jira.

**Como Usar**:

| Comando | Descrição |
|---|---|
| `/jira get <TICKET-KEY>` | Obter e analisar ticket Jira |
| `/jira comment <TICKET-KEY>` | Adicionar comentário de progresso |
| `/jira transition <TICKET-KEY>` | Alterar status do ticket |
| `/jira search <JQL>` | Buscar issues usando JQL |

**Fluxo de Trabalho `/jira get`**:
1. Obter ticket do Jira (via MCP ou REST API)
2. Extrair todos os campos: resumo, descrição, critérios de aceite, prioridade, labels, issues relacionadas
3. Opcionalmente obter comentários para contexto adicional
4. Gerar análise estruturada

**Pré-requisitos** - Configuração de credenciais Jira (um de dois):

**Opção A — MCP Server (Recomendado)**:
Adicionar `jira` à configuração `mcpServers`.

**Opção B — Variáveis de Ambiente**:
```bash
export JIRA_URL="https://yourorg.atlassian.net"
export JIRA_EMAIL="your.email@example.com"
export JIRA_API_TOKEN="your-api-token"
```

**Integração com Outros Comandos**:
- Após analisar ticket, usar `/plan` para criar plano de implementação
- Usar skill `tdd-workflow` para desenvolvimento orientado a testes
- Usar `/code-review` para revisar implementação
- Usar `/jira comment` ou `/jira transition` para atualizar status do ticket

---

### /gan-build (Em desenvolvimento)

**Propósito**: Operações de build GAN

**Descrição**: Operações de build GAN (Generative Agent Network).

---

### /gan-design (Em desenvolvimento)

**Propósito**: Operações de design GAN

**Descrição**: Operações relacionadas a design GAN.

---

### /prune

**Propósito**: Excluir instincts pendentes stale com mais de 30 dias sem promoção

**Descrição**: Limpar instincts stale com mais de 30 dias. Instincts são gerados automaticamente mas nunca foram revisados ou promovidos.

**Como Usar**:
```
/prune                    # Excluir instincts com mais de 30 dias
/prune --max-age 60      # Personalizar limiar de dias
/prune --dry-run         # Visualizar sem excluir
```

**Implementação**:
```bash
python3 "${CLAUDE_PLUGIN_ROOT}/skills/continuous-learning-v2/scripts/instinct-cli.py" prune
```

Ou quando instalado manualmente (`CLAUDE_PLUGIN_ROOT` não definido):
```bash
python3 ~/.claude/skills/continuous-learning-v2/scripts/instinct-cli.py prune
```

**Parâmetros**:

| Parâmetro | Descrição |
|---|---|
| Nenhum | Excluir instincts com mais de 30 dias |
| `--max-age <days>` | Personalizar limiar de tempo (dias) |
| `--dry-run` | Visualizar conteúdo a excluir sem realmente excluir |

**Melhores-Práticas**:
- Usar `--dry-run` primeiro para visualizar conteúdo a excluir
- Executar prune regularmente para manter inventário de instincts limpo
- Observação: apenas exclui instincts em estado pending, não exclui os já promovidos

---

### /security-scan

**Propósito**: Scanning de segurança

**Descrição**: Realizar scanning de vulnerabilidades de segurança no codebase.

**O que é Escaneado**:
- Credenciais hardcoded
- Riscos de injeção SQL
- Vulnerabilidades XSS
- Vulnerabilidades de dependências

---

### /feature-dev (Em desenvolvimento)

**Propósito**: Assistente de desenvolvimento de funcionalidades

**Descrição**: Fornecer fluxo de trabalho auxiliar para desenvolvimento de funcionalidades.

---

### /cost-report

**Propósito**: Relatório de custos de modelo

**Descrição**: Gerar relatório de custos de uso de modelos AI.

---

## Comandos Relacionados

- `/security-scan` - Scanning de segurança
- `/jira` - Integração Jira
- `/prune` - Limpeza de instincts