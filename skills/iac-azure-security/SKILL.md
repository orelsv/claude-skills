---
name: iac-azure-security
description: Write and review secure Terraform / Azure infrastructure as code. Use when the user creates or reviews Terraform for Azure, asks for a secure baseline (NSG, Key Vault, RBAC, managed identity, Sentinel, Log Analytics), or wants to add security scanning (tfsec, Checkov, Trivy) to a pipeline.
---

# Secure IaC for Azure (Terraform)

When the user writes or reviews Terraform for Azure, enforce this baseline:

## Secure-by-default rules
- **Identity**: managed identity over service principal secrets; no credentials in code or tfvars committed to git.
- **Secrets**: Key Vault with RBAC authorization, purge protection on, soft delete on; reference secrets, never inline them.
- **Network**: no `0.0.0.0/0` inbound rules; default-deny NSGs; private endpoints for PaaS where feasible.
- **Storage**: `min_tls_version = "TLS1_2"`, public blob access disabled, infrastructure encryption considered.
- **Logging**: diagnostic settings → Log Analytics workspace for every security-relevant resource.
- **RBAC**: least privilege, role assignments at the narrowest scope (resource group, not subscription, unless justified).
- **State**: remote backend (Azure Storage) with versioning; state contains secrets — restrict access.

## Review workflow
1. Scan for hardcoded secrets and wide-open network rules first (these are critical).
2. Check each resource against the baseline above.
3. Report findings as: [Severity] resource → issue → fixed HCL snippet.
4. Suggest adding a scanner to CI: tfsec or Checkov step in GitHub Actions, with an example job.

## Conventions for this user's lab environment
- VM size: Standard_D2s_v3 (program requirement).
- Images: full URNs, e.g. `Canonical:ubuntu-24_04-lts:server:latest`.
- Region: West Europe. Naming: `rg-`, `law-`, `vm-` prefixes.

## Output rules
- Provide complete, runnable HCL with comments explaining each security control.
- After code, add a 3-line "what to verify in the portal" checklist.
