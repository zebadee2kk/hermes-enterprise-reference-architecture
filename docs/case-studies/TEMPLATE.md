# Case Study: [Use Case Title]

> **Status:** Draft | Review Ready | Approved for Publication
> **Author:** [Author Name]
> **Date:** [YYYY-MM-DD]
> **Version:** 0.1
> **Redaction Status:** [ ] Not reviewed  [ ] Reviewed  [ ] Approved

---

## ⚠️ Redaction & Public-Safety Guidance

Before publishing this case study externally, **all** of the following must be completed:

1. **Replace all bracketed placeholders** (`[Client Name]`, `[Industry]`, etc.) with real or intentionally anonymised values.
2. **Remove or anonymise** any proprietary data, internal system names, IP addresses, or credentials.
3. **Verify no PII** (personally identifiable information) is present — names, emails, ticket IDs.
4. **Confirm client approval** if the client is named or identifiable.
5. **Replace metrics** with ranges or percentages if exact figures are sensitive (e.g., "reduced resolution time by ~40%" instead of exact numbers).
6. **Remove internal-only references** (project codenames, internal team names, Slack channels).

> **Rule of thumb:** If you wouldn't paste it on a public blog, redact it.

---

## Executive Summary

> 2–3 sentences summarising the engagement: who, what, outcome.

[Client Name] is a [industry] organisation with [size/scope]. They engaged [Our Company] to [problem statement]. The result was [key outcome], achieved within [timeframe].

---

## Problem

### Business Context

> What was the business situation before the engagement?

- [Describe the operational challenge, pain point, or opportunity]
- [Quantify the impact where possible: time lost, cost incurred, risk exposure]

### Technical Context

> What technical constraints or complexities were present?

- [Existing systems, architecture, or tooling]
- [Integration requirements or limitations]
- [Compliance, security, or data-sovereignty constraints]

### Constraints

| Constraint | Detail |
|------------|--------|
| **Timeline** | [e.g., 8-week pilot, 6-month engagement] |
| **Budget** | [Range or "undisclosed"] |
| **Team size** | [e.g., 2 engineers, 1 PM] |
| **Compliance** | [e.g., GDPR, HIPAA, internal policy] |
| **Data residency** | [e.g., EU-only, on-premises] |

---

## Architecture

### Solution Overview

> High-level description of what was built or deployed.

[Describe the solution: components, deployment model, integration points.]

### Key Components

| Component | Role | Technology |
|-----------|------|------------|
| [e.g., Agent Runtime] | [e.g., Orchestrates tool use and approval gates] | [e.g., Hermes] |
| [e.g., Workflow Engine] | [e.g., Connects internal systems] | [e.g., n8n] |
| [e.g., Model Serving] | [e.g., Runs inference locally] | [e.g., LM Studio] |
| [e.g., Observability] | [e.g., Traces and audit logging] | [e.g., Langfuse] |

### Deployment Model

- **Environment:** [On-premises / private cloud / hybrid]
- **Hypervisor:** [Hyper-V / VMware / Proxmox / bare metal]
- **High availability:** [Single node / active-passive / clustered]
- **Network:** [Isolated segment / VLAN / full integration]

---

## Governance

### Approval Model

> How were AI actions governed?

| Tier | Actions | Approver |
|------|---------|----------|
| Tier 0 | [e.g., Read-only queries] | Auto-approved |
| Tier 1 | [e.g., Internal data access] | Pre-approved policy |
| Tier 2 | [e.g., Write operations] | Named approver |
| Tier 3 | [e.g., External actions] | Dual approver |

### Security Controls

- [e.g., All data remained within the appliance boundary]
- [e.g., No model calls to external APIs without approval]
- [e.g., Audit logs retained for 90 days]
- [e.g., Role-based access control for admin functions]

---

## Outcomes

### Quantitative Results

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| [e.g., Mean time to resolution] | [e.g., 45 min] | [e.g., 12 min] | [-73%] |
| [e.g., Tickets handled per day] | [e.g., 20] | [e.g., 55] | [+175%] |
| [e.g., Escalation rate] | [e.g., 35%] | [e.g., 12%] | [-66%] |

### Qualitative Results

- [e.g., "Support team reported higher confidence in AI-assisted responses"]
- [e.g., "Reduced cognitive load during incident triage"]
- [e.g., "Improved consistency of customer communications"]

---

## Lessons Learned

### What Worked

1. [e.g., Starting with Tier 0–1 actions built trust before expanding scope]
2. [e.g., Weekly feedback sessions with pilot users caught issues early]
3. [e.g., Pre-built workflow templates accelerated onboarding]

### What We'd Do Differently

1. [e.g., Invest in data-quality improvements earlier in the engagement]
2. [e.g., Define success metrics with the client before Phase 1]
3. [e.g., Allocate more time for governance policy design]

### Reusability

> Can this pattern be applied to other clients or use cases?

- [Describe transferable elements: architecture patterns, workflow templates, governance models]
- [Note any client-specific customisations that would need adaptation]

---

## Appendix

### Related Documentation

- [Architecture diagram](link)
- [Deployment blueprint](link)
- [Governance policy](link)
- [Evaluation results](link)

### Glossary

| Term | Definition |
|------|------------|
| [e.g., Approval Gate] | [A checkpoint requiring human authorisation before an action proceeds] |
| [e.g., Tier 2 Action] | [An action that modifies internal systems and requires named approver sign-off] |

---

*This template is part of the Hermes Enterprise Reference Architecture. For redaction guidance, see the header section above. Do not publish without completing the redaction checklist.*
