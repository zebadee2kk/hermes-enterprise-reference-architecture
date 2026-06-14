# Architecture overview

## Purpose

This document describes the reference architecture for a governed Hermes-style agentic AI platform. It is written to be reusable across:

- internal homelab / RichardOS deployments;
- client agent appliances;
- MSP-managed AI jump-boxes;
- enterprise pilots where governance, observability, and approval trails matter.

## Logical architecture

```text
Human operator / client user
        |
        v
Agent interface / Hermes-style control surface
        |
        v
LangGraph supervisor / workflow router
        |
        +--------------------+
        |                    |
        v                    v
Tool Proxy / governed MCP    n8n workflow automation
        |                    |
        v                    v
Approved external tools      Scheduled and event-driven workflows
        |
        v
GitHub, NetBox, ticketing, email, calendar, browser, APIs

Model calls flow through:

Agent / workflow -> LiteLLM -> model provider or local inference
                         |
                         v
                   Langfuse / OTel traces

Memory flow:

Agent / workflow -> memory policy -> Mem0/Qdrant + session search + optional archival memory
                         |
                         v
                 review/export/hygiene process
```

## Layer responsibilities

### 1. Agent interface

The agent interface is the human-facing control surface. It should provide chat/task interaction, clear task boundaries, approval prompts, access to run history, and visible handover notes for future sessions.

### 2. Orchestration layer

The orchestration layer turns a user request into governed work. In this reference pattern, LangGraph or an equivalent deterministic orchestration system is responsible for routing tasks, preserving run context, propagating trace IDs, and deciding when to call tools, memory, model gateways, or workflow automations.

### 3. Workflow automation layer

n8n is used for repeatable workflows rather than as a second autonomous brain. Suitable workflows include scheduled reports, publishing workflows, ticket triage, memory hygiene, and integrations with CRMs, helpdesks, websites, or internal APIs.

### 4. Model gateway

LiteLLM or an equivalent gateway should sit between agents and model providers. It provides model routing, provider abstraction, usage capture, budget control, fallback models, and a clean integration point for Langfuse callbacks.

### 5. Memory layer

The memory layer should be managed, not treated as an uncontrolled dumping ground. The baseline pattern is:

- compact native/working memory for immediate steering facts;
- Mem0 over Qdrant for durable vector-backed memory;
- session search for keyword retrieval over historical sessions;
- optional archival memory for long summaries;
- optional Graphiti/Zep only where temporal relationship memory justifies the operational cost.

### 6. Tool gateway

The Tool Proxy or governed MCP gateway is the enforcement boundary. It should provide:

- deny-by-default policy;
- explicit allow/deny/approval states;
- run-context validation;
- secret redaction;
- audit logging;
- hard-blocks for destructive operations;
- clear separation between read-only tools and mutating tools.

### 7. Observability and evaluation

Agentic infrastructure needs observability that understands LLM calls, prompts, traces, cost, memory writes, and tool decisions. The reference pattern uses Langfuse plus OpenTelemetry conventions, with Promptfoo or an equivalent test runner for regression evals.

## Reference deployment tiers

| Tier | Description | Typical use |
|---|---|---|
| Lab | Local laptop/server deployment | Development, experiments, demos |
| Client appliance | VM or small physical appliance at client site | Managed AI operations, jump-box, local integrations |
| VPS/cloud | Hosted platform with secure tunnels | Website/content ops, scheduled workflows, external integrations |
| Enterprise | Segmented deployment with formal governance | Regulated clients, larger organisations, ISO/security alignment |

## Minimum viable governed deployment

A useful first deployment should include:

- agent interface;
- LiteLLM gateway;
- basic memory layer;
- Tool Proxy with read-only tools;
- Langfuse traces;
- one repeatable workflow;
- documented approval model;
- backup and restore runbook.

Do not add broad tool access or autonomous write capability until the above is stable.
