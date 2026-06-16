# Case Study: Client Appliance Deployment — Content Operations

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

[Client Name] is a [industry] organisation with [size/scope]. They engaged [Our Company] to deploy a managed AI appliance for content operations, enabling their marketing team to research, draft, and publish content with AI assistance under a governed approval workflow. The result was a [~X%] reduction in content production time and consistent brand voice across all channels, achieved within [timeframe].

---

## Problem

### Business Context

- The marketing team produced [~N] articles, social posts, and newsletters per month with a team of [N] people.
- Content research and drafting consumed ~[X]% of the team's time, leaving limited capacity for strategy and review.
- Brand voice consistency varied across authors and channels.
- Publishing workflows were manual, with no standardised approval process.

### Technical Context

- Existing stack included a CMS (e.g., WordPress), a GitHub-based website repo, and a social media scheduling tool.
- No automation existed between content creation and publishing pipelines.
- Brand guidelines existed as static documents but were not enforced programmatically.

### Constraints

| Constraint | Detail |
|------------|--------|
| **Timeline** | [e.g., 8-week pilot, 6-month engagement] |
| **Budget** | [Range or "undisclosed"] |
| **Team size** | [e.g., 2 engineers, 1 PM] |
| **Compliance** | [e.g., Brand guidelines, data-handling policy] |
| **Data residency** | [e.g., On-premises / private cloud] |

---

## Architecture

### Solution Overview

A managed AI appliance was deployed [on-premises/in private cloud], integrating with the client's CMS, GitHub repo, and brand guidelines documentation. The agent assisted with topic research, draft generation, metadata preparation, and content calendar management. All publish actions required human approval through a defined workflow.

### Key Components

| Component | Role | Technology |
|-----------|------|------------|
| Agent Runtime | Orchestrates content workflows, enforces approval gates | Hermes |
| Workflow Engine | Connects CMS, GitHub, and approval queues | n8n |
| Model Serving | Runs inference for research, drafting, and summarisation | LM Studio |
| Observability | Traces, audit logging, content versioning | Langfuse |

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
| Tier 0 | Read brand guidelines, research topics | Auto-approved |
| Tier 1 | Draft articles, prepare metadata | Pre-approved policy |
| Tier 2 | Create draft CMS entries | Named approver (content lead) |
| Tier 3 | Publish to live site, send newsletters | Dual approver |

### Security Controls

- All content drafts remained within the appliance boundary until approved.
- No external API calls for model inference — all processing was local.
- Role-based access control restricted publishing functions to authorised personnel.
- Full audit trail of all content changes and approval decisions.

---

## Outcomes

### Quantitative Results

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| [e.g., Content production time per article] | [e.g., 4 hours] | [e.g., 1.5 hours] | [-62%] |
| [e.g., Monthly content output] | [e.g., 20 articles] | [e.g., 45 articles] | [+125%] |
| [e.g., Time to publish (draft → live)] | [e.g., 3 days] | [e.g., 1 day] | [-67%] |

### Qualitative Results

- [e.g., "Marketing team reported more time for strategic planning"]
- [e.g., "Brand voice consistency improved across all channels"]
- [e.g., "Approval workflow reduced publishing errors"]

---

## Lessons Learned

### What Worked

1. Starting with research and drafting (Tier 0–1) built trust before enabling CMS write access.
2. Integrating brand guidelines as structured prompts improved output consistency.
3. Weekly content review sessions with the marketing team refined prompt templates.

### What We'd Do Differently

1. Invest in structured brand guideline formatting earlier for better prompt adherence.
2. Define content quality metrics (readability scores, brand voice consistency) before Phase 1.
3. Plan for multi-channel publishing (social, email, blog) from the start.

### Reusability

- The content operations workflow pattern is transferable to any marketing team with a CMS.
- The approval-tier model for publishing is reusable across content types.
- Client-specific customisations: CMS API schema, brand guideline structure, social media connectors.

---

## Appendix

### Glossary

| Term | Definition |
|------|------------|
| CMS | Content Management System — manages website content and publishing |
| Approval Gate | A checkpoint requiring human authorisation before content goes live |
| Brand Voice | The consistent tone, style, and personality used in all communications |

---

*This is a redacted case study template. Replace bracketed values and adjust metrics for actual engagements. For redaction guidance, see [TEMPLATE.md](./TEMPLATE.md).*
