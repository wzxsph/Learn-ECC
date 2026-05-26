# Learn-ECC

> Um caminho de aprendizado estruturado para dominar ECC (Everything Claude Code)

Bem-vindo ao Learn-ECC! Este é um caminho de aprendizado sistemático para Claude Code, que ajuda você a passar de usuário iniciante a usuário proficient.

---

## 🌐 Language / 语言 / 語言 / Dil / Язык / Ngôn ngữ

English | Português (Brasil) | 简体中文 | 繁體中文 | 日本語 | 한국어 | Türkçe | Русский | Tiếng Việt | ไทย

| [🇺🇸 English](./en/README.md) | [🇯🇵 日本語](./ja/README.md) | [🇰🇷 한국어](./ko/README.md) | [🇩🇪 Deutsch](./de/README.md) |
|:---:|:---:|:---:|:---:|
| [🇷🇺 Русский](./ru/README.md) | [🇹🇷 Türkçe](./tr/README.md) | [🇧🇷 Português](./README.md) | [🇻🇳 Tiếng Việt](./vi/README.md) |
| [🇹🇭 ไทย](./th/README.md) | [🇨🇳 简体中文](../README.md) | | | |

---

## Contexto

### Por que este projeto existe?

Eu tinha dificuldades com a documentação em inglês do ECC. Por isso, traduzi as explicações sobre comandos ECC (Commands), Agents, Skills, etc. para o chinês e organizei este curso de aprendizado.

A documentação de referência é exatamente isso - uma tradução do projeto ECC original, explicando o que cada comando e cada Agent faz.

### De onde vem o curso?

Frankly, este curso foi gerado por IA. A IA criou o conteúdo baseado na estrutura de arquivos do ECC, mas faltam análises detalhadas e avaliações práticas. Se você encontrar problemas ou o conteúdo não for claro o suficiente, welcome any sugestões de melhoria.

---

## Conceitos Principais

### O que é ECC?

**ECC** (Everything Claude Code) é um projeto de plugin Claude Code ([Repositório Original](https://github.com/affaan-m/ECC)), que fornece:
- **75 comandos** - comandos com barra (como `/plan`, `/code-review`) e comandos de script
- **60 Agents profissionais** - code review, build fixes, design de arquitetura, etc.
- **232+ Skills** - modelos de workflow reutilizáveis
- **Sistema de Hooks** - mecanismo de automação orientado a eventos
- **Rules** - diretrizes obrigatórias de desenvolvimento

### O que é Learn-ECC?

**Learn-ECC** é um caminho de aprendizado sistemático que criei para aprender ECC - não é o ECC em si.

Ele usa o conceito de "Learning by Doing", onde cada fase inclui aprendizado teórico e exercícios práticos. Através da conclusão de projetos reais, você dominará as habilidades essenciais do Claude Code.

### O que é a Documentação de Referência?

**Documentação de Referência** (./DocumentosDeReferência/) é uma tradução para chinês do projeto ECC. Traduzi a documentação em inglês do ECC para o chinês para facilitar a consulta.

O conteúdo inclui:
- Descrições completas de comandos (o que cada comando faz)
- Introduções detalhadas de todos os Agents
- Estrutura de diretório de Skills e padrões de escrita
- Documentação completa do sistema de Hooks
- Melhores práticas e padrões de design

---

## Visão Geral do Caminho de Aprendizado

| Fase | Conteúdo | Duração | Dificuldade |
|------|----------|---------|-------------|
| [1-Introdução](./1-Introdução/README.md) | Introdução a comandos ECC, Agents, Skills + Instalação | 2-3 horas | Iniciante |
| [2-HabilidadesCentrais](./2-HabilidadesCentrais/README.md) | Sistema de comandos, Colaboração de Agents, Customização de Hooks, Desenvolvimento de Skills, Criação de Rules | 8-12 horas | Intermediário |
| [3-CaminhoDoEspecialista](./3-CaminhoDoEspecialista/README.md) | Desenvolvimento MCP, Agents Autônomos, Padrões Avançados | 6-8 horas | Avançado |
| [4-ProjetosPráticos](./4-ProjetosPráticos/README.md) | Ferramentas CLI, Code Review, Automação, Sistemas de Agent | 10-15 horas | Prático |
| [5-ReferênciaRápida](./5-ReferênciaRápida/README.md) | Cheatsheet de comandos, Cheatsheet de agents, Cheatsheet de workflows | Consultar a qualquer momento | Referência |

---

## Navegação Rápida

### Fase 1: Iniciante
- [Introdução ao Curso](./1-Introdução/01-ECC-Introdução.md) - O que é ECC, o que Learn-ECC pode fazer
- [Instalação](./1-Introdução/02-Instalação.md) - Configuração rápida
- [Comandos Básicos](./1-Introdução/03-ComandosBásicos.md) - Visão geral dos comandos mais usados
- [Primeira Tarefa](./1-Introdução/04-PrimeiraTarefa.md) - Complete sua primeira tarefa
- [Exercícios para Iniciantes](./1-Introdução/exercises/入门练习.md) - Consolide os fundamentos

### Fase 2: Habilidades Essenciais
- [Domínio do Sistema de Comandos](./2-HabilidadesCentrais/01-SistemaDeComandos/README.md) - Domine todos os tipos de comandos
- [Colaboração de Agents](./2-HabilidadesCentrais/02-ColaboraçãoDeAgentes/README.md) - Design de sistema multi-agent
- [Customização de Hooks](./2-HabilidadesCentrais/03-PersonalizaçãoDeHooks/README.md) - Automação de workflows
- [Desenvolvimento de Skills](./2-HabilidadesCentrais/04-DesenvolvimentoDeSkills/README.md) - Construa habilidades reutilizáveis
- [Criação de Rules](./2-HabilidadesCentrais/05-CriaçãoDeRules/README.md) - Customize diretrizes de desenvolvimento

### Fase 3: Caminho do Especialista
- [Desenvolvimento MCP](./3-CaminhoDoEspecialista/01-DesenvolvimentoMCP.md) - Protocolo de Contexto de Modelo
- [Agents Autônomos](./3-CaminhoDoEspecialista/02-AgentesAutônomos.md) - Construa Agents autônomos
- [Padrões Avançados](./3-CaminhoDoEspecialista/03-PadrõesAvançados.md) - Padrões de design e melhores práticas

### Fase 4: Projetos Práticos
- [Projeto 1: Ferramenta CLI](./4-ProjetosPráticos/project-1-cli.md) - Construa ferramentas de linha de comando
- [Projeto 2: Code Review](./4-ProjetosPráticos/project-2-review.md) - Workflow automatizado de review
- [Projeto 3: Automação de Workflow](./4-ProjetosPráticos/project-3-auto.md) - Automação end-to-end
- [Projeto 4: Sistema de Agent](./4-ProjetosPráticos/project-4-agent.md) - Plataforma de colaboração multi-agent

### Fase 5: Referência Rápida
- [Cheatsheet de Comandos](./5-ReferênciaRápida/cheatsheet-commands.md)
- [Cheatsheet de Agents](./5-ReferênciaRápida/cheatsheet-agents.md)
- [Cheatsheet de Workflows](./5-ReferênciaRápida/cheatsheet-workflows.md)

---

## Como Usar Este Curso

### Ordem de Aprendizado Recomendada

1. **Aprenda sequencialmente por fase** - Cada fase constrói sobre a anterior
2. **Combine teoria e prática** - Cada capítulo tem exercícios correspondentes, entenda primeiro depois prática
3. **Complete todos os exercícios** - Exercícios são o núcleo do caminho de aprendizado

### Recomendações de Aprendizado

- **Fase de Iniciante**: 2-3 horas recomendadas, foque em conceitos básicos e operações
- **Habilidades Essenciais**: 8-12 horas recomendadas, pratique cada módulo pessoalmente
- **Caminho do Especialista**: 6-8 horas recomendadas, requer experiência em projetos
- **Projetos Práticos**: 10-15 horas recomendadas, escolha projetos interessantes para se aprofundar

### Pontos de Verificação de Aprendizado

Após completar cada fase, confirme que você domina:
- Consegue realizar todas as operações dessa fase independentemente
- Consegue explicar conceitos básicos para outros
- Consegue aplicar conhecimento a outros cenários

---

## Pré-requisitos

### Conhecimento Básico
- Noções básicas de linha de comando (conhecer comandos básicos)
- Noções básicas de JavaScript/Node.js (algumas funções avançadas requerem)
- Noções básicas de Git (conceitos de controle de versão)

### Requisitos de Ambiente
- Node.js >= 18
- Um dos: npm/pnpm/yarn/bun
- Editor de texto (VS Code recomendado)

### Cursos Pré-requisitos
Nenhum. A fase de iniciante é projetada paraゼロ基础.

---

## Mapa do Caminho de Aprendizado

```
┌─────────────────────────────────────────────────────────────────┐
│                      Learn-ECC Caminho de Aprendizado            │
└─────────────────────────────────────────────────────────────────┘

    Fase 1: Iniciante
        │
        ↓
    ┌───────────────────────┐
    │   Fase 2: Habilidades Essenciais│←─────────────────┐
    │  (Comandos/Agent/Hooks/    │                  │
    │   Skills/Rules)          │                  │
    └───────────┬───────────┘                     │
                │                                │
                ↓                                │
    ┌───────────────────────┐                     │
    │   Fase 3: Caminho do Especialista │         │
    │  (MCP/Agents Autônomos/ │                     │
    │   Padrões Avançados)    │                     │
    └───────────┬───────────┘                     │
                │                                │
                ↓                                │
    ┌───────────────────────┐                     │
    │   Fase 4: Projetos Práticos │──────────────┘
    │  (CLI/Review/Automação/│
    │   Agent)              │
    └───────────┬───────────┘
                │
                ↓
    ┌───────────────────────┐
    │   Fase 5: Referência Rápida│ ← Consultar a qualquer momento
    │  (Cheatsheets de Comandos/Agent)
    └───────────────────────┘
```

**Tempo total estimado de aprendizado**: 26-38 horas (cerca de 2-3 semanas)

---

## Estrutura de Diretórios

```
Learn-ECC/
├── 1-Introdução/                         # Fase 1: Iniciante (2-3 horas)
│   ├── 01-ECC-Introdução.md               # Visão geral do ECC, valor central, arquitetura
│   ├── 02-Instalação.md              # Preparação de ambiente, instalação, verificação
│   ├── 03-ComandosBásicos.md              # /plan, /code-review, /build-fix
│   ├── 04-PrimeiraTarefa.md           # Exemplo completo de desenvolvimento
│   └── exercises/
│       └── 入门练习.md             # 4 exercícios para iniciantes
│
├── 2-HabilidadesCentrais/                     # Fase 2: Habilidades Essenciais (8-12 horas)
│   ├── 01-命令系统精通/
│   ├── 02-Agent协作/
│   ├── 03-Hooks定制/
│   ├── 04-Skills开发/
│   └── 05-Rules编写/
│
├── 3-CaminhoDoEspecialista/                     # Fase 3: Caminho do Especialista (6-8 horas)
│   ├── 01-DesenvolvimentoMCP.md
│   ├── 02-AgentesAutônomos.md
│   └── 03-PadrõesAvançados.md
│
├── 4-ProjetosPráticos/                     # Fase 4: Projetos Práticos (10-15 horas)
│   ├── project-1-cli.md           # Desenvolvimento de ferramenta CLI
│   ├── project-2-review.md        # Pipeline automatizado de code review
│   ├── project-3-auto.md          # Automação de workflow
│   └── project-4-agent.md         # Desenvolvimento de Agent personalizado
│
├── 5-ReferênciaRápida/                     # Fase 5: Referência Rápida
│   ├── cheatsheet-commands.md     # Cheatsheet de comandos
│   ├── cheatsheet-agents.md       # Cheatsheet de agents
│   └── cheatsheet-workflows.md    # Cheatsheet de workflows
│
├── assets/                         # Recursos de imagem
├── progress-tracker.md             # Rastreador de progresso de aprendizado
├── contributing.md                 # Guia de contribuição
└── README.md                       # Este arquivo
```

---

## Dicas de Sucesso

1. **Progresso gradual**: Não pule fases, cada fase é a base para a próxima
2. **Pratique动手**: Pratique imediatamente cada conceito, não apenas leia
3. **Complete exercícios**: Cada fase tem exercícios correspondentes, only through completion consolidation knowledge
4. **Use cheatsheets**: No uso diário,充分利用 cheatsheets, não decore
5. **Projetos guiam**: Fase 4 usa projetos reais para linking all knowledge
6. **Tome notas**: Documente experiências e problemas de aprendizado
7. **Ensine outros**: Só quando você pode ensinar others you truly master

---

## Perguntas Frequentes (FAQ)

### P: É necessário ter base de programação?

R: É necessário ter conhecimento básico de linha de comando e programação (base JavaScript/Node.js ajuda em alguns funções), mas não é necessário dominar qualquer linguagem específica. A fase de iniciante é projetada para base zero.

### P: Posso pular algumas fases?

R: Fase 1-3 é recomendado completar sequencialmente, Fase 4 pode escolher projects interesting conforme necessário, Fase 5 pode ser consultada a qualquer momento.

### P: O que posso fazer após o aprendizado?

R: Dominar workflows Claude Code de nível de produção, desenvolver, revisar, testar e automatizar eficientemente; projetar e implementar sistemas de Agent personalizados; customizar Hooks e Skills.

### P: O que fazer se encontrar problemas durante o aprendizado?

R: 1) Consulte capítulos relevantes na [Documentação de Referência](./DocumentosDeReferência/); 2) Verifique o [Cheatsheet de Comandos](./5-ReferênciaRápida/cheatsheet-commands.md); 3) Crie um Issue para pedir ajuda.

### P: Como verificar o efeito do aprendizado?

R: Use [progress-tracker.md](./progress-tracker.md) para acompanhar progresso, cada fase tem claro learning objectives, completar todos os exercícios indica have mastered a fase.

### P: O conteúdo do curso será atualizado?

R: Sim, será continuamente atualizado, PRs são bem-vindos. Para detalhes, see [contributing.md](./contributing.md).

---

## Contribuições e Feedback

Se você encontrar problemas ou tiver sugestões de melhoria durante o aprendizado, sinta-se à vontade para criar um Issue ou Pull Request.

---

*Última atualização: 2026-05-26*