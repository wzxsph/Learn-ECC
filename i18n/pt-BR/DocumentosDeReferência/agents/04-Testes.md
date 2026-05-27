# Agentes de Testes

Agentes de testes são especializados em desenvolvimento dirigido por testes, testes end-to-end, análise de cobertura de testes e garantia de qualidade.

## Lista de Agentes

| Nome do Agente | Propósito | Modelo | Ferramentas Principais |
|----------------|-----------|--------|----------------------|
| tdd-guide | Especialista em TDD | sonnet | Read, Write, Edit, Bash, Grep |
| e2e-runner | Especialista em testes end-to-end | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| mle-reviewer | Revisão de código de engenharia ML (training/inference/monitoring) | sonnet | Read, Grep, Glob, Bash |
| pr-test-analyzer | Análise de cobertura de testes de PR | sonnet | Read, Grep, Glob, Bash |

---

## tdd-guide

### Nome e Propósito
Especialista em desenvolvimento dirigido por testes, impondo metodologia de escrita de testes primeiro. Usado proativamente para escrever novas funcionalidades, corrigir bugs ou refatorar código. Garante cobertura de testes de 80%+.

### Capacidades
- Imposição de metodologia de testes primeiro
- Orientação do ciclo Red-Green-Refactor
- Garantia de cobertura de testes 80%+
- Escrita de suítes de testes abrangentes (unit, integração, E2E)
- Captura de casos de borda antes da implementação

### Cenários de Uso
- Ao escrever nova funcionalidade
- Ao corrigir bugs
- Ao refatorar código
- Quando workflow TDD é necessário

### Ferramentas Utilizadas
- Read: Ler código existente
- Write: Escrever testes
- Edit: Modificar testes e implementação
- Bash: Executar comandos de teste
- Grep: Pesquisar padrões de teste

### Colaboração com Outros Agentes
- code-reviewer revisa código após ciclo TDD
- build-error-resolver corrige erros de build de testes
- e2e-runner executa testes E2E para jornadas de usuário críticas

### Fluxo de Trabalho TDD

#### 1. Escrever Teste Primeiro (RED)
Escrever teste que descreve comportamento esperado e falha.

#### 2. Executar Teste - Verificar que FALHA
```bash
npm test
```

#### 3. Escrever Implementação Mínima (GREEN)
Escrever apenas código suficiente para fazer o teste passar.

#### 4. Executar Teste - Verificar que PASSA

#### 5. Refatorar (IMPROVE)
Remover duplicação, melhorar nomes, otimizar - testes devem permanecer verdes.

#### 6. Verificar Cobertura
```bash
npm run test:coverage
# Requer: 80%+ de cobertura de branch, função, linha, declaração
```

### Casos de Borda que Devem Ser Testados

1. **Input Null/Undefined**
2. **Array/string vazio**
3. **Passagem de tipo inválido**
4. **Valores de borda** (mínimo/máximo)
5. **Caminhos de erro** (falha de rede, erros de banco de dados)
6. **Condições de corrida** (operações concorrentes)
7. **Dados grandes** (performance com 10k+ itens)
8. **Caracteres especiais** (Unicode, emoji, caracteres SQL)

### Anti-padrões - Devem Ser Evitados

- Testar detalhes de implementação (estado interno) ao invés de comportamento
- Testes dependentes uns dos outros (estado compartilhado)
- Asserções poucas (passam mas não verificam nada)
- Dependências externas não mockadas (Supabase, Redis, OpenAI, etc.)

### Checklist de Qualidade

- [ ] Todas as funções públicas têm testes unitários
- [ ] Todos os endpoints de API têm testes de integração
- [ ] jornadas de usuário críticas têm testes E2E
- [ ] Casos de borda estão cobertos (null, empty, invalid)
- [ ] Caminhos de erro estão testados (não apenas happy path)
- [ ] Dependências externas estão mockadas
- [ ] Testes são independentes (sem estado compartilhado)
- [ ] Asserções são específicas e significativas
- [ ] Cobertura é 80%+

### Apêndice TDD Eval-Driven v1.8

Integrar eval-driven development ao fluxo TDD:

1. Definir capabilities + evals de regressão antes da implementação
2. Executar baseline e capturar características de falha
3. Implementar mudança mínima que passa
4. Re-executar testes e evals; reportar pass@1 e pass@3
5. Caminhos críticos de release devem alcançar estabilidade pass^3 antes de merge

---

## e2e-runner

### Nome e Propósito
Especialista em testes end-to-end, usando Vercel Agent Browser (preferido) e Playwright fallback. Usado proativamente para gerar, manter e executar testes E2E.

### Capacidades
- Criação de jornadas de teste - Escrever testes para jornadas de usuário
- Manutenção de testes - Manter testes sincronizados com mudanças de UI
- Gerenciamento de testes instáveis - Identificar e isolar testes instáveis
- Relatórios de teste - Gerar relatórios HTML e JUnit XML
- Integração CI/CD - Garantir que testes executam de forma confiável em pipeline
- Gerenciamento de screenshot/video/trace

### Cenários de Uso
- Quando jornadas de usuário críticas precisam de verificação
- Quando verificação end-to-end é necessária
- Quando testes E2E executam em pipeline CI/CD
- Quando problemas de integração são descobertos

### Ferramentas Utilizadas
- Read: Ler conteúdo de página
- Write: Escrever testes
- Edit: Modificar testes
- Bash: Executar comandos Playwright/Agent Browser
- Grep: Pesquisar testes
- Glob: Encontrar arquivos de teste

### Colaboração com Outros Agentes
- tdd-guide escreve testes unitários/de integração
- code-reviewer revisa qualidade de teste
- mle-reviewer revisa testes relacionados a ML

### Ferramenta Principal: Agent Browser

**Preferir Agent Browser sobre Playwright bruto** - seletores semânticos, otimizado por AI, auto-wait, baseado em Playwright.

```bash
# Setup
npm install -g agent-browser && agent-browser install

# Fluxo de trabalho principal
agent-browser open https://example.com
agent-browser snapshot -i          # Obter elementos com refs [ref=e1]
agent-browser click @e1            # Clicar por ref
agent-browser fill @e2 "text"     # Preencher input por ref
agent-browser wait visible @e5     # Esperar elemento visível
agent-browser screenshot result.png
```

### Fallback: Playwright

Quando Agent Browser não está disponível, usar Playwright diretamente.

```bash
npx playwright test                        # Executar todos os testes E2E
npx playwright test tests/auth.spec.ts     # Executar arquivo específico
npx playwright test --headed               # Navegador visível
npx playwright test --debug                # Usar inspector para debug
npx playwright test --trace on             # Executar com trace
npx playwright show-report                 # Ver relatório HTML
```

### Fluxo de Trabalho

#### 1. Planejar
- Identificar jornadas de usuário críticas (auth, funcionalidade core, pagamento, CRUD)
- Definir cenários: happy path, casos de borda, casos de erro
- Ordenar por risco: HIGH (financeiro, auth), MEDIUM (search, navegação), LOW (otimização de UI)

#### 2. Criar
- Usar padrão Page Object Model (POM)
- Preferir seletores `data-testid`
- Adicionar asserções em passos críticos
- Capturar screenshot em momentos críticos
- Usar waits apropriados (nunca `waitForTimeout`)

#### 3. Executar
- Executar localmente 3-5 vezes para verificar instabilidade
- Usar `test.fixme()` ou `test.skip()` para isolar testes instáveis
- Fazer upload de artifact para CI

### Princípios Chave

- **Usar seletores semânticos**: `[data-testid="..."]` > seletores CSS > XPath
- **Esperar condições não tempo**: `waitForResponse()` > `waitForTimeout()`
- **Auto-wait integrado**: `page.locator().click()` espera automaticamente; `page.click()` bruto não espera
- **Isolar testes**: Cada teste deve ser independente; sem estado compartilhado
- **Falhar rápido**: Usar asserções `expect()` em cada passo crítico
- **Retry com trace**: Configurar `trace: 'on-first-retry'` para debug de falhas

### Tratamento de Testes Instáveis

```typescript
// Isolar
test('flaky: market search', async ({ page }) => {
  test.fixme(true, 'Flaky - Issue #123')
})

// Identificar instabilidade
// npx playwright test --repeat-each=10
```

Causas comuns: condições de corrida (usar seletores com auto-wait), timing de rede (esperar resposta), timing de animação (esperar `networkidle`).

### Métricas de Sucesso

- Todas as jornadas críticas passam (100%)
- Taxa de aprovação geral > 95%
- Taxa de instabilidade < 5%
- Duração dos testes < 10 minutos
- Artifact fazer upload e ser acessível

---

## mle-reviewer

### Nome e Propósito
Especialista em revisão de engenharia de machine learning de produção, focado em contratos de dados, pipelines de features, reprodutibilidade de training, avaliação offline/online, serving de modelo, monitoramento e rollback.

### Capacidades
- Revisão de qualidade de definição de problema e decisões
- Revisão de métricas, limiares e análise de erros
- Verificação de contratos de dados e detecção de vazamento
- Verificação de reprodutibilidade de training
- Revisão de fluxo de avaliação e promotion
- Verificação de segurança de serving e deploy
- Planejamento de monitoramento e resposta a eventos

### Cenários de Uso
- Quando há mudanças em código de ML, MLOps, training de modelo
- Quando há mudanças em código de inference, feature store, avaliação
- Quando há decisões de promotion de modelo

### Ferramentas Utilizadas
- Read: Ler código e configuração de ML
- Grep: Pesquisar padrões
- Glob: Encontrar arquivos
- Bash: Executar pytest, ruff, mypy

### Colaboração com Outros Agentes
- python-reviewer lidar com estilo Python, tipos, tratamento de erros
- pytorch-build-resolver lidar com falhas de tensor/CUDA/training
- database-reviewer lidar com tabelas de features, armazenamento de labels
- security-reviewer lidar com secrets, PII, segurança de pickle
- performance-optimizer lidar com latência, memória, utilização de GPU
- build-error-resolver lidar com CI, dependências, falhas de extensões nativas
- pr-test-analyzer analisar cobertura de testes
- silent-failure-hunter descobrir falhas silenciosas de pipeline
- e2e-runner executar testes E2E de fluxo de produto
- a11y-architect revisar acessibilidade de predição
- doc-updater atualizar documentação

### Áreas Chave de Revisão

#### Definição de Problema e Qualidade de Decisão
- Mudança parte de decisões de usuário ou sistema, não preferência de arquitetura de modelo
- Stakeholders e custo de falha são claros
- Seleção de métrica segue erro budget

#### Métricas, Limiares e Análise de Erros
- Baseline e comportamento atual de produção são comparados
- Limiares e configuração são decisões de produto
- Falsos positivos e falsos negativos são verificados diretamente

#### Contratos de Dados e Vazamento
- Granularidade de entidade, chave primária, timestamp são claros
- Divisão respeita agrupamento temporal, usuário/entidade
- Conexões de feature são temporalmente corretas
- Atributos sensíveis são excluídos ou tratados legitimamente

#### Reprodutibilidade de Training
- Training pode executar de código, configuração, versão de dataset, seed
- Hyperparâmetros, preprocessamento, versões de dependência estão documentados
- Aleatoriedade e não-determinismo de GPU são intencionalmente tratados

#### Avaliação e Promotion
- Métricas são comparadas com baseline e modelo atual de produção
- Portões de promotion são declarados antes da seleção
- Métricas de slice cobrem filas importantes

#### Serving e Deploy
- Transformações de training e serving são compartilhadas ou equivalentes testadas
- Schema de input rejeita features desatualizadas, faltantes, inválidas
- Caminhos de inference têm timeout, limites de recursos, lógica de fallback

#### Monitoramento e Resposta a Eventos
- Monitoramento cobre saúde de serviço, drift de feature, drift de predição
- Logs contêm identificadores suficientes
- Rollback nomeia artifact, configuração, dependências de dados anteriores

### Bloqueadores Comuns

- Divisão aleatória de train/test em dados temporais ou relacionados a usuário
- Geração de feature usando campos não disponíveis no tempo de prediction
- Melhorias de métricas offline mas regressão de slices críticas
- Preprocessamento de training manualmente copiado para código de serving
- Versão de modelo não em logs de prediction
- Labels atrasadas sistematicamente ou leakage irreversível

### Critérios de Aprovação

- **APPROVE**: Sem riscos MLE críticos/altos, testes ou portões de avaliação relacionados passam
- **APPROVE WITH WARNINGS**: Apenas problemas médios, com ação clara de follow-up
- **BLOCK**: Qualquer vazamento possível, promotion irreprodutível, comportamento de serving inseguro, falta de rollback de deploy de produção, exposição de dados sensíveis, ou gaps críticos de avaliação

---

## pr-test-analyzer

### Nome e Propósito
Revisar qualidade e completude de cobertura de testes de Pull Request, enfatizando cobertura de comportamento e prevenção de bugs reais.

### Capacidades
- Identificar mapeamento de código de mudança
- Verificar cobertura de comportamento
- Avaliar qualidade de teste
- Descobrir gaps de cobertura

### Cenários de Uso
- Durante revisão de PR
- Ao avaliar cobertura de testes
- Ao verificar suficiência de testes

### Ferramentas Utilizadas
- Read: Ler código de mudança e testes
- Grep: Pesquisar testes
- Glob: Encontrar arquivos de teste
- Bash: Executar comandos de teste

### Colaboração com Outros Agentes
- tdd-guide melhorar testes
- e2e-runner executar cobertura end-to-end
- code-reviewer fazer revisão de qualidade de código

### Fluxo de Análise

#### 1. Identificar Código de Mudança
- Mapear funções, classes e módulos alterados
- Localizar testes correspondentes
- Identificar novos caminhos de código não testados

#### 2. Cobertura de Comportamento
- Verificar se cada funcionalidade tem teste
- Verificar casos de borda e caminhos de erro
- Garantir que integrações importantes estão cobertas

#### 3. Qualidade de Teste
- Preferir asserções significativas ao invés de verificações sem throws
- Marcar padrões instáveis
- Verificar isolamento e clareza de nomes de teste

#### 4. Gaps de Cobertura
Classificar por impacto:
- critical (crítico)
- important (importante)
- nice-to-have (bom ter)

### Formato de Saída

1. Resumo de cobertura
2. Gaps críticos
3. Sugestões de melhoria
4. Observações positivas
[Voltar ao Índice de Agentes](../README.md)