# Case Study: Infrastructure / Managed Jump-Box

> **Status:** Draft
> **Author:** [Author Name]
> **Date:** [YYYY-MM-DD]
> **Version:** 0.1
> **Redaction Status:** [ ] Not reviewed  [ ] Reviewed  [ ] Approved for Publication

---

## ⚠️ Redaction & Public-Safety Guidance

Before publishing this case study externally, **all** of the following must be completed:

1. **Replace all bracketed placeholders** (`[Client Name]`, `[Industry]`, etc.) with real or intentionally anonymised values.
2. **Remove or anonymise** any proprietary data, internal system names, IP addresses, or credentials.
3. **Verify no PII** (personally identifiable information) is present.
4. **Confirm client approval** if the client is named or identifiable.
5. **Replace metrics** with ranges or percentages if exact figures are sensitive.
6. **Remove internal-only references** (project codenames, internal team names).

> **Rule of thumb:** If you wouldn't paste it on a public blog, redact it.

---

## Executive Summary

[Client Name] is a [industry] organisation with [size] infrastructure managed by a small IT team. They engaged [Our Company] to deploy a managed AI jump-box that provided a secure, governed access point for AI-assisted infrastructure management. The result was a [~X%] reduction in time-to-diagnose for infrastructure issues and a standardised approach to change management, achieved within [timeframe].

---

## Problem

### Business Context

- The IT team managed [N] servers, [N] network devices, and [N] services with limited automation.
- Infrastructure troubleshooting relied on manual log review and SSH sessions to individual systems.
- Change management was ad-hoc, with inconsistent documentation and approval processes.
- The team had no centralised, auditable management point for routine operations.

### Technical Context

- Infrastructure included a mix of [Linux/Windows servers, VMware/Hyper-V hypervisors, network devices].
- Monitoring was in place (e.g., Nagios, Zabbix, Prometheus) but provided no diagnostic assistance.
- No jump-box or bastion host existed for centralised, auditable access.
- Compliance requirements (e.g., SOC 2, ISO 27001) mandated audit trails for all infrastructure changes.

### Constraints

| Constraint | Detail |
|------------|--------|
| **Timeline** | [e.g., 8-week deployment] |
| **Budget** | [Range or "undisclosed"] |
| **Team size** | [e.g., 1 engineer, 1 solution architect] |
| **Compliance** | [e.g., SOC 2, ISO 27001, internal change management policy] |
| **Data residency** | [e.g., On-premises only] |

---

## Architecture

### Solution Overview

A managed AI jump-box was deployed on-premises, acting as a centralised, governed access point for infrastructure management. The appliance integrated with the monitoring system, inventory management (e.g., NetBox), and documentation store. The AI co-pilot assisted with evidence collection, log analysis, configuration comparison, and runbook generation — all with approval-gated write access for any infrastructure changes.

### Key Components

| Component | Role | Technology |
|-----------|------|------------|
| Agent Runtime | Orchestrates diagnostics, enforces approval gates | Hermes |
| Workflow Engine | Connects monitoring, inventory, and ticketing systems | n8n |
| Model Serving | Runs inference for log analysis and summarisation | LM Studio |
| Observability | Traces, audit logging, change documentation | Langfuse |

### Deployment Model

- **Environment:** On-premises, management network segment
- **Hypervisor:** [Hyper-V / VMware / Proxmox / bare metal]
- **High availability:** [Single node / active-passive / clustered]
- **Network:** Management VLAN with controlled SSH/API access to infrastructure

---

## Governance

### Approval Model

| Tier | Actions | Approver |
|------|---------|----------|
| Tier 0 | Read logs, query monitoring, collect evidence | Auto-approved |
| Tier 1 | Compare configuration snapshots, draft runbook steps | Pre-approved policy |
| Tier 2 | Generate change documentation, update runbooks | Named approver (IT lead) |
| Tier 3 | Execute configuration changes, restart services, deploy updates | Dual approver + change window |

### Security Controls

- All access was routed through the jump-box; no direct SSH to infrastructure from user workstations.
- All sessions were logged with full command history and output.
- Configuration changes (Tier 3) required dual approver and a scheduled change window.
- The appliance enforced role-based access control for all management functions.

---

## Outcomes

### Quantitative Results

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| [e.g., Mean time to diagnose] | [e.g., 90 min] | [e.g., 25 min] | [-72%] |
| [e.g., Change documentation rate] | [e.g., 30%] | [e.g., 95%] | [+217%] |
| [e.g., Unplanned changes] | [e.g., 15/month] | [e.g., 3/month] | [-80%] |
| [e.g., Audit preparation time] | [e.g., 20 hrs] | [e.g., 4 hrs] | [-80%] |

### Qualitative Results

- [e.g., "IT team reported faster incident resolution with AI-assisted diagnostics"]
- [e.g., "Change management became consistent and auditable"]
- [e.g., "New team members could safely perform diagnostics with AI guidance"]

---

## Lessons Learned

### What Worked

1. Starting with read-only diagnostics (Tier 0) built confidence in AI suggestions before enabling any write access.
2. Centralising access through the jump-box improved security posture immediately.
3. Auto-generated change documentation was valued by both the IT team and compliance auditors.

### What We'd Do Differently

1. Invest in structured runbook creation earlier — the AI's suggestions were only as good as the documented procedures.
2. Define "change window" policies with the client before enabling Tier 3 actions.
3. Plan for backup/restore procedures for the jump-box itself from day one.

### Reusability

- The managed jump-box pattern is transferable to any organisation with on-premises infrastructure.
- The approval-tier model for infrastructure changes is reusable across environments.
- Client-specific customisations: monitoring API, device CLI patterns, change management policies.

---

## Appendix

### Glossary

| Term | Definition |
|------|------------|
| Jump-Box | A hardened, auditable access point for managing infrastructure |
| Change Window | A scheduled period during which infrastructure changes are permitted |
| Runbook | A documented procedure for handling a specific operational task |
| Bastion Host | A server designed to withstand attacks and provide controlled access to a network |

---

*This is a redacted case study template. Replace bracketed values and adjust metrics for actual engagements. For redaction guidance, see [TEMPLATE.md](./TEMPLATE.md).*
