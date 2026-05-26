# 02 - Instalação e Configuração

## Requisitos de Ambiente

- Node.js >= 18
- Um de npm / pnpm / yarn / bun
- Claude Code CLI v2.1.0+ instalado

## Métodos de Instalação

### Método 1: Instalação por Plugin (Recomendado)

#### 1. Adicionar marketplace de plugins

```bash
/plugin marketplace add https://github.com/affaan-m/ECC
```

#### 2. Instalar plugin

```bash
/plugin install ecc@ecc
```

#### 3. Instalar Rules (plugins não instalam automaticamente)

```bash
# Clonar repositório
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Copiar regras para diretório do usuário
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
# Escolha sua stack técnica
cp -r rules/typescript ~/.claude/rules/ecc/   # ou python, golang, swift, php
```

### Método 2: Instalação Manual

Se você quer controle mais refinado sobre a instalação:

```bash
# Clonar repositório
git clone https://github.com/affaan-m/ECC.git
cd ECC

# Instalar dependências
npm install        # ou pnpm install | yarn install | bun install

# Instalar agents
cp agents/*.md ~/.claude/agents/

# Instalar skills (superfície principal de workflow)
mkdir -p ~/.claude/skills/ecc
cp -r .agents/skills/* ~/.claude/skills/ecc/

# Instalar rules
mkdir -p ~/.claude/rules/ecc
cp -r rules/common ~/.claude/rules/ecc/
cp -r rules/typescript ~/.claude/rules/ecc/

# Instalar commands (opcional, ECC está migrando para skills)
mkdir -p ~/.claude/commands
cp commands/*.md ~/.claude/commands/
```

## Verificar Instalação

```bash
# Verificar se plugin foi instalado com sucesso
/plugin list ecc@ecc

# Verificar comandos disponíveis
/ecc:plan
/ecc:code-review
```

## Configuração Básica

### Definir Gerenciador de Pacotes

ECC detecta automaticamente seu gerenciador de pacotes preferido, você também pode definir manualmente:

```bash
# Via variável de ambiente
export CLAUDE_PACKAGE_MANAGER=pnpm

# ou via comando
/setup-pm
```

Prioridade: variável de ambiente > configuração do projeto > package.json > arquivo lock > configuração global

### Controle de Runtime de Hooks

```bash
# Rigidez de Hook (padrão: standard)
export ECC_HOOK_PROFILE=standard

# Desativar hooks específicos
export ECC_DISABLED_HOOKS="pre:bash:tmux-reminder,post:edit:typecheck"

# Limite de contexto SessionStart (padrão: 8000 caracteres)
export ECC_SESSION_START_MAX_CHARS=4000
```

### Configurações de Modelo

Recomendado configurar em `~/.claude/settings.json`:

```json
{
  "model": "sonnet",
  "env": {
    "MAX_THINKING_TOKENS": "10000",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "50"
  }
}
```

## Próximos Passos

- [Comandos Básicos](./03-ComandosBásicos.md) - Aprender comandos frequentes
- [Retornar ao diretório de iniciantes](../README.md)