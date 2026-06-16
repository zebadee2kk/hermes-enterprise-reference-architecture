# Managed AI Appliance Service

## What It Is

The **Managed AI Appliance** is a pre-configured, on-premises (or private-cloud) platform that delivers governed AI capabilities to your organisation. It combines curated AI models, workflow automation, observability, and security controls into a single deployable unit — managed end-to-end by our team.

Think of it as **"AI infrastructure as a service"** deployed inside your perimeter, under your governance, with our operational expertise.

---

## Who It Is For

| Audience | Why They Care |
|----------|---------------|
| **CTOs / VP Engineering** | Reduce time-to-value for AI initiatives while maintaining security and compliance |
| **IT Operations** | Turnkey deployment with managed updates, monitoring, and support |
| **Security & Governance teams** | Approval gates, audit logging, data-sovereignty, and policy enforcement built in |
| **Line-of-business leaders** | Access to AI-powered workflows (support desk, content ops, data analysis) without building from scratch |

---

## Problems It Solves

1. **Fragmented AI tooling** — Teams adopt point solutions that don't integrate and create shadow IT.
2. **Security & compliance risk** — Unsanctioned AI usage bypasses data-handling policies.
3. **Operational overhead** — Internal teams lack bandwidth to deploy, tune, and maintain AI infrastructure.
4. **Unclear governance** — No approval model for what AI can do, what data it can access, and who is accountable.
5. **Pilot-to-production gap** — Successful PoCs fail to scale because they weren't designed for production governance.

---

## Deployment Options

### Hyper-V

| Attribute | Detail |
|-----------|--------|
| **Target** | Windows Server environments, Azure Stack HCI |
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
| **Networking** | Linux Bridge, OSDN, VLAN-aware |
| **Storage** | ZFS, Ceph, LVM-Thin, NFS |
| **Best for** | Cost-conscious deployments, open-source-first organisations, labs |

---

## Software Stack

```
┌─────────────────────────────────────────────────────────────┐
│                    Managed AI Appliance                      │
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │   Hermes     │  │  LM Studio  │  │       n8n           │ │
│  │  (Agent      │  │  (Local     │  │  (Workflow          │ │
│  │   Runtime)   │  │   Model     │  │   Automation)       │ │
│  │             │  │   Serving)  │  │                     │ │
│  └──────┬──────┘  └──────┬──────┘  └──────────┬──────────┘ │
│         │                │                     │            │
│         └────────────────┼─────────────────────┘            │
│                          │                                  │
│                 ┌────────▼────────┐                         │
│                 │    Langfuse     │                         │
│                 │ (Observability  │                         │
│                 │  & Tracing)     │                         │
│                 └─────────────────┘                         │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              OS: Ubuntu Server 24.04 LTS              │  │
│  │              Container Runtime: Docker / Podman       │  │
│  │              Orchestration: Docker Compose / K3s      │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

### Component Details

| Component | Role | Why It's Included |
|-----------|------|-------------------|
| **Hermes** | Agent runtime — handles tool use, memory, approval gates, and multi-step reasoning | Core intelligence layer; governed by design |
| **LM Studio** | Local model serving — runs open-weight LLMs on appliance GPU/CPU | Data never leaves the perimeter; supports model choice |
| **n8n** | Workflow automation — connects internal systems, triggers, and approvals | No-code/low-code integration with existing tools |
| **Langfuse** | Observability — traces, prompt versioning, eval dashboards, cost tracking | Required for audit, debugging, and continuous improvement |

---

## Appliance Profiles

### Profile: Starter

| Attribute | Value |
|-----------|-------|
| **Use case** | Team pilots, PoCs, dev/test |
| **CPU** | 8 cores |
| **RAM** | 32 GB |
| **GPU** | Optional (CPU inference) |
| **Storage** | 500 GB NVMe |
| **Models** | 1–2 small models (7B–13B params) |
| **Concurrent users** | 5–10 |
| **HA** | Single node |

### Profile: Standard

| Attribute | Value |
|-----------|-------|
| **Use case** | Production team deployment |
| **CPU** | 16 cores |
| **RAM** | 64 GB |
| **GPU** | 1× NVIDIA L4 / RTX 4000 Ada (16 GB VRAM) |
| **Storage** | 1 TB NVMe |
| **Models** | 2–3 models (up to 70B params with quantisation) |
| **Concurrent users** | 20–50 |
| **HA** | Optional warm standby |

### Profile: Enterprise

| Attribute | Value |
|-----------|-------|
| **Use case** | Organisation-wide production |
| **CPU** | 32+ cores |
| **RAM** | 128 GB |
| **GPU** | 2× NVIDIA L40S / A6000 (48 GB VRAM each) |
| **Storage** | 2 TB NVMe + network-attached |
| **Models** | Multiple large models, fine-tuned variants |
| **Concurrent users** | 100+ |
| **HA** | Active-passive cluster with automated failover |

---

## Governance and Approval Model

The appliance enforces a **governance-first** approach. Nothing runs without appropriate approval.

### Approval Tiers

| Tier | Action | Approver | Example |
|------|--------|----------|---------|
| **Tier 0** | Read-only, no side effects | Auto-approved | Answering questions, summarising documents |
| **Tier 1** | Read internal data | Pre-approved policy | Querying internal knowledge bases |
| **Tier 2** | Write/modify internal systems | Named approver | Creating tickets, updating records |
| **Tier 3** | External actions, data egress | Dual approver + audit | Sending emails, API calls to third parties |

### Audit & Compliance

- Every action is **traced** in Langfuse with full context (prompt, tool call, response, approver).
- **Approval logs** are immutable and exportable for compliance reviews.
- **Data residency**: All data remains within the appliance boundary unless explicitly approved for egress.
- **Model cards** and **system prompts** are version-controlled and reviewable.

---

## Support Model

### SLA Tiers

| Tier | Response Time | Resolution Target | Coverage | Includes |
|------|--------------|-------------------|----------|----------|
| **Standard** | 4 business hours | 2 business days | Business hours (Mon–Fri, 9–5) | Remote diagnostics, patch updates, config changes |
| **Premium** | 1 hour | 1 business day | 24/5 | All Standard + priority escalation, monthly health checks |
| **Enterprise** | 15 minutes | 4 hours | 24/7/365 | All Premium + dedicated TAM, quarterly architecture reviews, on-site support option |

### What's Included in Managed Service

- **Deployment & configuration** — Initial setup, network integration, model selection
- **Monitoring & alerting** — Proactive health checks, resource utilisation, error detection
- **Updates & patches** — OS security patches, component updates, model version upgrades
- **Backup & recovery** — Configuration backups, model weights, workflow definitions
- **Governance reviews** — Quarterly review of approval policies, access controls, audit logs

### What Requires Separate Agreement

- Custom model training or fine-tuning
- Integration with bespoke internal systems beyond standard connectors
- On-site hardware installation and rack-mounting
- Compliance certifications (SOC 2, ISO 27001) for the appliance itself

---

## Safe Pilot Approach

We recommend a **phased rollout** to validate value before scaling:

### Phase 1 — Foundation (Weeks 1–2)

- Deploy appliance in isolated network segment
- Configure Tier 0–1 approval policies only
- Onboard 3–5 pilot users
- Run baseline evaluations (see [Promptfoo Baseline](../evaluation/promptfoo-baseline.md))

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

## Acceptance Criteria Checklist

- [x] The document supports a client discovery call or proposal.
- [x] The offer is clear, realistic, and governance-led.
- [x] Deployment options cover Hyper-V, VMware, and Proxmox.
- [x] Software stack (Hermes, LM Studio, n8n, Langfuse) is documented.
- [x] SLA tiers are defined with response/resolution targets.
- [x] Safe pilot approach is outlined with phased rollout.
- [x] Deliberately excluded items are stated explicitly.
- [x] No private pricing or exaggerated claims are included.
