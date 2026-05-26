# Security Rules

## Rule Overview

ECC Rules security rules are mandatory security checking systems designed to ensure code meets the highest security standards before commit. This rule covers everything from secrets management to MCP server security, and is the baseline that all code changes must adhere to.

## Core Requirements

### Mandatory Pre-Commit Security Checklist

Must complete these security checks before committing any code:

| Check | Description |
|--------|------|
| No hardcoded Secrets | API keys, passwords, tokens must not be hardcoded in source code |
| User input validation | All user input must be validated |
| SQL injection prevention | Use parameterized queries to prevent SQL injection |
| XSS protection | HTML-escape user input |
| CSRF protection | Ensure CSRF protection is enabled |
| Authentication/authorization verification | Confirm authentication and authorization logic is correct |
| Rate limiting | Rate limiting must be enabled on all endpoints |
| Error message security | Error messages must not leak sensitive data |