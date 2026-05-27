# Comandos de Workflow de Pull Request

## Visão Geral

Comandos de workflow de PR são usados para criar, gerenciar e revisar GitHub Pull Requests.

## Lista de Comandos

### /pr

**Propósito**: Criar GitHub PR do branch atual, com commits não pushados

**Descrição**: Descobre template, analisa mudanças, faz push do branch e cria PR. Analisa histórico completo de commits, usa `git diff [base-branch]...HEAD` para ver todas as mudanças.

**Entrada**: `$ARGUMENTS` - Opcional, pode conter nome do branch base e/ou flags (como `--draft`)

**Regras de Parsing**:
- Extrai quaisquer flags identificadas (como `--draft`)
- Texto restante não-flag é usado como nome do branch base
- Default para `main` se não especificado

**Fluxo de Trabalho**:

| Fase | Descrição |
|---|---|
| **Phase 1: VALIDATE** | Verificar pré-condições (não estar no branch base, sem mudanças não commitadas, commits ahead, sem PR existente) |
| **Phase 2: DISCOVER** | Buscar template PR, analisar commits, analisar mudanças de arquivo, verificar artefatos de planejamento relacionados |
| **Phase 3: PUSH** | Fazer push do branch (rebasing primeiro se necessário) |
| **Phase 4: CREATE** | Criar PR usando template ou formato default |
| **Phase 5: VERIFY** | Verificar criação bem-sucedida do PR |
| **Phase 6: OUTPUT** | Reportar URL do PR e próximos passos |

**Verificações de Validação de Criação de PR**:

| Verificação | Condição | Ação se Falhar |
|---|---|---|
| Não está no branch base | Branch atual ≠ base | Parar: "Mude para branch de feature primeiro" |
| Workspace limpo | Sem mudanças não commitadas | Alerta: "Commit ou stash primeiro" |
| Commits ahead | `git log origin/<base>..HEAD` não vazio | Parar: "Sem commits ahead" |
| Sem PR existente | `gh pr list` vazio | Parar: "PR já existe" |

**Ordem de Busca de Template**:
1. Diretório `.github/PULL_REQUEST_TEMPLATE/` (se existir)
2. `.github/PULL_REQUEST_TEMPLATE.md`
3. `.github/pull_request_template.md`
4. `docs/pull_request_template.md`

**Formato Default de PR** (sem template):
```markdown
## Summary
<1-2 sentence description>

## Changes
<bulleted list grouped by area>

## Files Changed
<table of changed files>

## Testing
<how changes were tested, or "Needs testing">

## Related Issues
<linked issues or "None">
```

**Tratamento de Casos de Borda**:
- **Sem `gh` CLI**: Parar e instruir instalação
- **Não autenticado**: Instruir executar `gh auth login`
- **Branch desactualizado**: Usar `git fetch && git rebase` então fazer push
- **PR grande (>20 arquivos)**: Alerta e sugerir divisão

**Integração com Outros Comandos**:
- Usar `/code-review <number>` para revisar PR
- Usar `gh pr merge <number>` para fazer merge de PR pronto
- Usar `/plan-prd` ou `/plan` para criar artefatos de planejamento relacionados

---

### /review-pr

**Propósito**: Revisar GitHub PR ou mudanças locais não commitadas

**Descrição**: Revisa Pull Request no GitHub ou mudanças locais não commitadas. Modo de revisão de PR adaptado de PRPs-agentic-eng.

**Entrada**: `$ARGUMENTS` - Pode ser número do PR, URL do PR, ou vazio (revisão local)

---

#### Seleção de Modo

| Entrada | Ação |
|---|---|
| Número do PR (ex: `42`) | Usar como número do PR |
| URL (ex: `github.com/.../pull/42`) | Extrair número do PR |
| Nome do branch | Encontrar PR via `gh pr list --head <branch>` |
| Vazio | Usar modo de revisão local |

---

#### Modo de Revisão Local

**Fluxo de Trabalho**:
1. **GATHER** - Coletar mudanças: `git diff --name-only HEAD`
2. **REVIEW** - Revisar cada arquivo (problemas de segurança, qualidade, melhores práticas)
3. **REPORT** - Gerar relatório com níveis de severidade e sugestões de correção

**Checklist de Revisão**:

| Categoria | Itens de Verificação |
|---|---|
| **Problemas de Segurança (CRITICAL)** | Credenciais hardcoded, SQL injection, XSS, validação de input faltando, dependências inseguras, path traversal |
| **Qualidade de Código (HIGH)** | Funções>50 linhas, arquivos>800 linhas, aninhamento>4 níveis, tratamento de erros faltando, console.log, TODO/FIXME |
| **Melhores Práticas (MEDIUM)** | Padrões de mudança, uso de emoji, código novo sem testes, problemas de acessibilidade |

**Decisões**:

| Condição | Decisão |
|---|---|
| Existe problema CRITICAL ou HIGH | **BLOCK** - Prevenir commit |
| Apenas problemas MEDIUM/LOW | Aprovar com comentários |
| Sem problemas e verificação passa | **APPROVE** |

---

#### Modo de Revisão de PR

**Fluxo de Trabalho**:
1. **FETCH** - Obter metadados e diff do PR
2. **CONTEXT** - Construir contexto de revisão (regras do projeto, artefatos de planejamento, arquivos de mudança)
3. **REVIEW** - Ler arquivos completos, aplicar checklist de 7 categorias
4. **VALIDATE** - Executar comandos de verificação do projeto
5. **DECIDE** - Formar sugestões baseadas em descobertas
6. **REPORT** - Criar artefato de revisão
7. **PUBLISH** - Publicar revisão no GitHub
8. **OUTPUT** - Reportar ao usuário

**Checklist de 7 Categorias**:

| Categoria | Itens de Verificação |
|---|---|
| **Correctness** | Erros de lógica, casos de borda, tratamento null, condições de corrida |
| **Type Safety** | Tipos incompatíveis, casts de tipo inseguros, uso de `any`, generics faltando |
| **Pattern Compliance** | Conformidade com convenções do projeto (nomenclatura, estrutura de arquivo, tratamento de erros, imports) |
| **Security** | Injection, gaps de auth, exposição de secrets, SSRF, path traversal, XSS |
| **Performance** | Queries N+1, índices faltando, loops não limitados, vazamentos de memória, payloads grandes |
| **Completeness** | Testes faltando, tratamento de erros faltando, migrações incompletas, documentação faltando |
| **Maintainability** | Código morto, números mágicos, aninhamento profundo, nomenclatura unclear, tipos faltando |

**Detecção de Comandos de Verificação**:

| Tipo de Projeto | Arquivos Detectados | Comandos de Verificação |
|---|---|---|
| Node.js/TypeScript | `package.json` | `npm run typecheck`, `npm run lint`, `npm test`, `npm run build` |
| Rust | `Cargo.toml` | `cargo clippy`, `cargo test`, `cargo build` |
| Go | `go.mod` | `go vet ./...`, `go test ./...`, `go build ./...` |
| Python | `pyproject.toml` | `pytest` |

**Níveis de Decisão**:

| Nível | Significado | Ação |
|---|---|---|
| CRITICAL | Vulnerabilidade de segurança ou risco de perda de dados | Deve ser corrigido antes de fazer merge |
| HIGH | Bug ou erro lógico que pode causar problemas | Deve ser corrigido antes de fazer merge |
| MEDIUM | Falta de qualidade de código ou melhores práticas | Sugere correção |
| LOW | Problema de estilo ou sugestão pequena | Opcional |

**Publicar Revisão no GitHub**:
```bash
# APPROVE
gh pr review <NUMBER> --approve --body "<summary>"

# REQUEST_CHANGES
gh pr review <NUMBER> --request-changes --body "<summary with required fixes>"

# COMMENT only (draft PR ou informativo)
gh pr review <NUMBER> --comment --body "<summary>"
```

---

### /multi-workflow (Em desenvolvimento)

> (Em desenvolvimento) Detalhes do comando a serem adicionados

**Propósito**: Desenvolvimento colaborativo multi-modelo

**Descrição**: Coordena múltiplos modelos de AI para desenvolvimento colaborativo.

---

### /multi-backend (Em desenvolvimento)

> (Em desenvolvimento) Detalhes do comando a serem adicionados

**Propósito**: Desenvolvimento backend multi-modelo

**Descrição**: Desenvolvimento centrado em backend com múltiplos modelos.

---

### /multi-frontend (Em desenvolvimento)

> (Em desenvolvimento) Detalhes do comando a serem adicionados

**Propósito**: Desenvolvimento frontend multi-modelo

**Descrição**: Desenvolvimento centrado em frontend com múltiplos modelos.

---

### /multi-execute (Em desenvolvimento)

> (Em desenvolvimento) Detalhes do comando a serem adicionados

**Propósito**: Execução colaborativa multi-modelo

**Descrição**: Coordena múltiplos modelos para executar tarefas em paralelo.

---

## Comandos Relacionados

- `/pr` - Criar PR
- `/review-pr` - Revisar PR
- `/multi-workflow` - Colaboração multi-modelo