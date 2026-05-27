# Comandos de Refatoração e Limpeza

## Visão Geral

Comandos de refatoração e limpeza são usados para refatoração de código, limpeza de código morto e auto-atualização.

## Lista de Comandos

### /refactor-clean

**Propósito**: Excluir código morto + mesclar duplicatas

**Descrição**: Identifica e exclui código não usado, mescla fragmentos de código duplicados.

**Analisa**:
- Funções e variáveis não usadas
- Blocos de código duplicados
- Comentários desatualizados
- Imports não usados

**Ferramentas Suportadas**:
- `knip` - Detecção de código morto
- `depcheck` - Detecção de dependências não usadas
- `ts-prune` - Detecção de código morto TypeScript

---

### /auto-update

**Propósito**: Capacidades de auto-atualização

**Descrição**: Atualiza automaticamente dependências e configurações de projeto para versões mais recentes.

**Atualiza**:
- Atualizações de pacotes npm/pip
- Migração de arquivos de configuração
- Migração de código
- Verificação de compatibilidade de versão

---

## Comandos Relacionados

- `/refactor-clean` - Refatoração e limpeza
- `/auto-update` - Auto-atualização