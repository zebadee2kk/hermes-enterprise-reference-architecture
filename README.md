# Hermes Enterprise Reference Architecture

A reusable reference architecture for deploying Hermes-style agentic AI platforms in small business, MSP, client-appliance, and enterprise environments.

This repository is intended to be both:

1. a **client-safe architecture pack** for explaining how governed agentic infrastructure should be designed; and
2. an **internal delivery blueprint** for turning RichardOS / Hermes / HamNet learnings into repeatable client deployments.

It deliberately avoids personal secrets, live infrastructure details, customer data, and any configuration that should remain inside private operational repositories.

## What this architecture is for

The architecture describes a managed agentic AI platform with:

- a central agent/operator interface;
- model routing through an AI gateway;
- durable memory with hygiene and review controls;
- observable agent traces and evaluations;
- approval-gated tool access;
- repeatable workflows via orchestration and automation;
- deployment patterns for local VM, Hyper-V, VMware, cloud VPS, and managed client appliance use cases.

## Core design principles

| Principle | Meaning |
|---|---|
| Govern first | Tool access, writes, secrets, and autonomy are controlled before extra tools are added. |
| Observable by default | Model calls, tool calls, memory writes, workflow runs, and approvals must be traceable. |
| Memory is managed | Memory is classified, deduplicated, namespaced, reviewable, and exportable. |
| Local-first where useful | Client and homelab deployments can run locally, with cloud services used deliberately. |
| Human approval for mutation | High-impact actions require explicit approval gates and audit trails. |
| Client-safe packaging | The reusable pattern must not expose personal infrastructure, client data, or privileged credentials. |

## Reference components

| Layer | Reference component | Purpose |
|---|---|---|
| Agent interface | Hermes / equivalent operator UI | Main human-agent interaction layer |
| Orchestration | LangGraph | Deterministic agent workflow control |
| Workflow automation | n8n | Repeatable operational workflows and integrations |
| Model gateway | LiteLLM | Model routing, provider abstraction, spend/cost capture |
| Memory | Mem0 + Qdrant, session search, optional Letta/Graphiti | Durable recall, archival memory, temporal reasoning where justified |
| Observability | Langfuse + OpenTelemetry | LLM traces, debugging, prompt/version tracking, eval context |
| Evaluation | Promptfoo or equivalent | Regression tests, model comparison, prompt/security evals |
| Tool gateway | Tool Proxy / governed MCP gateway | Policy enforcement, approval, audit, redaction, and hard-blocks |
| Secrets | OS keyring / Vault / client secret store | No raw secrets in repos, prompts, logs, or memory |
| Documentation | Markdown / Obsidian / GitHub | Operator-readable architecture and runbooks |

## Repository map

```text
.
├── README.md
├── docs/
│   ├── architecture/
│   │   └── overview.md
│   ├── client-appliance-profiles.md
│   ├── deployment-patterns.md
│   ├── memory-observability-evals.md
│   ├── roadmap.md
│   └── security-and-governance.md
└── templates/
    ├── approval-gate.md
    ├── client-discovery-checklist.md
    └── deployment-checklist.md
```

## Recommended reading order

1. [Architecture overview](docs/architecture/overview.md)
2. [Security and governance model](docs/security-and-governance.md)
3. [Memory, observability and evaluation](docs/memory-observability-evals.md)
4. [Deployment patterns](docs/deployment-patterns.md)
5. [Client appliance profiles](docs/client-appliance-profiles.md)
6. [Roadmap](docs/roadmap.md)

## Non-goals

This repository is not intended to:

- mirror the full private `hermes-mgmt` repository;
- contain live customer configuration;
- contain API keys, tokens, SSH keys, credentials, logs, or exports;
- replace upstream Hermes, LangGraph, n8n, LiteLLM, Mem0, Langfuse, or Promptfoo documentation;
- prescribe one vendor-only architecture.

## Status

Initial scaffold created on 2026-06-14. It should now be onboarded into `hermes-mgmt` so future Hermes agents can maintain it as the public/reference companion to the private management plane.
