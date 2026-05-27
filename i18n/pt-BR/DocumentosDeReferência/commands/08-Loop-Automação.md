# Comandos de Loop e Automação

## Visão Geral

Comandos de loop e automação são usados para iniciar e gerenciar modos de trabalho de loop autônomo.

## Lista de Comandos

### /loop-start

**Propósito**: Iniciar modo de loop autônomo gerenciado com padrões seguros e condições de parada claras

**Descrição**: Inicia um modo de loop autônomo gerenciado com padrões seguros default e condições de parada claras.

**Uso**: `/loop-start [pattern] [--mode safe|fast]`

**Parâmetros**:

| Parâmetro | Valor | Descrição |
|---|---|---|
| `pattern` | `sequential` | Executar tarefas sequencialmente, uma após a outra |
| | `continuous-pr` | Loop contínuo de criação e merge de PR |
| | `rfc-dag` | Workflow DAG direcionado baseado em RFC |
| | `infinite` | Loop infinito (requer condições de parada claras) |
| `--mode` | `safe` (default) | Quality gates e checkpoints rigorosos |
| | `fast` | Reduzir gates para velocidade |

**Fluxo de Trabalho**:
1. Confirmar estado do repositório e estratégia de branch
2. Selecionar modo de loop e estratégia de nível de modelo relacionada
3. Habilitar hooks/profile necessários para o modo selecionado
4. Criar plano de loop e escrever runbook em `.claude/plans/`
5. Imprimir comandos de início e monitoramento

**Verificações de Segurança Obrigatórias**:
- Verificar que testes passam antes do primeiro loop
- Garantir que `ECC_HOOK_PROFILE` não está desabilitado globalmente
- Garantir que loop tem condições de parada claras

**Passagem de Parâmetros**:
- `$ARGUMENTS`: `[pattern]` e opcional `[--mode safe|fast]`
- Pattern é opcional (default é sequential)
- Flag mode é opcional (default é safe)

**Melhores Práticas**:
- Garantir condições de parada claras antes de usar modo infinite
- Usar modo fast apenas em código bem testado
- Monitorar progresso do loop com `/loop-status`

**Armadilhas Comuns**:
- Iniciar loop sem verificar testes
- Usar modo infinite sem condições de parada claras
- Tentar usar modo safe com hooks globalmente desabilitados

**Integração com Outros Comandos**:
- Usar `/loop-status` para monitorar loops em execução
- Usar `/santa-loop` para loops estilo Santa
- Usar `/checkpoint` para criar checkpoints em nós críticos

---

### /loop-status

**Propósito**: Verificar status do loop em execução

**Descrição**: Ver o status e progresso do loop autônomo em execução.

---

### /santa-loop

**Propósito**: Loop autônomo estilo Santa

**Descrição**: Um modo especial de loop autônomo estilo Santa.

---

## Comandos Relacionados

- `/loop-start` - Iniciar loop
- `/loop-status` - Verificar status
- `/santa-loop` - Estilo Santa