# Otimização de Performance

## Estratégia de Seleção de Modelo

**Haiku 4.5** (90% da capacidade do Sonnet, 1/3 do custo):
- Agent leve invoked com alta frequência
- Programação em par e geração de código
- Agent worker em sistemas multi-agent

**Sonnet 4.6** (melhor modelo de codificação):
- Trabalho principal de desenvolvimento
- Orquestrar workflows multi-agent
- Tarefas de codificação complexas

**Opus 4.5** (raciocínio mais profundo):
- Decisões de arquitetura complexas
- Necessidade máxima de raciocínio
- Tarefas de pesquisa e análise

## Gerenciamento de Janela de Contexto

Evitar usar os últimos 20% da janela de contexto:
- Refatoração em larga escala
- Implementação de funcionalidade cruzando múltiplos arquivos
- Debug de interações complexas

Tarefas de baixa sensibilidade a contexto:
- Edição de arquivo único
- Criação de utilitários independentes
- Atualização de documentação
- Correção de bugs simples

## Extended Thinking + Modo Plan

Extended thinking habilitado por default, reservando até 31.999 tokens para raciocínio interno.

Controlar extended thinking:
- **Alternar**: Option+T (macOS) / Alt+T (Windows/Linux)
- **Configurar**: Definir `alwaysThinkingEnabled` em `~/.claude/settings.json`
- **Limite de budget**: `export MAX_THINKING_TOKENS=10000`
- **Modo verbose**: Ctrl+O para ver saída de pensamento

Para tarefas complexas requiring deep reasoning:
1. Garantir que extended thinking está habilitado (default)
2. Habilitar **Modo Plan** para abordagem estruturada
3. Usar múltiplas rodadas de crítica para análise completa
4. Usar sub-agents com papéis diferentes para perspectivas diversas

## Troubleshooting de Build

Se build falha:
1. Usar agent **build-error-resolver**
2. Analisar mensagens de erro
3. Corrigir incrementalmente
4. Verificar após cada correção