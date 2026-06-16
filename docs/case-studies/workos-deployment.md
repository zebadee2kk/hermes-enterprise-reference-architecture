# Case Study: Identity & Access Management Automation

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

A mid-size technology company engaged our team to automate identity and access management (IAM) workflows using an agentic AI approach. By deploying a governed AI appliance with approval-gated tool policies, the client reduced provisioning time by ~60% and eliminated manual errors in access reviews.

---

## Problem

### Business Context

- The client's IT team managed user provisioning and access reviews across multiple SaaS tools using spreadsheets and manual ticketing.
- Onboarding a new employee required ~45 minutes of manual work across 6+ systems.
- Quarterly access reviews were error-prone, with a ~15% miss rate on stale permissions.

### Technical Context

- Existing IAM stack included a commercial identity provider (IdP), an HRIS system, and multiple SaaS applications.
- No API-level integration existed between the IdP and the HRIS for automated provisioning.
- Compliance requirements (SOC 2) mandated audit trails for all access changes.

### Constraints

| Constraint | Detail |
|------------|--------|
| **Timeline** | 10-week engagement |
| **Budget** | Undisclosed |
| **Team size** | 2 engineers, 1 solution architect |
| **Compliance** | SOC 2, internal data-handling policy |
| **Data residency** | On-premises deployment required |

---

## Architecture

### Solution Overview

A managed AI appliance was deployed on-premises, integrating with the client's IdP and HRIS via API connectors. The agent handled provisioning, deprovisioning, and access review workflows with human-in-the-loop approval for all write operations.

### Key Components

| Component | Role | Technology |
|-----------|------|------------|
| Agent Runtime | Orchestrates IAM workflows, enforces approval gates | Hermes |
| Workflow Engine | Connects IdP, HRIS, and SaaS tools | n8n |
| Model Serving | Runs inference for classification and summarisation | LM Studio |
| Observability | Traces, audit logging, prompt versioning | Langfuse |

### Deployment Model

- **Environment:** On-premises, isolated network segment
- **Hypervisor:** VMware vSphere
- **High availability:** Single node (pilot phase)
- **Network:** VLAN-isolated with controlled egress to IdP and HRIS APIs

---

## Governance

### Approval Model

| Tier | Actions | Approver |
|------|---------|----------|
| Tier 0 | Read user directory, list entitlements | Auto-approved |
| Tier 1 | Query HRIS for employment status | Pre-approved policy |
| Tier 2 | Provision/deprovision user accounts | Named approver (IT admin) |
| Tier 3 | Modify role definitions, bulk operations | Dual approver |

### Security Controls

- All data remained within the appliance boundary; no external model API calls.
- Every provisioning action was logged in Langfuse with full audit trail.
- Role-based access control restricted admin functions to authorised personnel.
- Approval logs were exportable for SOC 2 audit reviews.

---

## Outcomes

### Quantitative Results

| Metric | Before | After | Change |
|--------|--------|-------|--------|
| New-user provisioning time | ~45 min | ~18 min | -60% |
| Access review miss rate | ~15% | ~2% | -87% |
| Quarterly review effort | 32 person-hours | 8 person-hours | -75% |

### Qualitative Results

- IT team reported higher confidence in access review accuracy.
- New employees reported faster access to required tools on day one.
- Audit preparation time decreased significantly due to automated traceability.

---

## Lessons Learned

### What Worked

1. Starting with read-only (Tier 0–1) workflows built trust before enabling write operations.
2. Weekly feedback sessions with the IT team caught edge cases early (e.g., contractor vs. employee provisioning).
3. Pre-built n8n workflow templates for common IAM patterns accelerated deployment.

### What We'd Do Differently

1. Invest in HRIS data-quality improvements earlier — inconsistent employment-status fields caused initial misclassifications.
2. Define success metrics (provisioning time, miss rate) with the client before Phase 1 kickoff.
3. Allocate more time for governance policy design with the client's compliance team.

### Reusability

- The IAM provisioning workflow pattern is transferable to any organisation using a commercial IdP with API access.
- The approval-tier model (Tier 0–3) is a reusable governance framework for any access-change automation.
- Client-specific customisations: HRIS field mappings, SaaS connector configurations.

---

## Appendix

### Glossary

| Term | Definition |
|------|------------|
| IdP | Identity Provider — manages user authentication and authorisation |
| HRIS | Human Resource Information System — manages employee data |
| Approval Gate | A checkpoint requiring human authorisation before an action proceeds |

---

*This is a redacted case study template. Replace bracketed values and adjust metrics for actual engagements. For redaction guidance, see [TEMPLATE.md](./TEMPLATE.md).*
