# Comandos de Planejamento e Arquitetura

## Visão Geral

Comandos de planejamento e arquitetura são usados para planejamento de produtos, design de funcionalidades e decisões de arquitetura de sistemas. Estes comandos garantem que haja um plano claro antes de escrever código, reduzindo retrabalho e melhorando qualidade de código.

## Lista de Comandos

### /plan

**Propósito**: Planejamento de implementação geral - reformular requisitos, avaliar riscos, criar plano de implementação passo a passo

**Descrição**: Criar plano de implementação abrangente antes de tocar qualquer código, após confirmação do usuário. Aceita requisitos em formato livre ou arquivo PRD markdown.

**Modo de Entrada**:

| Entrada | Modo | Comportamento |
|---|---|---|
| `path/to/name.prd.md` | Modo artifact PRD | Ler PRD, selecionar próximo milestone pendente, gerar `.claude/plans/{name}.plan.md` |
| Outro caminho markdown | Modo de referência | Ler arquivo como contexto, gerar plano inline |
| Texto em formato livre | Modo conversa | Gerar plano inline |
| Entrada vazia | Modo esclarecimento | Perguntar o que planejar |

**Parâmetros Chave**:
- `$ARGUMENTS`: `[descrição da funcionalidade | path/to/*.prd.md]` - Descrição da funcionalidade ou caminho do arquivo PRD

**Fluxo de Trabalho**:
1. **Reformular Requisitos** - Esclarecer o que construir
2. **Identificar Riscos** - Listar problemas potenciais e obstáculos
3. **Criar Plano de Passos** - Decompor em fases de implementação
4. **Aguardar Confirmação** - Deve receber aprovação do usuário antes de continuar

**Formato de Saída** (modo artifact PRD):
```markdown
# Plan: {Feature Name}

**Source PRD**: {path}
**Selected Milestone**: {milestone or phase name}
**Complexity**: {Small | Medium | Large}

## Summary
{2-3 sentences}

## Patterns to Mirror
| Category | Source | Pattern |
|---|---|---|
| Naming | `path:line` | {short description} |
| Errors | `path:line` | {short description} |
| Tests | `path:line` | {short description} |

## Files to Change
| File | Action | Why |
|---|---|---|
| `path` | CREATE / UPDATE / DELETE | {reason} |

## Tasks
### Task 1: {name}
- **Action**: {what to do}
- **Mirror**: {pattern to follow}
- **Validate**: {command that proves correctness}

## Validation
```bash
{project-specific validation commands}
```

## Risks
| Risk | Likelihood | Mitigation |
|---|---|---|
```

**Melhores-Práticas**:
- Pesquisar padrões existentes no codebase antes de usar
- No modo artifact PRD, criar diretório `.claude/plans/` se não existir
- Se PRD contém tabela `Delivery Milestones`, atualizar apenas status da linha selecionada

**Armadilhas Comuns**:
- Não perguntar esclarecimento quando entrada vazia
- Não aguardarusuário confirmar antes de começar a escrever código
- Pular etapa de Pattern Grounding

**Integração com Outros Comandos**:
- Após planejar, usar skill `tdd-workflow` para desenvolvimento orientado a testes
- Usar `/build-fix` para corrigir erros de build
- Usar `/code-review` para revisar implementação completa
- Usar `/pr` ou `/prp-pr` para abrir Pull Request

---

### /plan-prd

**Propósito**: Gerador interativo de PRD

**Descrição**: Gerar PRD centrado em problemas, incluindo definição de problemas, personas de usuário, métricas de sucesso e escopo MVP.

**Cenários de Uso**: 
- Determinar funcionalidades a construir
- Quando necessidades de requisitos de produto serem claras
- Planejar nova funcionalidade do zero

**Fluxo de Trabalho**:
1. FRAME - Entender usuário e problema
2. GROUND - Coletar evidências
3. DECIDE - Determinar escopo e hipóteses
4. GENERATE - Gerar arquivo PRD

---

### /prp-plan

**Propósito**: Planejamento abrangente de funcionalidades

**Descrição**: Planejamento de projetos completo, cobrindo análise de codebase, avaliação de riscos e plano de implementação passo a passo.

**Cenários de Uso**:
- Planejamento detalhado de funcionalidades complexas
- Quando necessidade de análise de contexto de codebase
- Projetos de implementação multi-fase

---

### /prp-prd

**Propósito**: Gerador PRP workflow PRD

**Descrição**: Gerar documento de requisitos de produto usando fluxo de trabalho PRP (Plan-Record-Produce).

---

### /prp-implement

**Propósito**: Executar loop de plano+verificação PRP

**Descrição**: Executar plano seguindo fluxo de trabalho PRP, incluindo verificação e checagens contínuas.

---

### /prp-pr

**Propósito**: Criar PR do workflow PRP

**Descrição**: Criar GitHub Pull Request dos resultados do workflow PRP.

---

### /prp-commit

**Propósito**: Verificação de commit PRP

**Descrição**: Executar verificação e fazer commit de código no workflow PRP.

---

### /multi-plan

**Propósito**: Planejamento colaborativo multi-modelo

**Descrição**: Combinar análise de dois modelos (Codex e Gemini), gerar plano de implementação abrangente.

**Cenários de Uso**:
- Quando necessidade de múltiplas perspectivas
- Projetos complexos cross-domínio
- Quando necessidade de equilibrar considerações frontend e backend

---

## Comandos Relacionados

- `/plan` - Planejamento de implementação geral
- `/code-review` - Revisão de código
- `/build-fix` - Correção de build