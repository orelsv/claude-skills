---
name: security-review
description: Perform security reviews and build security into projects. Use when the user asks to check code or config for vulnerabilities, do a security audit, threat model an app, harden a setup, review auth/secrets handling, or apply OWASP / secure-by-design practices.
---

# Security Review & Secure-by-Design

When the user asks for a security check, audit, or hardening, follow this workflow:

## 1. Identify scope
Determine what is being reviewed: source code, IaC (Terraform/Bicep), cloud config, web app, API, or CI/CD pipeline. State the scope in one line before starting.

## 2. Review checklist (run through ALL that apply)
**Code & dependencies**
- Injection (SQLi, command injection, path traversal), XSS, SSRF, insecure deserialization
- Hardcoded secrets, API keys, connection strings (search the repo: grep for key/token/password patterns)
- Outdated/vulnerable dependencies (suggest `npm audit`, `pip-audit`, Dependabot)

**Authentication & authorization**
- Password storage (must be bcrypt/argon2), session/JWT handling, token expiry and refresh
- Missing authorization checks (IDOR), privilege escalation paths
- MFA support, rate limiting on login endpoints

**Data & transport**
- TLS everywhere, no sensitive data in logs/URLs, encryption at rest for sensitive fields
- Input validation at trust boundaries; output encoding

**Cloud & infrastructure (Azure focus)**
- Public exposure (open NSG rules, public storage, public IPs)
- Managed identity instead of credentials; Key Vault for secrets
- Least-privilege RBAC; diagnostic logging enabled

**CI/CD**
- Secrets in GitHub Actions via secrets store, never in YAML
- Pinned action versions; branch protection; security scanning step (e.g., Trivy, CodeQL, tfsec)

## 3. Report format
For each finding:
```
[SEVERITY: Critical/High/Medium/Low] Title
- Where: file:line or resource
- Why it matters: one-sentence real attack scenario
- Fix: concrete code/config change (show the diff or snippet)
```
Order findings by severity. End with a short "Quick wins" list (fixes under 15 minutes).

## 4. Threat modeling (when asked)
Use lightweight STRIDE: list assets → entry points → apply STRIDE per entry point → top 5 risks with mitigations. Keep it on one page.

## Rules
- Never invent vulnerabilities; if code looks fine, say so and suggest defense-in-depth improvements instead.
- Always explain WHY something is risky in plain language (the user is learning).
- Suggest one automated tool the user can add to the pipeline to catch this class of issue in the future.
