# Managed AI Appliance — Service Description

## Purpose

This document describes the **Managed AI Appliance** offering: a governed, on-premises AI platform deployed and operated inside a client's perimeter. It is intended to support client discovery calls, proposals, and scoping conversations.

> **Scope:** This is a service description, not a commercial proposal. Pricing is provided separately per engagement.

---

## What the Managed AI Appliance Is

The Managed AI Appliance is a pre-configured platform that delivers governed AI capabilities — agentic reasoning, workflow automation, local model serving, and observability — as a single deployable unit running on infrastructure the client controls.

It is managed end-to-end by our team: deployment, configuration, monitoring, updates, governance policy, and ongoing support.

**In one sentence:** *AI infrastructure as a service, deployed inside your perimeter, under your governance, with our operational expertise.*

---

## Who It Is For

| Audience | Why They Care |
|----------|---------------|
| **CTOs / VP Engineering** | Reduce time-to-value for AI initiatives while maintaining security and compliance |
| **IT Operations** | Turnkey deployment with managed updates, monitoring, and support |
| **Security & Governance teams** | Approval gates, audit logging, data sovereignty, and policy enforcement built in |
| **Line-of-business leaders** | Access to AI-powered workflows (support desk, content ops, reporting) without building from scratch |

---

## What Problems It Solves

1. **Fragmented AI tooling** — Teams adopt point solutions that do not integrate and create shadow IT.
2. **Security and compliance risk** — Unsanctioned AI usage bypasses data-handling policies.
3. **Operational overhead** — Internal teams lack bandwidth to deploy, tune, and maintain AI infrastructure.
4. **Unclear governance** — No approval model for what AI can do, what data it can access, and who is accountable.
5. **Pilot-to-production gap** — Successful proofs of concept fail to scale because they were not designed for production governance.

---

## Deployment Options

The appliance is delivered as a virtual machine or bare-metal deployment, standardised across four target platforms.

### Hyper-V

| Attribute | Detail |
|-----------|--------|
| **Target** | Windows Server 2022+, Azure Stack HCI |
| **Format** | VHDX virtual machine image |
| **Management** | Windows Admin Center, SCVMM |
| **Networking** | Virtual switch, VLAN isolation, optional NIC passthrough |
| **Storage** | VHDX on SMB3 or local NVMe |
| **Best for** | Organisations already standardised on Microsoft hypervisor |

### VMware vSphere / ESXi

| Attribute | Detail |
|-----------|--------|
| **Target** | vSphere 7+, vCenter-managed clusters |
| **Format** | OVF/OVA template |
| **Management** | vCenter, vRealize Operations |
| **Networking** | Distributed Port Group, NSX-T micro-segmentation |
| **Storage** | VMFS6, vSAN, or NFS datastore |
| **Best for** | Enterprises with existing VMware investments and DRS/HA requirements |

### Proxmox VE

| Attribute | Detail |
|-----------|--------|
| **Target** | Proxmox VE 8+ clusters |
| **Format** | QCOW2 or raw disk image |
| **Management** | Proxmox Web UI, API, Terraform provider |
| **Networking** | Linux Bridge, OVS, VLAN-aware |
| **Storage** | ZFS, Ceph, LVM-Thin, NFS |
| **Best for** | Cost-conscious deployments, open-source-first organisations, labs |

### Bare-Metal

| Attribute | Detail |
|-----------|--------|
| **Target** | Dell PowerEdge, HPE ProLiant, Supermicro, or client-approved hardware |
| **Format** | Ubuntu Server 24.04 LTS pre-installed via PXE or USB image |
| **Management** | IPMI/BMC, Ansible, Terraform |
| **Networking** | Dedicated NICs, VLAN, optional SR-IOV |
| **Storage** | Direct-attached NVMe, RAID-1/10, or ZFS mirror |
| **Best for** | Maximum performance, GPU-heavy workloads, air-gapped environments |

---

## Software Stack

```
┌─────────────────────────────────────────────────────────────────┐
│                     Managed AI Appliance                        │
│                                                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │    Hermes     │  │  LM Studio   │  │        n8n           │  │
│  │  (Agent       │  │  (Local      │  │  (Workflow           │  │
│  │   Runtime)    │  │   Model      │  │   Automation)        │  │
│  │              │  │   Serving)   │  │                      │  │
│  └──────┬───────┘  └──────┬───────┘  └──────────┬───────────┘  │
│         │                 │                      │              │
│         └─────────────────┼──────────────────────┘              │
│                           │                                     │
│                  ┌────────▼────────┐                            │
│                  │    Langfuse     │                            │
│                  │ (Observability  │                            │
│                  │  & Tracing)     │                            │
│                  └─────────────────┘                            │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │            Qdrant (Vector Store / Memory)                │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │            OS: Ubuntu Server 24.04 LTS                   │  │
│  │            Runtime: Docker / Podman                      │  │
│  │            Orchestration: Docker Compose / K3s           │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

### Component Details

| Component | Role | Why It Is Included |
|-----------|------|--------------------|
| **Hermes** | Agent runtime — tool use, memory, approval gates, multi-step reasoning | Core intelligence layer; governed by design |
| **LM Studio** | Local model serving — runs open-weight LLMs on appliance GPU/CPU | Data never leaves the perimeter; supports model choice |
| **n8n** | Workflow automation — connects internal systems, triggers, and approvals | No-code/low-code integration with existing tools |
| **Langfuse** | Observability — traces, prompt versioning, eval dashboards, cost tracking | Required for audit, debugging, and continuous improvement |
| **Qdrant** | Vector store — semantic search, memory retrieval, RAG pipelines | Powers contextual memory and document-grounded reasoning |

---

## Appliance Profiles

### Starter

| Attribute | Value |
|-----------|-------|
| **Use case** | Team pilots, proofs of concept, dev/test |
| **CPU** | 8 cores |
| **RAM** | 32 GB |
| **GPU** | Optional (CPU inference) |
| **Storage** | 500 GB NVMe |
| **Models** | 1–2 small models (7B–13B parameters) |
| **Concurrent users** | 5–10 |
| **High availability** | Single node |

### Standard

| Attribute | Value |
|-----------|-------|
| **Use case** | Production team deployment |
| **CPU** | 16 cores |
| **RAM** | 64 GB |
| **GPU** | 1× NVIDIA L4 / RTX 4000 Ada (16 GB VRAM) |
| **Storage** | 1 TB NVMe |
| **Models** | 2–3 models (up to 70B parameters with quantisation) |
| **Concurrent users** | 20–50 |
| **High availability** | Optional warm standby |

### Enterprise

| Attribute | Value |
|-----------|-------|
| **Use case** | Organisation-wide production |
| **CPU** | 32+ cores |
| **RAM** | 128 GB |
| **GPU** | 2× NVIDIA L40S / A6000 (48 GB VRAM each) |
| **Storage** | 2 TB NVMe + network-attached |
| **Models** | Multiple large models, fine-tuned variants |
| **Concurrent users** | 100+ |
| **High availability** | Active-passive cluster with automated failover |

---

## Governance and Approval Model

The appliance enforces a **governance-first** approach. No mutating action is taken without appropriate approval.

### Autonomy Levels

| Level | Name | Description | Examples |
|-------|------|-------------|----------|
| **A0** | Advice only | Agent can analyse and recommend, but cannot call tools | Architecture advice, draft plans |
| **A1** | Read-only tools | Agent can read approved sources | Read GitHub issue, fetch NetBox device, query docs |
| **A2** | Draft mutations | Agent can prepare changes but not apply them | Draft PR, draft email, draft config |
| **A3** | Approval-gated mutation | Agent can apply changes after explicit approval | Create issue, open PR, trigger n8n workflow |
| **A4** | Bounded automation | Agent can run pre-approved recurring workflows within strict scope | Weekly report, memory hygiene |

> **Default policy:** Deny-by-default. New tools, workflows, or memory sinks are not available to the agent until they have a named owner, documented purpose, allowed and blocked operations, approval requirements, audit output, and rollback guidance.

### Approval Tiers

| Tier | Action | Approver | Example |
|------|--------|----------|---------|
| **Tier 0** | Read-only, no side effects | Auto-approved | Answering questions, summarising documents |
| **Tier 1** | Read internal data | Pre-approved policy | Querying internal knowledge bases |
| **Tier 2** | Write/modify internal systems | Named approver | Creating tickets, updating records |
| **Tier 3** | External actions, data egress | Dual approver + audit | Sending emails, API calls to third parties |

### Hard-Blocked Operations

The following are blocked unless a specific client-approved exception exists:

- Deleting repositories, projects, production data, or backups
- Force-pushing or rewriting shared history
- Extracting broad secrets or credential stores
- Sending external communications without approval
- Publishing website or social content without approval
- Changing firewall, VPN, identity, or production access controls without approval
- Running arbitrary shell commands on production systems

### Audit and Compliance

- Every action is **traced** in Langfuse with full context (prompt, tool call, response, approver).
- **Approval logs** are immutable and exportable for compliance reviews.
- **Data residency:** All data remains within the appliance boundary unless explicitly approved for egress.
- **Model cards** and **system prompts** are version-controlled and reviewable.

---

## SLA Tiers

| Tier | Response Time | Resolution Target | Coverage | Includes |
|------|--------------|-------------------|----------|----------|
| **Standard** | 4 business hours | 2 business days | Business hours (Mon–Fri, 9–5) | Remote diagnostics, patch updates, config changes |
| **Performance** | 1 hour | 1 business day | 24/5 | All Standard + priority escalation, monthly health checks |
| **Enterprise** | 15 minutes | 4 hours | 24/7/365 | All Performance + dedicated TAM, quarterly architecture reviews, on-site support option |

---

## Pricing Model Outline

Pricing is provided separately per engagement. The following outlines the cost structure categories so clients can anticipate the shape of a proposal.

### Hardware Lease

| Model | Description |
|-------|-------------|
| **Customer-provided** | Client supplies the server, hypervisor host, or cloud tenancy. We deploy and manage the appliance on it. |
| **Leased appliance** | We provide the physical or virtual appliance hardware on a monthly lease. Includes warranty and hardware replacement. |
| **Hybrid** | Client provides the compute; we provide specialised components (GPU, storage) on lease. |

### Subscription Tiers

| Tier | What It Covers |
|------|----------------|
| **Software subscription** | Access to the managed software stack (Hermes, LM Studio, n8n, Langfuse, Qdrant), updates, and monitoring |
| **Management subscription** | Remote management, patching, configuration, and governance policy maintenance |
| **Support subscription** | SLA-backed support at the chosen tier (Standard / Performance / Enterprise) |

### Per-Project Services

| Service | Description |
|---------|-------------|
| **Initial deployment** | One-time setup, network integration, model selection, and pilot configuration |
| **Custom integration** | Connecting the appliance to bespoke internal systems beyond standard connectors |
| **Model fine-tuning** | Custom training or fine-tuning of models on client data |
| **On-site support** | Physical presence for installation, troubleshooting, or training |
| **Compliance engagement** | SOC 2, ISO 27017, or other certification support for the appliance |

> **Note:** No pricing figures are included in this document. All commercial terms are provided in a separate proposal.

---

## Deployment Workflow

### Phase 1 — Discovery and Scoping (Weeks 1–2)

- Client needs assessment and use-case prioritisation
- Infrastructure audit (hypervisor, network, storage, GPU availability)
- Security and governance requirements gathering
- Appliance profile selection (Starter / Standard / Enterprise)
- Proposal and SLA agreement

### Phase 2 — Provisioning (Weeks 3–4)

- Appliance image prepared and hardened
- Network segment configured (VLAN, firewall rules, DNS)
- Base software stack deployed (Hermes, LM Studio, n8n, Langfuse, Qdrant)
- Initial model selection and loading
- Integration with 1–2 identity sources (LDAP, SSO)

### Phase 3 — Pilot (Weeks 5–8)

- Deploy to isolated network segment
- Configure Tier 0–1 approval policies only
- Onboard 3–5 pilot users
- Run baseline evaluations
- First governance review

### Phase 4 — Production Rollout (Weeks 9–12)

- Enable Tier 2–3 approval workflows
- Integrate with production systems (ticketing, knowledge base, monitoring)
- Scale to target user count
- Establish ongoing support cadence
- Handover documentation and runbooks

### Phase 5 — Ongoing Operations

- Continuous monitoring and alerting
- Monthly patching and updates
- Quarterly governance reviews
- Annual architecture review and capacity planning

---

## Support Model

### What Is Included in the Managed Service

- **Deployment and configuration** — Initial setup, network integration, model selection
- **Monitoring and alerting** — Proactive health checks, resource utilisation, error detection
- **Updates and patches** — OS security patches, component updates, model version upgrades
- **Backup and recovery** — Configuration backups, model weights, workflow definitions
- **Governance reviews** — Quarterly review of approval policies, access controls, audit logs

### What Requires a Separate Agreement

- Custom model training or fine-tuning
- Integration with bespoke internal systems beyond standard connectors
- On-site hardware installation and rack-mounting
- Compliance certifications (SOC 2, ISO 27001) for the appliance itself

### Support Channels

| Channel | Availability | Use For |
|---------|-------------|---------|
| **Ticketing portal** | 24/7 submission, SLA-based response | All requests |
| **Email** | 24/7 submission, SLA-based response | Non-urgent requests, documentation |
| **Phone / video** | Per SLA tier | Escalations, critical issues |
| **Dedicated TAM** | Enterprise tier only | Strategic guidance, architecture reviews |

---

## Safe Pilot Approach

We recommend a **phased rollout** to validate value before scaling. The default pilot follows four phases:

### Phase 1 — Foundation (Weeks 1–2)

- Deploy appliance in isolated network segment
- Configure Tier 0–1 approval policies only
- Onboard 3–5 pilot users
- Run baseline evaluations

### Phase 2 — Expansion (Weeks 3–6)

- Enable Tier 2 approval workflows
- Integrate with 1–2 internal systems (e.g., ticketing, knowledge base)
- Onboard 10–20 users
- First governance review

### Phase 3 — Production (Weeks 7–12)

- Enable Tier 3 workflows with dual-approver policy
- Full network integration
- Scale to target user count
- Establish ongoing support cadence

### Phase 4 — Optimisation (Ongoing)

- Model performance tuning based on usage patterns
- Workflow refinement and automation expansion
- Quarterly architecture and governance reviews

---

## What Is Deliberately Not Included

To maintain a clear, realistic, and governance-led offering, the following are **explicitly excluded** from the base managed appliance:

1. **Unsupervised autonomy** — The appliance does not take irreversible actions without human approval.
2. **Public-facing chatbots** — The appliance is designed for internal organisational use, not customer-facing applications (available as a separate engagement).
3. **Guaranteed accuracy** — AI outputs are probabilistic; the appliance provides governance and observability, not accuracy warranties.
4. **Data processing agreements for client data** — The appliance processes data on behalf of the client; DPAs are the client's responsibility unless separately negotiated.
5. **Third-party SaaS integrations out of the box** — Custom integrations require scoping and may incur additional effort.
6. **Pricing** — This document describes the service, not commercial terms. Pricing is provided separately per engagement.

---

## Acceptance Criteria

- [x] The document supports a client discovery call or proposal.
- [x] The offer is clear, realistic, and governance-led.
- [x] Deployment options cover Hyper-V, VMware, Proxmox, and bare-metal.
- [x] Software stack (Hermes, LM Studio, n8n, Langfuse, Qdrant) is documented.
- [x] SLA tiers are defined with response and resolution targets.
- [x] Pricing model outline covers hardware lease, subscription, and per-project categories.
- [x] Deployment workflow is described from discovery through ongoing operations.
- [x] Support model covers included items, separate agreements, and channels.
- [x] Safe pilot approach is outlined with phased rollout.
- [x] Deliberately excluded items are stated explicitly.
- [x] No private pricing or exaggerated claims are included.
