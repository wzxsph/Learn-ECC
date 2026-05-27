# Agentes de Planejamento

Agentes de planejamento são especializados em planejamento de funcionalidades, design de arquitetura técnica e decomposição de etapas de implementação.

## Lista de Agentes

| Nome do Agente | Propósito | Modelo | Ferramentas Principais |
|----------------|-----------|--------|----------------------|
| planner | Especialista em planejamento de implementação de funcionalidades complexas | opus | Read, Grep, Glob |
| code-architect | Especialista em design de arquitetura de funcionalidades | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | Análise de cobertura de testes de PR | sonnet | Read, Grep, Glob, Bash |
| type-design-analyzer | Análise de design de tipos | sonnet | Read, Grep, Glob |
| comment-analyzer | Análise de comentários de código | sonnet | Read, Grep, Glob |

---

## planner

### Nome e Propósito
Especialista em planejamento de implementação de funcionalidades complexas e refatoração. Usado proativamente quando o usuário solicita implementação de funcionalidade, mudanças de arquitetura ou refatoração complexa.

### Capacidades
- Análise de requisitos e criação de planos de implementação detalhados
- Decompor funcionalidades complexas em etapas gerenciáveis
- Identificar dependências e riscos potenciais
- Sugerir ordem ótima de implementação
- Considerar casos de borda e cenários de erro

### Cenários de Uso
- Planejamento de implementação de nova funcionalidade
- Planejamento de mudanças de arquitetura
- Planejamento de refatoração complexa
- Planejamento de limpeza de technical debt

### Ferramentas Utilizadas
- Read: Ler código e documentação existentes
- Grep: Pesquisar padrões e implementações existentes
- Glob: Encontrar arquivos relevantes

### Colaboração com Outros Agentes
- Planos gerados são refinados ainda mais pelo code-architect
- Planos gerados são usados pelo tdd-guide para escrever testes
- Planos gerados são usados pelo code-reviewer para revisão

### Fluxo de Planejamento

1. **Análise de Requisitos**
   - Entender completamente a solicitação de funcionalidade
   - Fazer perguntas de esclarecimento se necessário
   - Identificar critérios de sucesso
   - Listar suposições e restrições

2. **Revisão de Arquitetura**
   - Analisar estrutura do codebase existente
   - Identificar componentes afetados
   - Revisar implementações similares
   - Considerar padrões reutilizáveis

3. **Decomposição de Etapas**
   - Criar etapas detalhadas contendo:
     - Operações concretas e claras
     - Caminhos de arquivo e localizações
     - Dependências entre etapas
     - Complexidade estimada
     - Riscos potenciais

4. **Ordem de Implementação**
   - Ordenar por dependências
   - Agrupar mudanças relacionadas
   - Minimizar troca de contexto
   - Suportar teste incremental

### Formato de Saída do Plano

```markdown
# Plano de Implementação: [Nome da Funcionalidade]

## Visão Geral
[Resumo de 2-3 frases]

## Requisitos
- [Requisito 1]
- [Requisito 2]

## Mudanças de Arquitetura
- [Mudança 1: caminho e descrição do arquivo]
- [Mudança 2: caminho e descrição do arquivo]

## Etapas de Implementação

### Fase 1: [Nome da Fase]
1. **[Nome da Etapa]** (Arquivo: path/to/file.ts)
   - Ação: Ação concreta a ser tomada
   - Razão: Razão para esta etapa
   - Dependência: Nenhuma / Requer etapa X
   - Risco: Baixo/Médio/Alto

2. **[Nome da Etapa]** (Arquivo: path/to/file.ts)
   ...

### Fase 2: [Nome da Fase]
...

## Estratégia de Testes
- Testes de unidade: [Arquivos a serem testados]
- Testes de integração: [Fluxos a serem testados]
- Testes E2E: [Jornadas do usuário a serem testadas]

## Riscos e Mitigação
- **Risco**: [Descrição]
  - Mitigação: [Como lidar]

## Critérios de Sucesso
- [ ] Critério 1
- [ ] Critério 2
```

---

## code-architect

### Nome e Propósito
Design de arquitetura de funcionalidades baseada em compreensão profunda do codebase existente. Fornece blueprint de implementação com arquivos específicos, interfaces, fluxo de dados e ordem de construção.

### Capacidades
- Pesquisar organização de código e convenções de nomenclatura existentes
- Identificar padrões de arquitetura já estabelecidos
- Documentar padrões de teste e limites existentes
- Entender gráfico de dependências antes de propor novas abstrações
- Design de arquitetura que se encaixa naturalmente nos padrões atuais

### Cenários de Uso
- Design de arquitetura de nova funcionalidade
- Design de refatoração de funcionalidade existente
- Otimização de organização de código

### Ferramentas Utilizadas
- Read: Ler código existente
- Grep: Pesquisar padrões e convenções
- Glob: Encontrar arquivos relevantes
- Bash: Executar comandos de diagnóstico

### Colaboração com Outros Agentes
- Design de arquitetura baseado no plano do planner
- Fornecer blueprint para implementação de código
- Colaborar com build-error-resolver para corrigir problemas de build

### Fluxo de Design

1. **Análise de Padrões**
   - Pesquisar organização de código e convenções de nomenclatura existentes
   - Identificar padrões de arquitetura já estabelecidos
   - Notar padrões de teste e limites existentes
   - Entender dependências antes de propor novas abstrações

2. **Design de Arquitetura**
   - Design de funcionalidade para se encaixar naturalmente nos padrões atuais
   - Escolher arquitetura mais simples que atenda aos requisitos
   - Evitar abstrações especulativas (a menos que o codebase já use)

3. **Blueprint de Implementação**
   Para cada componente importante fornecer:
   - Caminho do arquivo
   - Propósito
   - Interfaces-chave
   - Dependências
   - Papel no fluxo de dados

4. **Ordem de Construção**
   Ordenar implementação por dependências:
   1. Tipos e interfaces
   2. Lógica principal
   3. Camada de integração
   4. UI
   5. Testes
   6. Documentação

### Formato de Saída

```markdown
## Arquitetura: [Nome da Funcionalidade]

### Decisões de Design
- Decisão 1: [Razão]
- Decisão 2: [Razão]

### Arquivos a Criar
| Arquivo | Propósito | Prioridade |
|---------|-----------|------------|
| | | |

### Arquivos a Modificar
| Arquivo | Mudança | Prioridade |
|---------|---------|------------|
| | | |

### Fluxo de Dados
[Descrever]

### Ordem de Construção
1. Etapa 1
2. Etapa 2
```

---

## pr-test-analyzer

(Veja [04-Testes.md](./04-Testes.md))

---

## type-design-analyzer

### Nome e Propósito
Analisa se design de tipo torna estados ilegais difíceis ou impossíveis de representar.

### Capacidades
- Avaliação de encapsulamento
- Análise de expressão de invariância
- Avaliação de utilidade de invariância
- Verificação de imposição

### Cenários de Uso
- Revisão de design de sistema de tipos
- Revisão de design de API
- Revisão de modelagem de domínio

### Ferramentas Utilizadas
- Read: Ler definições de tipo
- Grep: Pesquisar uso de tipos
- Glob: Encontrar arquivos de tipo

### Colaboração com Outros Agentes
- Colaborar com code-reviewer para revisão de código
- Colaborar com code-architect para design de arquitetura

### Critérios de Avaliação

1. **Encapsulamento**
   - Detalhes internos estão escondidos
   - Invariantes podem ser violados externamente

2. **Expressão de Invariância**
   - Tipo codifica regras de negócio
   - Estados impossíveis são impedidos no nível de tipo

3. **Utilidade de Invariância**
   - Invariantes previnem bugs reais
   - Alinhado com domínio

4. **Imposição**
   - Invariantes são impostas pelo sistema de tipos
   - Há escape simples se necessário

### Formato de Saída

Para cada tipo revisado:
- Nome e localização do tipo
- Pontuação em quatro dimensões
- Avaliação geral
- Sugestões de melhoria específicas

---

## comment-analyzer

### Nome e Propósito
Analisa precisão, completude, manutenibilidade e risco de putrefação de comentários de código.

### Capacidades
- Verificação de precisão factual
- Verificação de completude
- Avaliação de valor de longo prazo
- Identificação de elementos enganosos

### Cenários de Uso
- Durante revisão de código
- Avaliação de qualidade de documentação
- Identificação de technical debt de comentários

### Ferramentas Utilizadas
- Read: Ler código e comentários
- Grep: Pesquisar comentários e código
- Glob: Encontrar arquivos fonte

### Colaboração com Outros Agentes
- Colaborar com code-reviewer para revisão de código
- Colaborar com doc-updater para atualização de documentação

### Framework de Análise

1. **Precisão Factual**
   - Verificar declarações contra código
   - Verificar descrições de parâmetros e retornos contra implementação
   - Marcar referências desatualizadas

2. **Completude**
   - Verificar se lógica complexa tem explicação suficiente
   - Verificar se efeitos colaterais importantes e casos de borda estão documentados
   - Garantir que APIs públicas têm comentários suficientes

3. **Valor de Longo Prazo**
   - Marcar comentários que apenas reescrevem código
   - Identificar comentários frágeis que apodrecem rapidamente
   - Descobrir dívida de TODO / FIXME / HACK

4. **Elementos Enganosos**
   - Comentários que contradizem código
   - Referências desatualizadas a comportamento removido
   - Comportamento exagerado ou descrito insuficientemente

### Formato de Saída

Descobertas agrupadas por severidade:
- Inaccurate (Impreciso)
- Stale (Desatualizado)
- Incomplete (Incompleto)
- Low-value (Baixo valor)
[Voltar ao Índice de Agentes](../README.md)