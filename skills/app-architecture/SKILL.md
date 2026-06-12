---
name: app-architecture
description: Design architecture for applications, web apps, and projects. Use when the user asks to design, plan, or structure a new app/project, choose a tech stack, define folder structure, draw architecture diagrams, or write an Architecture Decision Record (ADR).
---

# Application Architecture Designer

When the user asks to design an application or project architecture, follow this workflow:

## 1. Clarify requirements first (max 3 questions)
Ask only what is essential: target users, expected scale, hosting (cloud/local), budget constraints, must-have integrations. If the user already provided this, do NOT ask again.

## 2. Propose architecture in this order
1. **One-paragraph summary** — what we are building and the simplest architecture that works.
2. **Component diagram** — use Mermaid (```mermaid) so it renders in editors and GitHub.
3. **Tech stack table** — layer | choice | why | simpler alternative.
4. **Folder structure** — a tree of the repo with one-line comments per folder.
5. **Data flow** — how a request travels through the system (numbered steps).
6. **Security & deployment notes** — auth strategy, secrets handling, CI/CD outline.

## 3. Principles
- Start with the simplest architecture that satisfies requirements (monolith before microservices).
- Prefer boring, well-documented technology over trendy options.
- Every architectural choice must include a trade-off ("we gain X, we accept Y").
- For cloud projects default to: managed identity over keys, infrastructure as code (Terraform), least privilege.
- Always state what we are explicitly NOT building in v1 (scope control).

## 4. ADR format (when user asks to document a decision)
```
# ADR-NNN: Title
Status: Proposed | Accepted
Context: (why a decision is needed)
Decision: (what we chose)
Consequences: (positive, negative, risks)
Alternatives considered: (and why rejected)
```

## Output rules
- Diagrams in Mermaid, not ASCII art.
- Keep the first answer under ~600 words; offer to deep-dive into any component.
