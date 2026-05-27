# Melhores Práticas

Esta seção cobre padrões de codificação, tratamento de erros e loops autônomos.

---

## Padrões de Codificação

### Propósito
Convenções de codificação de linha de base aplicáveis entre projetos, incluindo nomenclatura, legibilidade, imutabilidade e revisão de qualidade de código.

### Quando Usar
- Iniciar novo projeto ou módulo
- Revisar qualidade e manutenibilidade de código
- Refatorar código existente para seguir convenções
- Impor consistência de nomenclatura, formatação ou estrutura
- Configurar regras de linting, formatação ou verificação de tipo
- Adotar convenções de codificação para novos contribuidores

### Princípios Centrais

**1. Legibilidade Primeiro**
- Código é lido mais frequentemente do que escrito
- Nomes claros de variáveis e funções
- Priorizar código autodocumentado ao invés de comentários
- Formatação consistente

**2. Princípio KISS**
- Manter simples, evitar over-engineering
- Evitar otimização prematura
- Clareza > esperteza

**3. Princípio DRY**
- Extrair lógica comum para funções
- Criar componentes reutilizáveis
- Compartilhar utilities entre módulos
- Evitar programação copy-paste

**4. Princípio YAGNI**
- Não construir funcionalidades antes de serem necessárias
- Evitar generalização especulativa
- Adicionar complexidade apenas quando necessário
- Começar simples, refatorar quando preciso

---

## Tratamento de Erros

### Propósito
Padrões robustos de tratamento de erros em TypeScript, Python e Go, incluindo erros tipados, limites de erro, retry, circuit breaker e mensagens de erro amigáveis ao usuário.

### Quando Usar
- Projetar estrutura de tipo ou hierarquia de exceção para novo módulo ou serviço
- Adicionar lógica de retry ou circuit breaker para dependências externas não confiáveis
- Revisar endpoints de API para tratamento de erros ausente
- Implementar mensagens de erro amigáveis e feedback
- Debug de falhas em cascata ou erros silenciosos

### Princípios Centrais

1. **Fail Fast and Loud** - Expor erros nas fronteiras
2. **Erros Tipados Preferíveis a Mensagens de String** - Erros são cidadãos de primeira classe com estrutura
3. **Mensagem de Usuário ≠ Mensagem de Desenvolvedor** - Mostrar texto amigável ao usuário, logar contexto completo no lado servidor
4. **Nunca Silenciar Erros** - Cada bloco `catch` deve tratar, relançar ou logar
5. **Erros São Contrato de API** - Documentar cada código de erro que cliente pode receber

---

## Sistemas Autônomos e loops

### autonomous-agent-harness

**Propósito**: Transformar Claude Code em sistema de agente totalmente autônomo

**Funcionalidades Centrais**:
- Persistência de memória armazenada
- Agendamento de tarefas (similar a cron)
- Dispatch de agente remoto (Dispatch)
- Controle de computador (Computer Use)
- Gerenciamento de fila de tarefas

**Quando Usar**:
- Quando usuário deseja operações autônomas contínuas ou tarefas agendadas
- Quando precisar construir assistente AI pessoal que mantém memória entre sessões
- Quando precisar replicar funcionalidades de frameworks de agente autônomo como Hermes, AutoGPT
- Quando precisar de controle de computador combinado com execução agendada

**Componentes de Arquitetura**:
| Componente | Descrição |
|------------|-----------|
| Crons | Agendamento de tarefas |
| Dispatch | Dispatch de agente remoto |
| Memory | Persistência de memória armazenada |
| Computer Use | Controle de navegador e desktop |

**Mapeamento para alternatives Hermes**:
| Hermes | Equivalente ECC |
|--------|------------------|
| Gateway/Router | Claude Code dispatch + crons |
| Memory System | Claude memory + servidor de memória MCP |
| Tool Registry | Servidores MCP |
| Orchestration | ECC skills + agents |
| Computer Use | computer-use MCP |

---

### autonomous-loops

**Propósito**: Padrões e arquitetura de loop autônomo - de pipeline sequencial simples a sistema DAG multi-agente direcionado por RFC

**Espectro de Padrões de Loop**:
| Padrão | Complexidade | Cenário de Uso |
|---------|-------------|----------------|
| Sequential Pipeline | Baixa | Passos diários de desenvolvimento, workflows scriptizados |
| NanoClaw REPL | Baixa | Sessão interativa persistente |
| Infinite Agentic Loop | Média | Geração de conteúdo paralela, trabalho dirigido por especificação |
| Continuous Claude PR Loop | Média | Projetos iterativos de múltiplos dias, quality gates CI |
| De-Sloppify Pattern | Anexa | Limpeza de qualidade após qualquer etapa de Implementer |
| Ralphinho / RFC-Driven DAG | Alta | Funcionalidades grandes, trabalho paralelomulti-unidade com fila de merge |

**Quando Usar**:
- Configurar workflows de desenvolvimento autônomos
- Selecionar arquitetura de loop apropriada
- Construir pipeline de desenvolvimento contínuo estilo CI/CD
- Executar agentes paralelos com coordenação de merge

**Padrões Chave**:

1. **Sequential Pipeline** - Pipeline simples `claude -p`
2. **NanoClaw REPL** - Loop persistente integrado com suporte a sessão
3. **Infinite Agentic Loop** - Geração paralela dirigida por especificação
4. **Continuous Claude PR Loop** - Scripts shell de produção, cria PRs automaticamente, espera CI, faz merge automaticamente
5. **De-Sloppify Pattern** - Adiciona etapa dedicada de limpeza/refatoração
6. **Ralphinho** - Orquestração DAG multi-agente dirigida por RFC