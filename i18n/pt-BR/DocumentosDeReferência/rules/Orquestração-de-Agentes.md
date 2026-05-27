# Orquestração de Agentes

## Agentes Disponíveis

Localizados em `~/.claude/agents/`:

| Agent | Propósito | Quando Usar |
|-------|-----------|-------------|
| planner | Planejamento de implementação | Funcionalidades complexas, refatoração |
| architect | Design de sistemas | Decisões de arquitetura |
| tdd-guide | Desenvolvimento dirigido por testes | Nova funcionalidade, correção de bugs |
| code-reviewer | Revisão de código | Após escrever código |
| security-reviewer | Análise de segurança | Antes de commits |
| build-error-resolver | Corrigir erros de build | Quando build falha |
| e2e-runner | Testes end-to-end | jornadas críticas de usuário |
| refactor-cleaner | Limpeza de código morto | Manutenção de código |
| doc-updater | Atualização de documentação | Quando atualizar docs |
| rust-reviewer | Revisão de código Rust | Projetos Rust |
| harmonyos-app-resolver | Desenvolvimento de apps HarmonyOS | Projetos HarmonyOS/ArkTS |

## Usar Agentes Imediatamente

Sem prompt do usuário:
1. Solicitações de funcionalidade complexa → Usar agent **planner**
2. Código acabou de ser escrito/modificado → Usar agent **code-reviewer**
3. Correção de bugs ou nova funcionalidade → Usar agent **tdd-guide**
4. Decisões de arquitetura → Usar agent **architect**

## Execução Paralela de Tarefas

Para operações independentes, sempre usar Task paralela:

```markdown
# BOM: Execução paralela
Iniciar 3 agents em paralelo:
1. Agent 1: Análise de segurança do módulo de auth
2. Agent 2: Revisão de performance do sistema de cache
3. Agent 3: Verificação de tipos de utilities

# RUIM: Execução sequencial desnecessária
Primeiro agent 1, depois agent 2, depois agent 3
```

## Análise Multi-perspectiva

Para problemas complexos, usar sub-agents com papéis definidos:
- Revisor de fatos
- Engenheiro sênior
- Especialista em segurança
- Revisor de consistência
- Verificador de redundância