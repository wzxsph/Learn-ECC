# Security Agents

Security agents specialize in security vulnerability detection, compliance review, and sensitive information protection.

## Agent List

| Agent Name | Purpose | Model Used | Core Tools |
|------------|---------|------------|------------|
| security-reviewer | Security vulnerability detection and fixing expert | sonnet | Read, Write, Edit, Bash, Grep, Glob |
| silent-failure-hunter | Silent failure detection expert | sonnet | Read, Grep, Glob, Bash |
| healthcare-reviewer | Healthcare application code review (PHI/clinical safety) | opus | Read, Grep, Glob |
| opensource-sanitizer | Open source project leak checking expert | sonnet | Read, Grep, Glob, Bash |

---

## security-reviewer

### Name and Purpose
Security vulnerability detection and fixing expert. Proactively used after writing code that handles user input, authentication, API endpoints, or sensitive data. Flags secrets, SSRF, injection, insecure cryptography, and OWASP Top 10 vulnerabilities.

### Capabilities
- Vulnerability detection - Identify OWASP Top 10 and common security issues
- Secrets detection - Find hardcoded API keys, passwords, tokens
- Input validation - Ensure all user input is properly sanitized
- Auth/Authorization - Verify appropriate access controls
- Dependency security - Check for vulnerable npm packages
- Security best practices - Enforce secure coding patterns

### Applicable Scenarios
- New API endpoints
- Authentication code changes
- User input handling
- Database query changes
- File uploads
- Payment code
- External API integration
- Dependency updates

### Tools Used
- Read: Read code
- Write: Write security fixes
- Edit: Edit security fixes
- Bash: Run npm audit, eslint
- Grep: Search sensitive patterns
- Glob: Find files

### Collaboration with Other Agents
- code-reviewer shares code quality checks
- healthcare-reviewer handles healthcare-specific security issues
- opensource-sanitizer handles open source pre-release checks

### Initial Scan

```bash
npm audit --audit-level=high
npx eslint . --plugin security
```

Search for hardcoded secrets, review high-risk areas: auth, API endpoints, DB queries, file uploads, payments, webhooks.

### OWASP Top 10 Checks

1. **Injection** - Are queries parameterized? Is user input sanitized? Is ORM used safely?
2. **Broken Authentication** - Are passwords hashed (bcrypt/argon2)? Are JWTs verified? Are sessions secure?
3. **Sensitive Data** - Is HTTPS enforced? Are secrets in env vars? Is PII encrypted? Are logs sanitized?
4. **XXE** - Are XML parsers configured securely? Are external entities disabled?
5. **Broken Access Control** - Does every route check auth? Is CORS configured correctly?
6. **Security Misconfiguration** - Have default credentials been changed? Is production debug mode off? Are security headers set?
7. **XSS** - Is output escaped? Is CSP set? Do frameworks auto-escape?
8. **Insecure Deserialization** - Is user input safely deserialized?
9. **Using Components with Known Vulnerabilities** - Are dependencies up to date? Does npm audit pass?
10. **Insufficient Logging & Monitoring** - Are security events logged? Are alerts configured?

### Code Pattern Review

Immediately flag these patterns:

| Pattern | Severity | Fix |
|---------|----------|-----|
| Hardcoded secrets | CRITICAL | Use `process.env` |
| User input in shell commands | CRITICAL | Use safe API or execFile |
| String-concatenated SQL | CRITICAL | Parameterized queries |
| `innerHTML = userInput` | HIGH | Use `textContent` or DOMPurify |
| `fetch(userProvidedUrl)` | HIGH | Whitelist allowed domains |
| Plaintext password comparison | CRITICAL | Use `bcrypt.compare()` |
| Routes without auth check | CRITICAL | Add auth middleware |
| Balance check without lock | CRITICAL | Use `FOR UPDATE` in transaction |
| No rate limiting | HIGH | Add `express-rate-limit` |
| Logging passwords/secrets | MEDIUM | Sanitize log output |

### Key Principles

1. **Defense in Depth** - Multiple layers of security
2. **Least Privilege** - Minimum required permissions
3. **Secure Fail** - Errors should not expose data
4. **Distrust Input** - Validate and sanitize everything
5. **Keep Updated** - Keep dependencies current

### Common False Positives

- Environment variables in `.env.example` (not actual secrets)
- Test credentials in test files (if clearly marked)
- Actually public API keys
- SHA256/MD5 used for checksums (not passwords)

**Always verify context before flagging.**

### Emergency Response

If CRITICAL vulnerability found:
1. Document detailed report
2. Immediately notify project lead
3. Provide secure code examples
4. Verify fix is effective
5. If credentials were exposed, rotate secrets

---

## silent-failure-hunter

### Name and Purpose
Review code for silent failures, swallowed errors, bad fallbacks, and missing error propagation.

### Capabilities
- Empty catch block detection
- Insufficient logging detection
- Dangerous fallback detection
- Error propagation issue detection
- Missing error handling detection

### Applicable Scenarios
- During code review
- When discovering strange bugs
- When pipelines seem green but skipping data
- During exception handling review

### Tools Used
- Read: Read code
- Grep: Search error handling patterns
- Glob: Find source files
- Bash: Run tests

### Collaboration with Other Agents
- code-reviewer shares code quality checks
- security-reviewer handles security-related issues
- mle-reviewer handles ML pipeline issues

### Hunting Targets

#### 1. Empty Catch Blocks
- `catch {}` or swallowed exceptions
- Converting to `null` / empty array but losing error context

#### 2. Insufficient Logging
- Logs without sufficient context
- Wrong error severity
- Log-and-forget handling

#### 3. Dangerous Fallbacks
- Defaults that hide real failures
- `.catch(() => [])`
- Paths that are graceful but make downstream bugs harder to diagnose

#### 4. Error Propagation Issues
- Lost stack traces
- Generic rethrows
- Missing async handling

#### 5. Missing Error Handling
- No timeout or error handling for network/file/database paths
- Transaction work without rollback

### Output Format

Each finding:
- Location
- Severity
- Problem
- Impact
- Fix suggestion

---

## healthcare-reviewer

### Name and Purpose
Review clinical safety, CDSS accuracy, PHI compliance, and medical data integrity for healthcare application code. Designed for EMR/EHR, clinical decision support, and health information systems.

### Capabilities
- CDSS accuracy - Verify drug interaction logic, dose validation rules, and clinical scoring implementations
- PHI/PII protection - Scan for patient data exposure in logs, errors, responses, URLs, and client storage
- Clinical data integrity - Ensure audit trails, locked records, and cascade protections
- Medical data correctness - Verify ICD-10/SNOMED mappings, lab reference ranges, and drug database entries
- Integration compliance - Verify HL7/FHIR message processing and error recovery

### Applicable Scenarios
- EMR/EHR system development
- Clinical decision support systems
- Health information systems
- Medical data processing applications

### Tools Used
- Read: Read medical code
- Grep: Search PHI patterns
- Glob: Find medical-related files

### Collaboration with Other Agents
- security-reviewer handles general security vulnerabilities
- code-reviewer handles code quality issues
- silent-failure-hunter handles silent failure issues

### Key Checks

#### CDSS Engine
- [ ] All drug interaction pairs generate correct alerts (bidirectional)
- [ ] Dose validation rules trigger on out-of-range values
- [ ] Clinical scores match published specifications
- [ ] No false negatives (missed interactions = patient safety event)
- [ ] Malformed input produces errors, not silently passes

#### PHI Protection
- [ ] Patient data not in `console.log`, `console.error`, or error messages
- [ ] PHI not in URL parameters or query strings
- [ ] PHI not in browser localStorage/sessionStorage
- [ ] No `service_role` keys in client code
- [ ] All patient data tables have RLS enabled
- [ ] Cross-facility data isolation verified

#### Clinical Workflows
- [ ] Clinical lock prevents editing (append only)
- [ ] Every clinical data CRUD has audit trail entry
- [ ] Critical alerts are not dismissible (not toast notifications)
- [ ] When clinicians override critical alerts, record reason
- [ ] Red flag symptoms trigger visible alerts

#### Data Integrity
- [ ] Patient records have no CASCADE DELETE
- [ ] Concurrent edit detection (optimistic lock or conflict resolution)
- [ ] Clinical tables have no orphaned records
- [ ] Timestamps use consistent time zones

### Output Format

```
## Healthcare Review: [module/feature]

### Patient Safety Impact: [CRITICAL / HIGH / MEDIUM / LOW / NONE]

### Clinical Accuracy
- CDSS: [check pass/fail]
- Drug DB: [verified/issues]
- Scoring: [matches spec/deviates]

### PHI Compliance
- Exposure vectors check: [list]
- Issues found: [list or none]

### Issues
1. [PATIENT SAFETY / CLINICAL / PHI / TECHNICAL] Description
   - Impact: [potential harm or exposure]
   - Fix: [required changes]

### Verdict: [SAFE TO DEPLOY / NEEDS FIXES / BLOCK — PATIENT SAFETY RISK]
```

### Rules

- When in doubt about clinical accuracy, mark NEEDS REVIEW - never approve uncertain clinical logic
- One missed drug interaction is worse than a hundred false alarms
- PHI exposure is always CRITICAL severity, regardless of how small the leak
- Never approve code that silently swallows CDSS errors

---

## opensource-sanitizer

### Name and Purpose
Verify open source forks are fully sanitized before release. Scan for leaked secrets, PII, internal references, and dangerous files. Uses 20+ regex patterns. Generates PASS/FAIL/PASS-WITH-WARNINGS report.

### Capabilities
- Secrets scanning - Scan for API keys, passwords, tokens
- PII scanning - Scan for emails, IP addresses
- Internal reference scanning - Scan for absolute paths, internal domains
- Dangerous file checking - Check for .env, credentials.json, etc.
- Configuration completeness verification - Verify .env.example
- Git history audit - Check for leaked credentials

### Applicable Scenarios
- Before open source fork release
- Before third-party code audit
- Before code release security checks

### Tools Used
- Read: Read files
- Grep: Search sensitive patterns
- Glob: Find files
- Bash: Run git log, etc.

### Collaboration with Other Agents
- security-reviewer handles general security vulnerabilities
- code-reviewer handles code quality issues
- doc-updater updates documentation

### Workflow

#### Step 1: Secrets Scan (CRITICAL)

Scan every text file (excluding `node_modules`, `.git`, `__pycache__`, `*.min.js`, binaries):

```
# API keys
pattern: [A-Za-z0-9_]*(api[_-]?key|apikey|api[_-]?secret)[A-Za-z0-9_]*\s*[=:]\s*['"]?[A-Za-z0-9+/=_-]{16,}

# AWS
pattern: AKIA[0-9A-Z]{16}
pattern: (?i)(aws_secret_access_key|aws_secret)\s*[=:]\s*['"]?[A-Za-z0-9+/=]{20,}

# Database URL (with credentials)
pattern: (postgres|mysql|mongodb|redis)://[^:]+:[^@]+@[^\s'"]+

# JWT tokens (3-segment)
pattern: eyJ[A-Za-z0-9_-]{20,}\.eyJ[A-Za-z0-9_-]{20,}\.[A-Za-z0-9_-]+

# Private keys
pattern: -----BEGIN\s+(RSA\s+|EC\s+|DSA\s+|OPENSSH\s+)?PRIVATE KEY-----

# GitHub tokens
pattern: gh[pousr]_[A-Za-z0-9_]{36,}
pattern: github_pat_[A-Za-z0-9_]{22,}
```

#### Step 2: PII Scan (CRITICAL)

```
# Personal emails (not generic emails like noreply@, info@)
pattern: [a-zA-Z0-9._%+-]+@(gmail|yahoo|hotmail|outlook|protonmail|icloud)\.(com|net|org)

# Private IP addresses (indicates internal infrastructure)
pattern: (192\.168\.\d+\.\d+|10\.\d+\.\d+\.\d+|172\.(1[6-9]|2\d|3[01])\.\d+\.\d+)

# SSH connection strings
pattern: ssh\s+[a-z]+@[0-9.]+
```

#### Step 3: Internal Reference Scan (CRITICAL)

```
# Absolute paths to specific user home directories
pattern: /home/[a-z][a-z0-9_-]*/
pattern: /Users/[A-Za-z][A-Za-z0-9_-]*/
pattern: C:\\Users\\[A-Za-z]

# Internal secret file references
pattern: \.secrets/
pattern: source\s+~/\.secrets/
```

#### Step 4: Dangerous File Check (CRITICAL)

Verify these do NOT exist:
```
.env (any variant: .env.local, .env.production, .env.*.local)
*.pem, *.key, *.p12, *.pfx, *.jks
credentials.json, service-account*.json
.secrets/, secrets/
.claude/settings.json
sessions/
*.map (source maps expose raw source structure)
node_modules/, __pycache__/, .venv/, venv/
```

#### Step 5: Git History Audit

```bash
# Should be a single initial commit
cd PROJECT_DIR
git log --oneline | wc -l
# If > 1, history not sanitized — FAIL

# Search for possible secrets
git log -p | grep -iE '(password|secret|api.?key|token)' | head -20
```

### Output Format

Generate `SANITIZATION_REPORT.md` in project directory:

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

### Rules

- **Never** show full secret values - truncate to first 4 characters + "..."
- **Never** modify source files - only generate report (SANITIZATION_REPORT.md)
- **Always** scan every text file, not just known extensions
- **Always** check git history, even for new repositories
- **Stay paranoid** - false positives acceptable, missed findings not
- Any single CRITICAL finding in any category = overall FAIL
- Warnings alone = PASS WITH WARNINGS (user decides)
[Return to Agent Index](../README.md)