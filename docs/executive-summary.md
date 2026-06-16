# Executive summary

## Problem statement

Organisations are adopting AI agents faster than they can govern them. A single prompt injection can trigger destructive actions. Costs can spiral without budget controls. Client data can leak into logs, memory, or external APIs. Auditors and regulators are left with no evidence trail.

The result is a common pattern: teams experiment with agentic AI, hit a governance or safety wall, and then spend months rebuilding on a stronger foundation. This reference architecture exists to shorten that cycle.

## Solution architecture

The Hermes Enterprise Reference Architecture describes a governed, observable agentic AI platform built around two core layers:

**Hermes Agent** — the operator-facing control surface and orchestration layer. It provides chat and task interaction, deterministic workflow routing, approval prompts, and full run history. Every model call, tool call, and memory write is traced by default.

**WorkOS control plane** — the identity, access, and policy backbone. It enforces who can do what, when, and under which approval model. It integrates with enterprise identity providers, manages role-based access, and ensures every mutating action has an owner, a reason, and an audit record.

Together, these layers let teams deploy agents that can read, reason, draft, and act — while keeping humans in control of every mutation.

## Deployment models

The architecture supports four primary deployment patterns, selected to match the client's infrastructure, risk appetite, and operational capacity.

| Model | Typical environment | Best for |
|---|---|---|
| **Hyper-V appliance** | Windows-hosted virtual machine on client premises | SMB clients already running Hyper-V |
| **VMware appliance** | vSphere/ESXi virtual machine | Regulated environments with existing VMware estates |
| **Proxmox appliance** | Open-source hypervisor on dedicated hardware | Cost-sensitive or open-source-first deployments |
| **Bare-metal appliance** | Dedicated mini-PC or server at client site | Maximum performance, full hardware control, air-gapped options |

All four models share the same software stack and governance model. The choice of hypervisor affects only placement, sizing, and lifecycle management — not the agent behaviour or policy model.

## Reference use cases

The architecture is designed to support multiple governed agent profiles from a single platform. Three reference use cases are prioritised for initial deployment.

**Security operations** — The agent triages alerts, correlates events, drafts incident summaries, and suggests containment steps. All actions are read-only by default; any remediation requires explicit approval. Every decision is traced for post-incident review.

**Content operations** — The agent researches, drafts, and prepares content for publication. Human approval gates sit between draft and publish. The agent can also manage scheduled publishing workflows, content audits, and cross-channel consistency checks.

**Support desk co-pilot** — The agent triages incoming tickets, suggests resolutions from knowledge bases, drafts replies, and escalates when confidence is low. It reduces mean time to first response while keeping a qualified human in the loop for every customer-facing action.

## ROI and value proposition

Organisations that adopt this pattern typically see measurable gains in four areas:

- **Faster time to value** — repeatable deployment patterns eliminate the build-from-scratch cycle. A new client or internal use case can be onboarded in days rather than weeks.
- **Reduced operational risk** — deny-by-default tool access, approval gates, and full audit trails mean fewer incidents caused by uncontrolled agent behaviour.
- **Lower compliance cost** — traceable decisions and documented approval models satisfy security review and regulatory requirements with less manual evidence gathering.
- **Reusable platform investment** — the same architecture serves multiple clients, departments, or use cases, turning a one-off experiment into a durable organisational capability.

The core financial argument is straightforward: a governed platform costs less than a single uncontrolled incident.

## Getting started

A responsible first deployment follows a simple progression.

1. **Read this summary and the architecture overview** — confirm the pattern fits your environment and risk model.
2. **Select a deployment model** — Hyper-V, VMware, Proxmox, or bare-metal, depending on your existing infrastructure.
3. **Start with a read-only pilot** — the agent can analyse, recommend, and draft, but cannot change production systems.
4. **Enable approval gates** — any mutation requires explicit human approval during the pilot phase.
5. **Verify observability** — confirm that every model call, tool call, and memory write is traced before expanding scope.
6. **Expand tool access gradually** — add mutating tools only after the baseline is stable and the approval model is proven.

A pilot that follows this path can typically be operational within one to two weeks, with a clear review date and success criteria defined upfront.

---

*This summary is intended to be non-technical enough for directors and owners. For full detail, see the [architecture overview](architecture/overview.md) and [security and governance model](security-and-governance.md).*
