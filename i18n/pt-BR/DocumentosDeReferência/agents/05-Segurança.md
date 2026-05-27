# Agentes de Segurança

Agentes de segurança são especializados em detecção de vulnerabilidades, revisão de conformidade e proteção de informações sensíveis.

## Lista de Agentes

| Nome do Agente | Propósito | Modelo | Ferramentas Principais |
|----------------|-----------|--------|----------------------|
| security-reviewer | Especialista em detecção e correção de vulnerabilidades | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| silent-failure-hunter | Especialista em detecção de falhas silenciosas | sonnet | Read, Grep, Glob, Bash |
| healthcare-reviewer | Revisão de código de aplicações médicas (PHI/segurança clínica) | opus | Read, Grep, Glob |
| opensource-sanitizer | Especialista em verificação de vazamento de projetos open source | sonnet | Read, Grep, Glob, Bash |

---

## security-reviewer

### Nome e Propósito
Especialista em detecção e correção de vulnerabilidades. Usado proativamente após escrever código que lida com input de usuário, auth, endpoints de API ou dados sensíveis. Marca secrets, SSRF, injection, criptografia insegura e vulnerabilidades OWASP Top 10.

### Capacidades
- Detecção de vulnerabilidades - Identificar OWASP Top 10 e problemas comuns de segurança
- Detecção de Secrets - Descobrir API keys, senhas, tokens hardcoded
- Validação de input - Garantir que todo input de usuário é corretamente sanitizado
- Auth/autorização - Verificar controles de acesso apropriados estão em lugar
- Segurança de dependências - Verificar pacotes npm vulneráveis
- Melhores práticas de segurança - Impor padrões de codificação segura

### Cenários de Uso
- Novos endpoints de API
- Mudanças de código de autenticação
- Processamento de input de usuário
- Mudanças em queries de banco
- Upload de arquivos
- Código de pagamentos
- Integração com APIs externas
- Atualizações de dependências

### Ferramentas Utilizadas
- Read: Ler código
- Write: Escrever correções de segurança
- Edit: Fazer edições de correção de segurança
- Bash: Executar npm audit, eslint
- Grep: Pesquisar padrões sensíveis
- Glob: Encontrar arquivos

### Colaboração com Outros Agentes
- code-reviewer compartilhar verificações de qualidade de código
- healthcare-reviewer lidar com problemas específicos de saúde
- opensource-sanitizer lidar com verificações pré-lançamento de open source

### Scan Inicial

```bash
npm audit --audit-level=high
npx eslint . --plugin security
```

Pesquisar secrets hardcoded, revisar áreas de alto risco: auth, endpoints de API, queries de DB, upload de arquivos, pagamentos, webhooks.

### Verificações OWASP Top 10

1. **Injection** - Queries estão parametrizadas? Input de usuário está sanitizado? ORM está sendo usado de forma segura?
2. **Broken Auth** - Senhas estão hashed (bcrypt/argon2)? JWT está verificado? Sessões estão seguras?
3. **Sensitive Data** - HTTPS está forçado? Secrets estão em env vars? PII está criptografado? Logs estão limpos?
4. **XXE** - Parsers XML estão configurados seguramente? Entidades externas estão desabilitadas?
5. **Broken Access Control** - Cada rota verifica auth? CORS está corretamente configurado?
6. **Security Misconfiguration** - Credenciais default foram alteradas? Modo debug de produção está desativado? Headers de segurança estão configurados?
7. **XSS** - Output está escapado? CSP está configurado? Framework faz escape automático?
8. **Insecure Deserialization** - Input de usuário está sendo desserializado de forma segura?
9. **Using Components with Known Vulnerabilities** - Dependências estão atualizadas? npm audit passa?
10. **Insufficient Logging & Monitoring** - Eventos de segurança estão logados? Alertas estão configurados?

### Revisão de Padrões de Código

Marcar estes padrões imediatamente:

| Padrão | Severidade | Correção |
|--------|-----------|----------|
| Secrets hardcoded | CRITICAL | Usar `process.env` |
| Comando shell de input de usuário | CRITICAL | Usar API segura ou execFile |
| SQL com concatenação de string | CRITICAL | Queries parametrizadas |
| `innerHTML = userInput` | HIGH | Usar `textContent` ou DOMPurify |
| `fetch(userProvidedUrl)` | HIGH | Whitelist de domínios permitidos |
| Comparação de senha em texto claro | CRITICAL | Usar `bcrypt.compare()` |
| Rota sem verificação de auth | CRITICAL | Adicionar middleware de auth |
| Verificação de saldo sem lock | CRITICAL | Usar `FOR UPDATE` em transação |
| Sem rate limiting | HIGH | Adicionar `express-rate-limit` |
| Log de senhas/secrets | MEDIUM | Limpar output de log |

### Princípios Centrais

1. **Defesa em Profundidade** - Múltiplas camadas de segurança
2. **Menor Privilégio** - Privilégio mínimo necessário
3. **Falha Segura** - Erros não devem expor dados
4. **Não Confiar em Input** - Validar e sanitizar tudo
5. **Atualizações Regulares** - Manter dependências atualizadas

### Falsos Positivos Comuns

- Variáveis de ambiente em `.env.example` (não são secrets reais)
- Credenciais de teste em arquivos de teste (se explicitamente marcados)
- Chaves de API públicas que são realmente públicas
- SHA256/MD5 usados para checksums (não senhas)

**Sempre verificar contexto antes de marcar.**

### Resposta de Emergência

Se vulnerabilidade CRITICAL for encontrada:
1. Documentar relatório detalhado
2. Notificar lead de projeto imediatamente
3. Fornecer exemplo de código seguro
4. Verificar que correção funciona
5. Se credenciais expostas, rotacionar secrets

---

## silent-failure-hunter

### Nome e Propósito
Revisar código para falhas silenciosas, erros engolidos, fallbacks incorretos e propagação de erro faltando.

### Capacidades
- Detecção de blocos catch vazios
- Detecção de logging insuficiente
- Detecção de fallbacks perigosos
- Detecção de problemas de propagação de erro
- Detecção de tratamento de erro faltando

### Cenários de Uso
- Durante revisão de código
- Quando bugs estranhos são descobertos
- Quando pipeline parece verde mas dados são pulados
- Durante revisão de tratamento de exceções

### Ferramentas Utilizadas
- Read: Ler código
- Grep: Pesquisar padrões de tratamento de erro
- Glob: Encontrar arquivos fonte
- Bash: Executar testes

### Colaboração com Outros Agentes
- code-reviewer compartilhar verificações de qualidade de código
- security-reviewer lidar com problemas relacionados a segurança
- mle-reviewer lidar com problemas de pipeline de ML

### Alvos de Caça

#### 1. Blocos Catch Vazios
- `catch {}` ou exceções ignoradas
- Conversão para `null` / array vazio sem contexto de erro

#### 2. Logging Insuficiente
- Contexto de log insuficiente
- Severidade de erro incorreta
- Tratamento log-and-forget

#### 3. Fallbacks Perigosos
- Padrões que escondem falhas reais
- `.catch(() => [])`
- Caminhos que são graciosos mas tornam bugs downstream mais difíceis de diagnosticar

#### 4. Problemas de Propagação de Erro
- Stack traces perdidos
- Rethrows genéricos
- Tratamento async faltando

#### 5. Tratamento de Erro Faltando
- Caminhos de rede/arquivo/banco sem timeout ou tratamento de erro
- Trabalho transacional sem rollback

### Formato de Saída

Cada descoberta:
- Localização
- Severidade
- Problema
- Impacto
- Sugestão de correção

---

## healthcare-reviewer

### Nome e Propósito
Revisar código de aplicações médicas para segurança clínica, precisão de CDSS, conformidade PHI e integridade de dados médicos. Projetado para EMR/EHR, sistemas de suporte a decisão clínica e sistemas de informação de saúde.

### Capacidades
- Precisão de CDSS - Verificar lógica de interação de medicamentos, regras de validação de dosage e implementação de scores clínicos
- Proteção PHI/PII - Escanear exposição de dados de pacientes em logs, erros, respostas, URLs e armazenamento do cliente
- Integridade de dados clínicos - Garantir audit trails, registros bloqueados e proteção cascata
- Correção de dados médicos - Verificar mapeamentos ICD-10/SNOMED, intervalos de referência de laboratório e entradas de banco de dados de medicamentos
- Conformidade de integração - Verificar processamento de mensagens HL7/FHIR e recuperação de erros

### Cenários de Uso
- Desenvolvimento de sistemas EMR/EHR
- Sistemas de suporte a decisão clínica
- Sistemas de informação de saúde
- Aplicações de processamento de dados médicos

### Ferramentas Utilizadas
- Read: Ler código médico
- Grep: Pesquisar padrões PHI
- Glob: Encontrar arquivos relacionados a saúde

### Colaboração com Outros Agentes
- security-reviewer lidar com vulnerabilidades de segurança genéricas
- code-reviewer lidar com problemas de qualidade de código
- silent-failure-hunter lidar com problemas de falhas silenciosas

### Verificações Chave

#### CDSS Engine
- [ ] Todos os pares de interação de medicamentos geram alertas corretos (bidirecionais)
- [ ] Regras de validação de dosage disparam em valores fora de faixa
- [ ] Scores clínicos correspondem à especificação publicada
- [ ] Sem falsos negativos (interações perdidas = evento de segurança do paciente)
- [ ] Input mal formatado gera erros, não passa silenciosamente

#### Proteção PHI
- [ ] Dados de pacientes não estão em `console.log`, `console.error` ou mensagens de erro
- [ ] PHI não está em parâmetros de URL ou query strings
- [ ] PHI não está em localStorage/sessionStorage do navegador
- [ ] Sem chave `service_role` em código cliente
- [ ] RLS habilitado em todas as tabelas de dados de pacientes
- [ ] Isolamento de dados entre instalações verificado

#### Workflows Clínicos
- [ ] Bloqueio de registro clínico previne edição (apenas append)
- [ ] Todo CRUD de dados clínicos tem entrada de audit trail
- [ ] Alertas críticos não podem ser ignorados (não são notificações toast)
- [ ] Quando clínicos continuam com alertas críticos, razão de sobrescrita é registrada
- [ ] Sintomas de bandeira vermelha disparam alertas visíveis

#### Integridade de Dados
- [ ] Registros de pacientes não têm CASCADE DELETE
- [ ] Detecção de edição concorrente (locking otimista ou resolução de conflitos)
- [ ] Sem registros órfãos em tabelas clínicas
- [ ] Timestamps usam timezone consistente

### Formato de Saída

```
## Healthcare Review: [módulo/funcionalidade]

### Impacto na Segurança do Paciente: [CRITICAL / HIGH / MEDIUM / LOW / NONE]

### Precisão Clínica
- CDSS: [verificado/falhou]
- Drug DB: [validado/problema]
- Scoring: [corresponde à especificação/divergência]

### Conformidade PHI
- Verificações de vetor de exposição: [lista]
- Problemas encontrados: [lista ou nenhum]

### Problemas
1. [PATIENT SAFETY / CLINICAL / PHI / TECHNICAL] Descrição
   - Impact: [dano potencial ou exposição]
   - Fix: [mudanças necessárias]

### Veredito: [SAFE TO DEPLOY / NEEDS FIXES / BLOCK — PATIENT SAFETY RISK]
```

### Regras

- Em caso de dúvida sobre precisão clínica, marcar como NEEDS REVIEW - nunca aprovar lógica clínica incerta
- Uma interação de medicamento perdida é pior que cem falsos alertas
- Exposição de PHI é sempre severidade CRITICAL, não importa o tamanho do vazamento
- Nunca aprovar código que captura silenciosamente erros de CDSS

---

## opensource-sanitizer

### Nome e Propósito
Verificar que forks open source estão completamente limpos antes de lançamento. Escanear secrets vazados, PII, referências internas e arquivos perigosos. Usa 20+ padrões regex. Gera relatório PASS/FAIL/PASS-WITH-WARNINGS.

### Capacidades
- Scan de secrets - Escanear API keys, senhas, tokens
- Scan de PII - Escanear emails, endereços IP
- Scan de referências internas - Escanear caminhos absolutos, nomes de domínio internos
- Verificação de arquivos perigosos - Verificar .env, credentials.json etc.
- Verificação de completude de configuração - Verificar .env.example
- Auditoria de histórico Git - Verificar credenciais vazadas

### Cenários de Uso
- Antes de lançamento de fork open source
- Antes de auditoria de código de terceiros
- Verificações de segurança antes de release de código

### Ferramentas Utilizadas
- Read: Ler arquivos
- Grep: Pesquisar padrões sensíveis
- Glob: Encontrar arquivos
- Bash: Executar git log etc.

### Colaboração com Outros Agentes
- security-reviewer lidar com vulnerabilidades de segurança genéricas
- code-reviewer lidar com problemas de qualidade de código
- doc-updater atualizar documentação

### Fluxo de Trabalho

#### Passo 1: Scan de Secrets (CRITICAL)

Escanear cada arquivo de texto (excluir `node_modules`, `.git`, `__pycache__`, `*.min.js`, binários):

```
# API keys
pattern: [A-Za-z0-9_]*(api[_-]?key|apikey|api[_-]?secret)[A-Za-z0-9_]*\s*[=:]\s*['"]?[A-Za-z0-9+/=_-]{16,}

# AWS
pattern: AKIA[0-9A-Z]{16}
pattern: (?i)(aws_secret_access_key|aws_secret)\s*[=:]\s*['"]?[A-Za-z0-9+/=]{20,}

# Database URL (com credenciais)
pattern: (postgres|mysql|mongodb|redis)://[^:]+:[^@]+@[^\s'"]+

# JWT tokens (3-segment)
pattern: eyJ[A-Za-z0-9_-]{20,}\.eyJ[A-Za-z0-9_-]{20,}\.[A-Za-z0-9_-]+

# Private keys
pattern: -----BEGIN\s+(RSA\s+|EC\s+|DSA\s+|OPENSSH\s+)?PRIVATE KEY-----

# GitHub tokens
pattern: gh[pousr]_[A-Za-z0-9_]{36,}
pattern: github_pat_[A-Za-z0-9_]{22,}
```

#### Passo 2: Scan de PII (CRITICAL)

```
# Emails pessoais (não emails genéricos como noreply@, info@)
pattern: [a-zA-Z0-9._%+-]+@(gmail|yahoo|hotmail|outlook|protonmail|icloud)\.(com|net|org)

# Endereços IP privados (indicam infraestrutura interna)
pattern: (192\.168\.\d+\.\d+|10\.\d+\.\d+\.\d+|172.(1[6-9]|2\d|3[01])\.\d+\.\d+)

# Strings de conexão SSH
pattern: ssh\s+[a-z]+@[0-9.]+
```

#### Passo 3: Scan de Referências Internas (CRITICAL)

```
# Caminhos absolutos para diretórios home de usuários específicos
pattern: /home/[a-z][a-z0-9_-]*/
pattern: /Users/[A-Za-z][A-Za-z0-9_-]*/
pattern: C:\\Users\\[A-Za-z]

# Referências a arquivos de secrets internos
pattern: \.secrets/
pattern: source\s+~/\.secrets/
```

#### Passo 4: Verificação de Arquivos Perigosos (CRITICAL)

Verificar que estes **não existem**:
```
.env (qualquer variante: .env.local, .env.production, .env.*.local)
*.pem, *.key, *.p12, *.pfx, *.jks
credentials.json, service-account*.json
.secrets/, secrets/
.claude/settings.json
sessions/
*.map (source maps expõem estrutura de código fonte original)
node_modules/, __pycache__/, .venv/, venv/
```

#### Passo 5: Auditoria de Histórico Git

```bash
# Deve ser um único commit inicial
cd PROJECT_DIR
git log --oneline | wc -l
# Se > 1, histórico não foi limpo — FAIL

# Procurar secrets possíveis
git log -p | grep -iE '(password|secret|api.?key|token)' | head -20
```

### Formato de Saída

Gerar `SANITIZATION_REPORT.md` no diretório do projeto:

```markdown
# Sanitization Report: {project-name}

**Date:** {date}
**Auditor:** opensource-sanitizer v1.0.0
**Verdict:** PASS | FAIL | PASS WITH WARNINGS

## Summary

| Category | Status | Findings |
|----------|--------|----------|
| Secrets | PASS/FAIL | {count} findings |
| PII | PASS/FAIL | {count} findings |
| Internal References | PASS/FAIL | {count} findings |
| Dangerous Files | PASS/FAIL | {count} findings |
| Config Completeness | PASS/WARN | {count} findings |
| Git History | PASS/FAIL | {count} findings |

## Critical Findings (Must Fix Before Release)

1. **[SECRETS]** `src/config.py:42` — Hardcoded database password: `DB_P...` (truncated)

## Warnings (Review Before Release)

1. **[CONFIG]** `src/app.py:8` — Port 8080 hardcoded, should be configurable

## .env.example Audit

- Variables in code but NOT in .env.example: {list}
- Variables in .env.example but NOT in code: {list}

## Recommendation

{If FAIL: "Fix the {N} critical findings and re-run sanitizer."}
{If PASS: "Project is clear for open-source release. Proceed to packager."}
{If WARNINGS: "Project passes critical checks. Review {N} warnings before release."}
```

### Regras

- **Nunca** mostrar valor completo de secret - truncar para primeiros 4 caracteres + "..."
- **Nunca** modificar arquivos fonte - apenas gerar relatório (SANITIZATION_REPORT.md)
- **Sempre** escanear cada arquivo de texto, não apenas extensões conhecidas
- **Sempre** verificar histórico git, mesmo para novos repositórios
- **Manter paranoia** - falsos positivos são aceitáveis, falsos negativos não são
- Qualquer descoberta CRITICAL única em qualquer categoria = FAIL geral
- Avisos sozinhos = PASS WITH WARNINGS (decisão do usuário)
[Voltar ao Índice de Agentes](../README.md)