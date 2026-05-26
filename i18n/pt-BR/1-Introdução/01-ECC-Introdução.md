# 01 - [ECC](../DocumentosDeReferência/README.md) Introdução

## O que é [ECC](../DocumentosDeReferência/README.md)?

[ECC](../DocumentosDeReferência/README.md) (Everything Claude Code) é um sistema de plugin Claude Code que fornece [Agents](../DocumentosDeReferência/agents/1.%20代码审查类.md) de nível de produção, [Skills](../DocumentosDeReferência/skills/Melhores-Práticas.md), Hooks, Rules, [Commands](../DocumentosDeReferência/commands/01-Workflow-Principal.md) e configurações MCP. Originário de um projeto derrotado no Anthropic Hackathon, refinado por mais de 10 meses de uso diário.

## O que [ECC](../DocumentosDeReferência/README.md) pode fazer?

### 1. Sistema de [Agent](../DocumentosDeReferência/agents/1.%20代码审查类.md)
[ECC](../DocumentosDeReferência/README.md) oferece 60+ [Agents](../DocumentosDeReferência/agents/1.%20代码审查类.md) profissionais cobrindo revisão multilíngue, build fixes, design de arquitetura e mais:

| [Agent](../DocumentosDeReferência/agents/1.%20代码审查类.md) | Uso |
|-------|------|
| `planner` | Planejamento de implementação de funcionalidades |
| `code-reviewer` | Revisão de qualidade e segurança de código |
| `security-reviewer` | Análise de vulnerabilidades de segurança |
| `build-error-resolver` | Reparo de erros de build |
| `architect` | Design de arquitetura de sistema |
| `tdd-guide` | Guia de desenvolvimento orientado a testes |

### 2. Workflows de [Skills](../DocumentosDeReferência/skills/Melhores-Práticas.md)
[Skills](../DocumentosDeReferência/skills/Melhores-Práticas.md) são a superfície principal de workflow, [ECC](../DocumentosDeReferência/README.md) oferece 232+ [Skills](../DocumentosDeReferência/skills/Melhores-Práticas.md):

- `tdd-workflow` - Desenvolvimento orientado a testes
- `e2e-testing` - Testes end-to-end
- `security-review` - Lista de verificação de revisão de segurança
- `continuous-learning-v2` - Aprendizado contínuo baseado em instinct
- `eval-harness` - Avaliação de loop de verificação

### 3. [Commands](../DocumentosDeReferência/commands/01-Workflow-Principal.md)
Mantém compatibilidade com comandos tradicionais com barra (75+ comandos):

| Comando | Função |
|------|------|
| `/plan` | Criar plano de implementação |
| `/code-review` | Revisão de código |
| `/build-fix` | Reparo de erros de build |
| `/learn` | Extrair padrões da sessão |
| `/skill-create` | Gerar Skill do histórico Git |
| `/sessions` | Gerenciamento de histórico de sessões |

### 4. Automação de Hooks
Sistema flexível de hooks (8 tipos de eventos):
- `SessionStart` - Carregar contexto no início da sessão
- `SessionEnd` - Salvar estado no fim da sessão
- `PreToolUse` - Validação antes da execução da ferramenta
- `PostToolUse` - Formatação após execução da ferramenta

### 5. Sistema de Rules
Diretrizes abrangentes de desenvolvimento (common + específicas por linguagem):
- **common/** - Princípios gerais (segurança, estilo de código, testes, performance)
- **typescript/** - TypeScript/JavaScript
- **python/** - Python
- **golang/** - Go
- **swift/** - Swift
- **php/** - PHP
- **arkts/** - HarmonyOS/ArkTS

### 6. Serviços MCP
Suporte para 14+ configurações de servidor MCP: GitHub, Supabase, Vercel, Railway, Context7, Exa, Playwright e mais.

## Suporte multiplataforma

[ECC](../DocumentosDeReferência/README.md) suporta várias ferramentas de programação AI:

| Ferramenta | Agents | Commands | Skills | Hooks |
|------|--------|----------|--------|-------|
| Claude Code | 60 | 75 | 232 | 8 types |
| Cursor | 48 | compartilhado | compartilhado | 15 types |
| Codex | compartilhado | instrucional | 32 | - |
| OpenCode | 12 | 35 | 37 | 11 types |
| GitHub Copilot | - | 6 prompts | instrucional | - |

## Conceitos Principais

### [Agent](../DocumentosDeReferência/agents/1.%20代码审查类.md)
Subexecutores de tarefas reutilizáveis, cada [Agent](../DocumentosDeReferência/agents/1.%20代码审查类.md) tem responsabilidades claras. Construir workflows complexos combinando múltiplos [Agents](../DocumentosDeReferência/agents/1.%20代码审查类.md).

### [Skill](../DocumentosDeReferência/skills/Melhores-Práticas.md)
Define o fluxo de trabalho para completar tarefas específicas, é a superfície principal de workflow do [ECC](../DocumentosDeReferência/README.md).

### Hook
Scripts que são automaticamente disparados em momentos específicos, suportam automação de workflows.

### Rule
Restrições que o Claude Code deve seguir para garantir a qualidade da saída.


- [Instalação](./02-Instalação.md) - Comece a usar [ECC](../DocumentosDeReferência/README.md)
- [Retornar ao diretório de iniciantes](../README.md)