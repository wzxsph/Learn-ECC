# Comandos do Sistema Hookify

## Visão Geral

O sistema Hookify é usado para criar e gerenciar hooks, prevenir comportamentos indesejados e automatizar workflows.

## Lista de Comandos

### /hookify

**Propósito**: Criar hooks para prevenir comportamentos indesejados

**Descrição**: Cria hooks personalizados para prevenir comportamentos que não deseja que aconteçam.

**Cenários de Uso**:
- Prevenir operações de exclusão acidentais
- Bloquear comandos perigosos
- Auto-formatação
- Verificações de qualidade

**Tipos de Hook**:
- **PreToolUse**: Antes da execução da ferramenta
- **PostToolUse**: Após a execução da ferramenta
- **Stop**: Quando a sessão termina

---

### /hookify-list

**Propósito**: Listar todas as regras hookify configuradas

**Descrição**: Mostra todas as regras hookify atualmente configuradas.

---

### /hookify-configure

**Propósito**: Ativar/desativar regras hookify interativamente

**Descrição**: Configura interativamente o estado de ativação e desativação das regras hookify.

---

### /hookify-help

**Propósito**: Ajuda do sistema Hookify

**Descrição**: Obtém informação de ajuda detalhada sobre o sistema Hookify.

---

## Exemplo de Configuração de Hook

```json
{
  "matcher": { "pattern": "Bash|Edit|Write" },
  "hooks": [
    {
      "type": "command",
      "command": "echo 'Running quality check'"
    }
  ]
}
```

---

## Comandos Relacionados

- `/hookify` - Criar hook
- `/hookify-list` - Listar regras
- `/hookify-configure` - Configurar regras
- `/hookify-help` - Obter ajuda