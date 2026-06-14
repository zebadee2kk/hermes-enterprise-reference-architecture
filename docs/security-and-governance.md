# Security and governance model

## Purpose

This document defines the minimum security and governance controls for a Hermes-style agentic AI platform.

The central idea is simple: agentic systems should become more capable only after they become more governable.

## Governance boundaries

| Boundary | Required control |
|---|---|
| Model access | Route through a gateway where possible; log model, cost, and provider. |
| Tool access | Route through Tool Proxy or governed MCP gateway. |
| Memory writes | Classify, deduplicate, namespace, and audit. |
| External mutations | Require explicit approval for writes, deletes, publishes, deploys, and sends. |
| Secrets | Store in OS keyring, Vault, cloud secret manager, or client-approved equivalent. |
| Logs and traces | Redact secrets and personal data before persistence. |
| Client data | Keep client data inside client-approved systems and repositories. |

## Autonomy levels

| Level | Name | Description | Examples |
|---|---|---|---|
| A0 | Advice only | Agent can analyse and recommend, but cannot call tools. | Architecture advice, draft plans |
| A1 | Read-only tools | Agent can read approved sources. | Read GitHub issue, fetch NetBox device, query docs |
| A2 | Draft mutations | Agent can prepare changes but not apply them. | Draft PR, draft email, draft config |
| A3 | Approval-gated mutation | Agent can apply changes after explicit approval. | Create issue, open PR, trigger n8n workflow |
| A4 | Bounded automation | Agent can run pre-approved recurring workflows within strict scope. | Weekly report, memory hygiene |
| A5 | High autonomy | Reserved for mature systems with strong guardrails. | Not recommended for early client deployments |

## Default policy

The default policy is deny-by-default.

A new tool, workflow, or memory sink should not be available to the agent until it has:

1. a named owner;
2. a documented purpose;
3. allowed operations;
4. blocked operations;
5. approval requirements;
6. audit output;
7. rollback or recovery guidance.

## Hard-blocked operations

The following should normally be blocked unless a specific client-approved exception exists:

- deleting repositories, projects, production data, or backups;
- force-pushing or rewriting shared history;
- extracting broad secrets or credential stores;
- sending external communications without approval;
- publishing website or social content without approval;
- changing firewall, VPN, identity, or production access controls without approval;
- running arbitrary shell commands on production systems.

## Approval gate pattern

Every approval gate should capture:

- requested action;
- reason;
- affected system;
- risk level;
- expected change;
- rollback plan;
- operator approval status;
- timestamp and actor.

Use the approval template in `templates/approval-gate.md`.

## Secret handling

Secrets must not be stored in this repository or any public reference repository.

Acceptable stores:

- OS keyring;
- 1Password / Bitwarden / enterprise password manager;
- HashiCorp Vault;
- Azure Key Vault;
- AWS Secrets Manager;
- GCP Secret Manager;
- client-approved local secret store.

Secrets must not be placed in:

- prompts;
- memory;
- logs;
- GitHub issues;
- Markdown docs;
- exported traces;
- support screenshots unless redacted.

## Client deployment rule

For client environments, the agent must start read-only. Mutating workflows should be introduced only after:

- the client understands the approval model;
- rollback paths are documented;
- logging and redaction are proven;
- the scope of tools is deliberately narrow.

## Evidence and audit trail

Each meaningful action should produce enough evidence for a future operator to answer:

- who or what triggered this;
- what was read;
- what was changed;
- what model was used;
- what memory was read or written;
- what tool was called;
- whether approval was required and granted;
- how to reverse or repair the action.
