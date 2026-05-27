# Sistema de Hooks

## Tipos de Hook

- **PreToolUse**: Antes da execução da ferramenta (validação, modificação de parâmetros)
- **PostToolUse**: Após a execução da ferramenta (auto-formatação, verificações)
- **Stop**: Quando a sessão termina (verificação final)

## Aceitação Automática de Permissões

Usar com cautela:
- Habilitar para planos definidos e confiáveis
- Desabilitar para trabalho exploratório
- Nunca usar flag dangerously-skip-permissions
- Configurar `allowedTools` em `~/.claude.json`

## Melhores Práticas de TodoWrite

Usar ferramenta TodoWrite para:
- Rastrear progresso de tarefas multi-etapa
- Validar entendimento de instruções
- Habilitar steering em tempo real
- Mostrar etapas de implementação granulares

Lista de todos revela:
- Passos fora de ordem
- Itens faltando
- Itens extras desnecessários
- Granularidade incorreta
- Requisitos mal interpretados