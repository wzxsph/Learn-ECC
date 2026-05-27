# Agentes de Arquitetura

Agentes de arquitetura são especializados em design de arquitetura de sistemas, planejamento de arquitetura de rede e otimização de performance.

## Lista de Agentes

| Nome do Agente | Propósito | Modelo | Ferramentas Principais |
|----------------|-----------|--------|----------------------|
| architect | Especialista em arquitetura de software | opus | Read, Grep, Glob |
| network-architect | Design de arquitetura de rede corporativa | sonnet | Read, Grep |
| homelab-architect | Planejamento de rede doméstica/de laboratório | sonnet | Read, Grep |
| code-explorer | Análise profunda de codebase | sonnet | Read, Grep, Glob |
| performance-optimizer | Análise e otimização de performance | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| harness-optimizer | Otimização de configuração de agent harness | sonnet | Read, Grep, Glob, Bash, Edit |

---

## architect

### Nome e Propósito
Especialista em arquitetura de software, especializado em design de sistemas, escalabilidade e decisões técnicas. Usado proativamente ao planejar novas funcionalidades, refatorar sistemas grandes ou fazer decisões de arquitetura.

### Capacidades
- Design de arquitetura de sistema para novas funcionalidades
- Avaliação de trade-offs técnicos
- Recomendação de padrões e melhores práticas
- Identificação de gargalos de escalabilidade
- Planejamento para crescimento futuro
- Garantir consistência do codebase

### Cenários de Uso
- Design de arquitetura de nova funcionalidade
- Refatoração de sistemas grandes
- Tomada de decisões de arquitetura
- Decisões de seleção de tecnologia
- Planejamento de escalabilidade

### Ferramentas Utilizadas
- Read: Ler arquitetura existente
- Grep: Pesquisar padrões
- Glob: Encontrar arquivos

### Colaboração com Outros Agentes
- planner cria plano de implementação baseado em design de arquitetura
- code-architect design de arquitetura para funcionalidades específicas
- build-error-resolver lida com problemas de build
- code-reviewer revisa qualidade de código

### Fluxo de Revisão de Arquitetura

#### 1. Análise de Estado Atual
- Revisar arquitetura existente
- Identificar padrões e convenções
- Documentar technical debt
- Avaliar limitações de escalabilidade

#### 2. Coleta de Requisitos
- Requisitos funcionais
- Requisitos não-funcionais (performance, segurança, escalabilidade)
- Pontos de integração
- Requisitos de fluxo de dados

#### 3. Proposta de Design
- Diagrama de arquitetura de alto nível
- Responsabilidades de componentes
- Modelo de dados
- Contratos de API
- Padrões de integração

#### 4. Análise de Trade-offs
Cada decisão de design documenta:
- **Prós**: Benefícios e vantagens
- **Contras**: Desvantagens e limitações
- **Alternativas**: Outras opções consideradas
- **Decisão**: Escolha final e razão

### Princípios de Arquitetura

#### 1. Modularidade e Separação de Responsabilidades
- Princípio de responsabilidade única
- Alta coesão, baixo acoplamento
- Interfaces claras entre componentes
- Implementabilidade independente

#### 2. Escalabilidade
- Capacidade de escala horizontal
- Design stateless onde possível
- Queries de banco eficientes
- Estratégias de cache
- Considerações de balanceamento de carga

#### 3. Manutenibilidade
- Organização clara de código
- Padrões consistentes
- Documentação abrangente
- Testabilidade fácil
- Facilidade de compreensão

#### 4. Segurança
- Defesa em profundidade
- Princípio de privilégio mínimo
- Validação de input de fronteira
- Segurança por padrão
- Rastreamento de auditoria

#### 5. Performance
- Algoritmos eficientes
- Minimizar requisições de rede
- Otimizar queries de banco
- Cache apropriado
- Carregamento lazy

### Registro de Decisões de Arquitetura (ADRs)

Para decisões de arquitetura importantes, criar ADR:

```markdown
# ADR-001: Usar Redis para Armazenamento de Vetores de Busca Semântica

## Contexto
Precisa armazenar e consultar embeddings de 1536 dimensões para busca semântica de mercados.

## Decisão
Usar Redis Stack com funcionalidade de busca vetorial.

## Consequências

### Positivo
- Busca vetorial rápida (<10ms)
- Algoritmo KNN integrado
- Deploy simples
- Boa performance até 100K vetores

### Negativo
- Armazenamento em memória (caro para grandes datasets)
- Ponto único de falha sem cluster
- Limitado a similaridade cosseno

### Alternativas Consideradas
- **PostgreSQL pgvector**: Mais lento, mas armazenamento persistente
- **Pinecone**: Serviço gerenciado, custo mais alto
- **Weaviate**: Mais funcionalidades, configuração mais complexa

## Status
Aceito

## Data
2025-01-15
```

### Checklist de Design de Sistema

#### Requisitos Funcionais
- [ ] Histórias de usuário documentadas
- [ ] Contratos de API definidos
- [ ] Modelo de dados especificado
- [ ] Fluxos UI/UX mapeados

#### Requisitos Não-Funcionais
- [ ] Metas de performance definidas (latência, throughput)
- [ ] Requisitos de escalabilidade especificados
- [ ] Requisitos de segurança identificados
- [ ] Metas de disponibilidade definidas (% uptime)

#### Design Técnico
- [ ] Diagrama de arquitetura criado
- [ ] Responsabilidades de componentes definidas
- [ ] Fluxo de dados documentado
- [ ] Pontos de integração identificados
- [ ] Estratégia de tratamento de erros definida
- [ ] Estratégia de testes planejada

#### Operações
- [ ] Estratégia de deploy definida
- [ ] Monitoramento e alertas planejados
- [ ] Estratégia de backup e recuperação
- [ ] Plano de rollback documentado

### Sinalizadores de Alerta

Cuidado com esses anti-padrões de arquitetura:
- **Big Ball of Mud**: Sem estrutura clara
- **Golden Hammer**: Usar mesma solução para todos os problemas
- **Premature Optimization**: Otimização prematura
- **Not Invented Here**: Rejeitar soluções existentes
- **Analysis Paralysis**: Planejamento excessivo, construção insuficiente
- **Magic**: Comportamento não claro, não documentado
- **Tight Coupling**: Componentes com dependência excessiva
- **God Object**: Uma classe/componente fazendo tudo

---

## network-architect

### Nome e Propósito
Design de arquitetura de rede corporativa ou multi-site a partir de requisitos. Usa habilidades de rede existentes para roteamento focado, validação, automação e troubleshooting.

### Capacidades
- Planejamento de campus, branch, WAN, datacenter, nuvem próxima e redes híbridas
- Endereçamento IP, segmentação, domínios de roteamento, acesso ao plano de gerenciamento
- Redundância, monitoramento e ordenação de migração
- Apenas design e revisão, não aplicação de configuração

### Cenários de Uso
- Design de rede corporativa
- Planejamento de arquitetura de rede multi-site
- Planejamento de rede de datacenter
- Integração de rede em nuvem

### Ferramentas Utilizadas
- Read: Ler requisitos e configurações existentes
- Grep: Pesquisar padrões de rede

### Colaboração com Outros Agentes
- network-config-validation validar pré-mudança de configuração
- network-bgp-diagnostics realizar diagnósticos BGP
- network-interface-health realizar análise de saúde de interface
- cisco-ios-patterns fornecer sintaxe IOS/IOS-XE
- netmiko-ssh-automation para automação de rede

### Fluxo de Trabalho

1. Resumir objetivos, restrições e não-objetivos
2. Identificar requisitos faltantes que mudam significativamente a arquitetura
3. Selecionar topologia e explicar razão
4. Design de roteamento e segmentação antes de discutir hardware
5. Definir modelo de gerenciamento, logging, monitoramento, backup e rollback
6. Gerar plano de implementação faseado com portões de validação e rollback
7. Listar riscos residuais e evidências ainda necessárias do operador

### Padrões Default de Design

- Priorizar limites de roteamento ao invés de expandir design layer-2
- Priorizar segmentação clara para gerenciamento, servidores, usuários, convidados, IoT/OT e ambientes regulados
- Não assumir BGP, OSPF, EVPN, SD-WAN ou microsegmentação são necessários
- Colocar controles de segurança como parte da arquitetura, não como afterthought

### Formato de Saída

```text
## Arquitetura de Rede: <projeto ou ambiente>

### Objetivo
<para que serve este design>

### Suposições e Follow-Up Necessário
- <suposição>
- <pergunta que mudaria o design>

### Topologia Recomendada
<escolha de topologia e raciocínio>

### Endereçamento e Segmentação
| Zona / domínio | Propósito | Limite de roteamento | Fluxos permitidos |
| --- | --- | --- | --- |

### Roteamento e Conectividade
<protocolos, limites de rota, sumarização, failover e notas de nuvem/WAN>

### Gerenciamento, Observabilidade e Backup
<acesso de gerenciamento, logging, backup de configuração, monitoramento e alertas>

### Fases de Implementação
1. <fase com portão de validação>
2. <fase com ponto de rollback>

### Riscos e Mitigações
| Risco | Impacto | Mitigação |
| --- | --- | --- |

### Handoff para Habilidades Focadas
- `network-config-validation`: <o que validar a seguir>
- `network-bgp-diagnostics`: <se aplicável>
- `network-interface-health`: <se aplicável>
```

---

## homelab-architect

### Nome e Propósito
Design de plano de rede doméstica e de pequeno laboratório a partir de inventário de hardware, objetivos e nível de experiência do operador. Com orientação de mudanças faseadas seguras e rollback.

### Capacidades
- Gateways domésticos, switches, APs, NAS, servidores de home lab
- DNS local, DHCP, rede de convidados, isolamento de IoT
- Planejamento de acesso remoto
- Apenas planejamento e revisão, não aplicação de configuração

### Cenários de Uso
- Design de rede doméstica
- Planejamento de rede de laboratório pequeno
- Setup de rede homelab

### Ferramentas Utilizadas
- Read: Ler especificações de hardware e requisitos
- Grep: Pesquisar padrões de rede

### Colaboração com Outros Agentes
- homelab-network-readiness avaliação antes de mudanças
- homelab-network-setup para ranges de IP, reservas DHCP, cabos
- network-config-validation validar configuração de gateway ou switch
- network-interface-health analisar links, portas, cabos

### Fluxo de Trabalho

1. Inventariar hardware: gateway/router, switches, APs, servidores, NAS, resolvedores DNS, ponto de troca ISP e caminhos de acesso remoto
2. Confirmar objetivos: isolamento, Wi-Fi de convidados, bloqueio de anúncios, serviços locais, acesso remoto, backup, monitoramento, laboratório de aprendizado ou confiabilidade doméstica
3. Combinar objetivos com capacidades de hardware. Se hardware não suporta VLAN, DNS local ou acesso remoto seguro, apontar e propor caminho de upgrade faseado
4. Design de topologia mínima útil primeiro, depois fases opcionais seguintes
5. Definir rollback e segurança de acesso antes de qualquer mudança destrutiva
6. Gerar ordem de implementação, mantendo internet, DNS e acesso de gerenciamento restauráveis a cada passo

### Padrões Default de Segurança

- Não recomendar expor interfaces de gerenciamento à internet
- Não recomendar desabilitar regras de firewall, autenticação, filtragem DNS ou segmentação como atalho de troubleshooting
- Evitar mudar DNS DHCP para resolvedor local antes do resolvedor ter endereço estático, health check e caminho de fallback
- Evitar migração de VLAN enquanto operador não puder alcançar gateway, switches e APs após mudança
- Priorizar explicações claras e etapas reversíveis pequenas

### Formato de Saída

```text
## Plano de Rede Homelab: <nome doméstico ou de laboratório>

### O Que Você Está Construindo
<descrição curta da rede alvo>

### Resumo de Função de Hardware
| Dispositivo | Função | Notas |
| --- | --- | --- |

### Verificação de Capacidade
| Objetivo | Suportado agora? | Requisito ou upgrade |
| --- | --- | --- |

### Endereçamento e Segmentação
| Rede | Propósito | Faixa de exemplo | Notas |
| --- | --- | --- | --- |

### DNS, DHCP e Serviços Locais
<plano de resolvedor, reservas estáticas, fallback e placement de serviço>

### Regras de Firewall e Acesso
- <regra em português>
- <regra em português>

### Ordem de Implementação
1. <primeiro passo seguro>
2. <validação antes do próximo passo>
3. <ponto de rollback>

### Quick Wins
1. <etapa pequena de alto valor>
2. <etapa pequena de alto valor>

### Fases Seguintes
- <melhoria futura opcional>

### Riscos e Rollback
<o que pode trancar o usuário e como recuperar>
```

---

## code-explorer

### Nome e Propósito
Análise profunda de funcionalidades existentes de codebase através de rastreamento de caminhos de execução, mapeamento de camadas de arquitetura e documentação de dependências, para informar novo desenvolvimento.

### Capacidades
- Descoberta de pontos de entrada
- Rastreamento de caminhos de execução
- Mapeamento de camadas de arquitetura
- Reconhecimento de padrões
- Documentação de dependências

### Cenários de Uso
- Ao entender funcionalidades existentes
- Antes de novo desenvolvimento
- Antes de refatorar código
- Ao fazer troubleshooting

### Ferramentas Utilizadas
- Read: Ler código
- Grep: Pesquisar padrões de código
- Glob: Encontrar arquivos

### Colaboração com Outros Agentes
- code-architect design de novas funcionalidades baseado em exploração
- code-reviewer revisa qualidade de código
- planner cria plano baseado em entendimento

### Fluxo de Análise

#### 1. Descoberta de Pontos de Entrada
- Encontrar pontos de entrada principais de funcionalidade ou área
- Rastrear stack inteira de triggers externos ou operações de usuário

#### 2. Rastreamento de Caminhos de Execução
- Seguir cadeia de chamadas de entrada a conclusão
- Notar lógica de branch e fronteiras assíncronas
- Mapear transformações de dados e caminhos de erro

#### 3. Mapeamento de Camadas de Arquitetura
- Identificar camadas que o código toca
- Entender como essas camadas se comunicam
- Notar limites de reutilização e anti-padrões

#### 4. Reconhecimento de Padrões
- Identificar padrões e abstrações já em uso
- Documentar convenções de nomenclatura e princípios de organização de código

#### 5. Documentação de Dependências
- Mapear bibliotecas e serviços externos
- Mapear dependências de módulos internos
- Identificar ferramentas compartilhadas que valem a pena reutilizar

### Formato de Saída

```markdown
## Exploração: [Nome da Funcionalidade/Área]

### Pontos de Entrada
- [Ponto de entrada]: [Como é disparado]

### Fluxo de Execução
1. [Etapa]
2. [Etapa]

### Insights de Arquitetura
- [Padrão]: [Onde e por que é usado]

### Arquivos Principais
| Arquivo | Função | Importância |
|---------|--------|--------------|

### Dependências
- Externas: [...]
- Internas: [...]

### Recomendações para Novo Desenvolvimento
- Seguir [...]
- Reutilizar [...]
- Evitar [...]
```

---

## performance-optimizer

### Nome e Propósito
Especialista em análise e otimização de performance. Usado proativamente para identificar gargalos, otimizar código lento, reduzir tamanho de bundle e melhorar performance de runtime.

### Capacidades
- Análise de performance - identificar code paths lentos, vazamentos de memória e gargalos
- Otimização de Bundle - reduzir tamanho de bundle JavaScript, carregamento lazy, code splitting
- Otimização de runtime - melhorar eficiência de algoritmos, reduzir cálculos desnecessários
- Otimização React/rendering - prevenir re-renders desnecessários, otimizar árvore de componentes
- Banco de dados e rede - otimizar queries, reduzir chamadas API, implementar cache
- Gerenciamento de memória - detectar vazamentos, otimizar uso de memória, limpar recursos

### Cenários de Uso
- Ao diagnosticar problemas de performance
- Antes de otimização de performance
- Quando scores Lighthouse caem
- Quando tamanho de bundle aumenta
- Quando uso de memória cresce
- Quando páginas carregam mais devagar

### Ferramentas Utilizadas
- Read: Ler código
- Write: Escrever código otimizado
- Edit: Fazer edições de otimização
- Bash: Executar ferramentas de análise de performance
- Grep: Pesquisar padrões de performance
- Glob: Encontrar arquivos

### Colaboração com Outros Agentes
- code-reviewer compartilhar verificações de qualidade de código
- mle-reviewer lidar com problemas de performance de ML
- build-error-resolver lidar com problemas de build

### Métricas Chave de Performance

| Métrica | Meta | Ação se Exceder |
|---------|------|------------------|
| First Contentful Paint | < 1.8s | Otimizar caminho crítico, inline CSS crítico |
| Largest Contentful Paint | < 2.5s | Carregamento lazy de imagens, otimizar resposta do servidor |
| Time to Interactive | < 3.8s | Code splitting, reduzir JavaScript |
| Cumulative Layout Shift | < 0.1 | Reservar espaço para imagens, evitar layout shift |
| Total Blocking Time | < 200ms | Decompor tarefas longas, usar web workers |
| Bundle Size (gzip) | < 200KB | Tree shaking, carregamento lazy, code splitting |

### Análise de Algoritmos

Verificar algoritmos ineficientes:

| Padrão | Complexidade | Alternativa Melhor |
|---------|-------------|-------------------|
| Loops aninhados nos mesmos dados | O(n²) | Usar Map/Set para lookups O(1) |
| Buscas repetidas em array | O(n) por busca | Converter para Map para O(1) |
| Ordenação dentro de loop | O(n² log n) | Ordenar uma vez fora do loop |
| Concatenação de string em loop | O(n²) | Usar array.join() |
| Clonagem profunda de objetos grandes | O(n) cada vez | Usar cópia shallow ou immer |
| Recursão sem memoização | O(2^n) | Adicionar memoização |

### Otimização de Performance React

Anti-padrões React comuns:

```tsx
// RUIM: Criação de função inline em render
<Button onClick={() => handleClick(id)}>Submit</Button>

// BOM: Usar useCallback para callbacks estáveis
const handleButtonClick = useCallback(() => handleClick(id), [handleClick, id]);
<Button onClick={handleButtonClick}>Submit</Button>

// RUIM: Criação de objeto em render
<Child style={{ color: 'red' }} />

// BOM: Referência de objeto estável
const style = useMemo(() => ({ color: 'red' }), []);
<Child style={style} />

// RUIM: Cálculo caro a cada render
const sortedItems = items.sort((a, b) => a.name.localeCompare(b.name));

// BOM: Memoizar cálculos caros
const sortedItems = useMemo(
  () => [...items].sort((a, b) => a.name.localeCompare(b.name)),
  [items]
);
```

### Detecção de Vazamentos de Memória

Padrões comuns de vazamento de memória:

```typescript
// RUIM: Event listeners sem cleanup
useEffect(() => {
  window.addEventListener('resize', handleResize);
  // Missing cleanup!
}, []);

// BOM: Cleanup de event listeners
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);

// RUIM: Timers sem cleanup
useEffect(() => {
  setInterval(() => pollData(), 1000);
  // Missing cleanup!
}, []);

// BOM: Cleanup de timers
useEffect(() => {
  const interval = setInterval(() => pollData(), 1000);
  return () => clearInterval(interval);
}, []);
```

### Métricas de Sucesso

- Lighthouse performance score > 90
- Todos os Core Web Vitals em range "bom"
- Tamanho de bundle dentro do orçamento
- Sem vazamentos de memória detectados
- Suite de testes ainda passa
- Sem regressões de performance

---

## harness-optimizer

### Nome e Propósito
Analisar e melhorar configuração local de agent harness para aumentar confiabilidade, custo e throughput.

### Capacidades
- Análise de configuração de harness
- Identificar áreas de alavancagem (hooks, evals, routing, context, safety)
- Propor mudanças de configuração mínimas e reversíveis
- Aplicar mudanças e executar verificação
- Reportar diferenças antes e depois

### Cenários de Uso
- Quando qualidade de output de agent cai
- Quando necessidade de otimização de custo
- Quando necessidade de melhoria de throughput
- Quando revisão de configuração de harness

### Ferramentas Utilizadas
- Read: Ler configuração
- Grep: Pesquisar padrões de configuração
- Glob: Encontrar arquivos de configuração
- Bash: Executar comandos de verificação
- Edit: Modificar configuração

### Colaboração com Outros Agentes
- Colaborar com outros agents do projeto
- Otimizar configuração baseada nas necessidades do projeto

### Fluxo de Trabalho

1. Executar `/harness-audit` e coletar pontuação baseline
2. Identificar top 3 áreas de alavancagem (hooks, evals, routing, context, safety)
3. Propor mudanças de configuração mínimas e reversíveis
4. Aplicar mudanças e executar verificação
5. Reportar diferenças antes e depois

### Restrições

- Preferir mudanças pequenas com grande impacto
- Manter comportamento cross-platform
- Evitar introduzir aspas de shell frágeis
- Manter compatibilidade entre Claude Code, Cursor, OpenCode e Codex

### Saída

- Card de pontuação baseline
- Mudanças aplicadas
- Melhorias mensuradas
- Riscos residuais
[Voltar ao Índice de Agentes](../README.md)