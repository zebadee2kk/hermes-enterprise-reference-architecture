# Executive summary

## What the platform is

The Hermes Enterprise Reference Architecture describes a governed, observable agentic AI platform designed for small businesses, managed-service providers, and enterprise pilots. It is not a single product. It is a reusable pattern for deploying AI agents that can read, reason, draft, and act — while keeping humans in control of every mutation.

## What business problems it solves

Organisations are adopting AI agents faster than they can govern them. This architecture addresses three common failures:

1. **Uncontrolled autonomy** — agents that can change production systems, send messages, or delete data without oversight.
2. **Invisible decisions** — no trace of why an agent called a tool, chose a model, or wrote to memory.
3. **Unrepeatable deployments** — every new client or project starts from scratch with no shared pattern.

This reference architecture provides a repeatable, governable starting point.

## Why governance matters

Agentic AI systems can act at machine speed across many tools. Without governance:

- a single prompt injection can trigger destructive actions;
- cost can spiral without budget controls;
- client data can leak into logs, memory, or external APIs;
- auditors and regulators have no evidence trail.

The governance model here is **deny-by-default**: nothing is allowed until it has a named owner, a documented purpose, an approval requirement, and an audit trail.

## Typical deployment options

| Pattern | Best starting point |
|---|---|
| Local lab / workstation | Personal experimentation and prompt development |
| Client appliance VM | SMB or MSP delivery with local system access |
| Cloud / VPS hosted | Website, content, and external integrations |
| Enterprise segmented | Regulated environments with formal change control |
| Managed AI jump-box | MSP support operations and remote assistance |

## Example appliance profiles

| Profile | What it does |
|---|---|
| Read-only reconnaissance | Analyses systems and produces documentation without changing anything |
| Content operations | Researches, drafts, and publishes content with approval gates |
| Support desk co-pilot | Triages tickets, suggests resolutions, drafts replies |
| Finance/admin automation | Extracts data, flags anomalies, prepares reconciliations |
| Managed AI jump-box | Secure local management point for support operations |
| Executive reporting | Produces recurring management reports from operational data |

## What a safe first pilot looks like

A responsible first deployment should be:

- **Read-only or draft-only** — the agent can analyse and recommend, but cannot change production systems.
- **Approval-gated** — any mutation requires explicit human approval.
- **Observable** — every model call, tool call, and memory write is traced.
- **Time-boxed** — the pilot has a defined review date and success criteria.

## What outcomes to expect

Organisations that follow this pattern typically see:

- faster, more consistent AI deployment across teams;
- clear audit trails for every agent action;
- reduced risk of uncontrolled autonomous behaviour;
- a reusable foundation for multiple client or internal use cases;
- a governance model that satisfies security review and compliance checks.

## What this is not

This reference architecture is not:

- a single vendor product;
- a fully autonomous or unsupervised system;
- a replacement for upstream documentation from Hermes, LangGraph, n8n, LiteLLM, Mem0, Langfuse, or Promptfoo;
- intended to expose private infrastructure, client data, or credentials.

---

*This summary is intended to be non-technical enough for directors and owners. For full detail, see the [architecture overview](architecture/overview.md) and [security and governance model](security-and-governance.md).*
