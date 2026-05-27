# Regras de Segurança

## Visão Geral das Regras

As regras ECC Regras-de-Segurança são um sistema de verificação de segurança obrigatório, com o objetivo de garantir que código cumpra os mais altos padrões de segurança antes de commit. Estas regras cobrem todos os aspectos desde gerenciamento de Secrets até segurança de servidor MCP, e são a linha de base que todas as mudanças de código devem seguir.

## Requisitos Centrais

### Checklist Obrigatório Pré-commit

Devem completar as seguintes verificações de segurança antes de fazer commit de qualquer código:

| Item de Verificação | Descrição |
|---------------------|------------|
| Sem Secrets Hardcoded | API keys, senhas, tokens não devem estar hardcoded no código fonte |
| Validação de Input de Usuário | Todo input de usuário deve ser validado |
| Proteção contra SQL Injection | Usar queries parametrizadas para prevenir SQL injection |
| Proteção contra XSS | Fazer escape de input de usuário para HTML |
| Proteção CSRF | Garantir que proteção CSRF está habilitada |
| Verificação de Auth/Autorização | Confirmar que lógica de autenticação e autorização está correta |
| Mecanismo de Rate Limiting | Todos os endpoints devem ter rate limiting |
| Segurança de Mensagens de Erro | Mensagens de erro não devem vazar dados sensíveis |