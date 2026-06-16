# Case Study: Support Desk Co-Pilot

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

[Client Name] is a [industry] organisation with a support team handling [~N] tickets per month. They engaged [Our Company] to deploy a support desk co-pilot that assisted with ticket triage, response drafting, and knowledge base maintenance. The result was a [~X%] reduction in first-response time and a [~X%] improvement in resolution consistency, achieved within [timeframe].

---

## Problem

### Business Context

- The support team managed [~N] tickets/month with a team of [N] agents across [N] tiers.
- First-response SLA compliance was at [X]%, with customer satisfaction scores at [X]/10.
- New agent onboarding required [N] weeks of training before independent ticket handling.
- Knowledge base articles were outdated, with [X%] last updated over a year ago.

### Technical Context

- Existing stack included a helpdesk/ticketing system (e.g., Zendesk, Freshdesk), a knowledge base, and email integration.
- No AI-assisted triage or response drafting was in place.
- Ticket data was subject to [compliance requirements: e.g., GDPR, internal data policy].

### Constraints

| Constraint | Detail |
|------------|--------|
| **Timeline** | [e.g., 10-week engagement] |
| **Budget** | [Range or "undisclosed"] |
| **Team size** | [e.g., 2 engineers, 1 solution architect] |
| **Compliance** | [e.g., GDPR, SOC 2, internal data-handling policy] |
| **Data residency** | [e.g., On-premises / EU-only cloud] |

---

## Architecture

### Solution Overview

A managed AI appliance was deployed [on-premises/in private cloud], integrating with the client's ticketing system and knowledge base via API. The co-pilot provided ticket summarisation, suggested responses, identified similar resolved tickets, and generated draft knowledge base articles. All customer-facing actions required human approval.

### Key Components

| Component | Role | Technology |
|-----------|------|------------|
| Agent Runtime | Orchestrates triage workflows, drafts responses | Hermes |
| Workflow Engine | Connects ticketing system, triggers auto-enrichment | n8n |
| Model Serving | Runs inference for classification, summarisation, and drafting | LM Studio |
| Observability | Traces, audit logging, response quality tracking | Langfuse |

### Deployment Model

- **Environment:** [On-premises / private cloud / hybrid]
- **Hypervisor:** [Hyper-V / VMware / Proxmox / bare metal]
- **High availability:** [Single node / active-passive / clustered]
- **Network:** [Isolated segment / VLAN / full integration]

---

## Governance

### Approval Model

| Tier | Actions | Approver |
|------|---------|----------|
| Tier 0 | Read tickets, summarise content, identify similar issues | Auto-approved |
| Tier 1 | Query knowledge base, suggest resolution steps | Pre-approved policy |
| Tier 2 | Draft customer responses, generate KB articles | Named approver (team lead) |
| Tier 3 | Send replies to customers, close tickets, modify KB | Dual approver |

### Security Controls

- All customer data remained within the appliance boundary; no external model API calls.
- Automated PII detection flagged tickets containing sensitive data for restricted handling.
- All agent actions were logged with full context for compliance audits.
- Customer-facing responses required human review before sending.

---

## Outcomes

### Quantitative Results

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| [e.g., First-response time] | [e.g., 4 hours] | [e.g., 1.5 hours] | [-62%] |
| [e.g., SLA compliance] | [e.g., 78%] | [e.g., 94%] | [+20%] |
| [e.g., New agent ramp-up] | [e.g., 6 weeks] | [e.g., 2 weeks] | [-67%] |
| [e.g., KB article currency] | [e.g., 40% current] | [e.g., 85% current] | [+112%] |

### Qualitative Results

- [e.g., "Support agents reported higher confidence in response quality"]
- [e.g., "Customers reported faster, more consistent answers"]
- [e.g., "Team leads spent less time on routine triage and more on escalation management"]

---

## Lessons Learned

### What Worked

1. Starting with read-only triage (Tier 0) let the team validate suggestions before trusting automated responses.
2. Integrating similar-ticket lookup reduced duplicate research effort significantly.
3. Auto-generated KB articles from resolved tickets kept the knowledge base current with minimal effort.

### What We'd Do Differently

1. Invest in ticket tagging/label quality early — inconsistent labels affected triage accuracy.
2. Define CSAT and SLA targets with the client before deployment.
3. Plan for multilingual support from the start if the client serves international customers.

### Reusability

- The support co-pilot pattern is transferable to any ticketing system with API access.
- The response-drafting workflow is reusable across industries with domain-specific tuning.
- Client-specific customisations: ticket taxonomy, KB schema, escalation rules.

---

## Appendix

### Glossary

| Term | Definition |
|------|------------|
| SLA | Service Level Agreement — defines expected response and resolution times |
| KB | Knowledge Base — a repository of articles documenting solutions to common issues |
| Triage | The process of categorising and prioritising incoming tickets |
| CSAT | Customer Satisfaction — a metric measuring customer happiness with support |

---

*This is a redacted case study template. Replace bracketed values and adjust metrics for actual engagements. For redaction guidance, see [TEMPLATE.md](./TEMPLATE.md).*
