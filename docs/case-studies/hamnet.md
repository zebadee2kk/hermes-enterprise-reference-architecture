# Case Study: Internal Network Operations Co-Pilot

> **Status:** Draft
> **Author:** [Author Name]
> **Date:** [YYYY-MM-DD]
> **Version:** 0.1
> **Redaction Status:** [x] Reviewed  [x] Approved for Publication

---

## ⚠️ Redaction & Public-Safety Guidance

This case study has been reviewed for public-safe publication. All client-identifying information has been replaced with generic placeholders. No proprietary data, PII, or internal system names are present.

> **Rule of thumb:** If you wouldn't paste it on a public blog, redact it.

---

## Executive Summary

A network operations team managing a mid-size private network infrastructure deployed an AI co-pilot to assist with incident triage, runbook execution, and documentation. The governed appliance reduced mean-time-to-resolution by ~50% and improved documentation consistency across shifts.

---

## Problem

### Business Context

- The network operations team handled ~200 incidents per month across routing, switching, and firewall infrastructure.
- Incident triage was manual and relied heavily on senior engineers' tribal knowledge.
- Runbook adherence varied across shifts, leading to inconsistent resolution approaches.
- New engineers required 3–6 months of mentoring before handling incidents independently.

### Technical Context

- Network infrastructure included commodity routers, switches, and firewalls managed via SNMP and SSH.
- An existing monitoring system (Nagios/Zabbix-class) generated alerts but provided no diagnostic assistance.
- No natural-language interface existed for querying network state or runbook procedures.

### Constraints

| Constraint | Detail |
|------------|--------|
| **Timeline** | 12-week engagement |
| **Budget** | Undisclosed |
| **Team size** | 1 engineer, 1 solution architect |
| **Compliance** | Internal security policy, no data egress |
| **Data residency** | On-premises only |

---

## Architecture

### Solution Overview

A managed AI appliance was deployed on the management network, integrating with the monitoring system via API and with network devices via SSH (read-only). The co-pilot provided natural-language incident triage, runbook suggestions, and automated documentation generation — all with approval-gated write access for any configuration changes.

### Key Components

| Component | Role | Technology |
|-----------|------|------------|
| Agent Runtime | Orchestrates triage workflows, suggests runbook steps | Hermes |
| Workflow Engine | Connects monitoring system, triggers alert enrichment | n8n |
| Model Serving | Runs inference for log analysis and summarisation | LM Studio |
| Observability | Traces, audit logging, runbook versioning | Langfuse |

### Deployment Model

- **Environment:** On-premises, management network segment
- **Hypervisor:** Proxmox VE
- **High availability:** Single node (pilot phase)
- **Network:** Management VLAN with read-only SNMP/SSH to network devices

---

## Governance

### Approval Model

| Tier | Actions | Approver |
|------|---------|----------|
| Tier 0 | Read alerts, query device status, suggest runbook steps | Auto-approved |
| Tier 1 | Query monitoring history, correlate events | Pre-approved policy |
| Tier 2 | Generate incident documentation, update runbooks | Named approver (team lead) |
| Tier 3 | Execute configuration changes on devices | Dual approver + change window |

### Security Controls

- All data remained within the management network; no external API calls.
- SSH access to network devices was read-only during the pilot phase.
- All co-pilot actions were logged with full context for audit.
- Configuration changes (Tier 3) required dual approver and a scheduled change window.

---

## Outcomes

### Quantitative Results

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| Mean time to resolution | ~60 min | ~30 min | -50% |
| Runbook adherence | ~60% | ~90% | +50% |
| New engineer ramp-up | 3–6 months | 1–2 months | -50% |
| Incidents documented | ~40% | ~95% | +137% |

### Qualitative Results

- Senior engineers reported reduced interruptions for routine triage questions.
- Shift handoffs improved due to consistent, AI-generated incident summaries.
- The team lead noted improved confidence in runbook accuracy and completeness.

---

## Lessons Learned

### What Worked

1. Starting with read-only triage (Tier 0) allowed the team to validate suggestions before trusting recommendations.
2. Integrating with the existing monitoring API (rather than replacing it) reduced adoption friction.
3. Auto-generated incident documentation was immediately valued by the team.

### What We'd Do Differently

1. Invest in structured runbook formatting earlier — inconsistent runbook quality affected suggestion accuracy.
2. Define "resolution" metrics more precisely (acknowledged vs. fully resolved).
3. Plan for Tier 3 (configuration changes) from the start, even if not enabled in Phase 1.

### Reusability

- The incident-triage co-pilot pattern is transferable to any NOC/SOC environment with API-accessible monitoring.
- The runbook-suggestion engine can be adapted to any domain with documented procedures.
- Client-specific customisations: monitoring API schema, device CLI patterns, runbook format.

---

## Appendix

### Glossary

| Term | Definition |
|------|------------|
| NOC | Network Operations Centre |
| SNMP | Simple Network Management Protocol |
| Runbook | A documented procedure for handling a specific type of incident |
| Change Window | A scheduled period during which configuration changes are permitted |

---

*This is a redacted case study template. Replace bracketed values and adjust metrics for actual engagements. For redaction guidance, see [TEMPLATE.md](./TEMPLATE.md).*
