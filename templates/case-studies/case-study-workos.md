# Case Study: Enterprise Identity & Access Automation with Agentic Workflows

> **Classification:** Public-Safe Template  
> **Redaction Status:** Placeholder content — no real client data  
> **Last Updated:** {{DATE}}

---

## ⚠️ Public-Safety & Redaction Guidance

Before publishing any derivative of this case study:

- [ ] Replace all placeholder names ({{CLIENT}}, {{INDUSTRY}}, etc.) with anonymised descriptors
- [ ] Remove or generalise any internal IP addresses, hostnames, or URLs
- [ ] Confirm no proprietary client data appears in metrics or quotes
- [ ] Remove any reference to specific security configurations or credentials
- ] Obtain written approval from the referenced client (if real) before publication
- ] Have the final draft reviewed by Legal and Marketing

---

## Problem Statement

**Client Profile:** {{CLIENT}} — a mid-market {{INDUSTRY}} company with {{EMPLOYEE_COUNT}} employees

### The Challenge

{{CLIENT}} faced critical inefficiencies in their identity and access management posture:

- **Onboarding bottlenecks:** New employees waited an average of **{{DAYS}} days** for full system access, with provisioning handled manually across **_{{NUM_SYSTEMS}}_** discrete platforms.
- **Offboarding risk:** Deprovisioning was inconsistent. Audit logs showed **{{PERCENT}}%** of former employees retained at least one active account **30+ days** after departure.
- **Compliance pressure:** Upcoming **{{FRAMEWORK}}** audit required demonstrable access controls and a complete access review trail.
- **Helpdesk overload:** {{TICKETS_PER_MONTH}} tickets/month were access-related, consuming **_{{PERCENT_HOURS}}%_** of IT support capacity.

### Constraints

| Constraint | Detail |
|---|---|
| Budget | Cap of **${{BUDGET}}** for tooling and implementation |
| Timeline | **{{WEEKS}} weeks** to first production rollout |
| Staffing | {{NUM_IT_STAFF}} IT generalists — no dedicated identity team |
| Compliance | Must satisfy **{{FRAMEWORK}}** controls within 6 months |
| Integration | Must work with {{IDP_PROVIDER}} as the identity provider of record |

---

## Solution Architecture

### High-Level Design

```
┌─────────────┐     ┌──────────────┐     ┌─────────────────┐
│  IdP ({{IDP}}) │────▶│  Hermes Agent │────▶│  Target Systems  │
│             │     │  Workflow     │     │  ({{SYSTEMS}})   │
└─────────────┘     └──────────────┘     └─────────────────┘
                           │
                    ┌──────▼──────┐
                    │  Approval   │
                    │  Gate       │
                    │  ({{TOOL}})  │
                    └─────────────┘
```

### Components

1. **Agentic Provisioning Workflow**  
   An Hermes-orchestrated workflow triggered by HR system events (hire, role change, termination). The agent evaluates the employee's role against an access policy matrix, provisions entitlements across target systems via API, and routes any exceptions through an approval gate.

2. **Access Policy Matrix**  
   A version-controlled document mapping RBAC roles to system entitlements. Managed in a Git repository with PR-based change control.

3. **Approval Gate**  
   Out-of-band approval step for non-standard access requests. Integrated with {{APPROVAL_TOOL}} (e.g., Slack, PagerDuty, ServiceNow).

4. **Audit & Reporting Layer**  
   All workflow executions are logged to an immutable audit store. Quarterly access reviews are generated automatically.

### Technology Stack

| Layer | Technology |
|---|---|
| IdP | {{IDP_PROVIDER}} |
| HRIS Trigger | {{HRIS_SYSTEM}} |
| Agent Platform | Hermes ({{VERSION}}) |
| Target Systems | {{SYSTEM_1}}, {{SYSTEM_2}}, {{SYSTEM_3}} |
| Approval Tool | {{APPROVAL_TOOL}} |
| Audit Store | {{AUDIT_STORE}} |

---

## Deployment Details

### Phase 1 — Foundation (Weeks 1–{{W1}})

- Deployed Hermes agent runtime in {{CLOUD_PROVIDER}} ({{REGION}})
- Connected HRIS webhook as the primary trigger source
- Built initial access policy matrix for {{NUM_ROLES}} roles
- Configured approval gate with {{APPROVAL_TOOL}} integration

### Phase 2 — Pilot (Weeks {{W2_START}}–{{W2_END}})

- Onboarded {{PILOT_DEPT}} department ({{PILOT_USERS}} users)
- Ran parallel with existing manual process for validation
- Tuned policy matrix based on pilot feedback ({{NUM_ADJUSTMENTS}} adjustments)

### Phase 3 — Production Rollout (Weeks {{W3_START}}–{{W3_END}})

- Expanded to all {{NUM_DEPARTMENTS}} departments ({{TOTAL_USERS}} users)
- Deprecated manual provisioning runbooks
- Enabled automated offboarding on termination events
- Transferred runbook ownership to operations team

### Governance

- All policy changes require PR review by **≥2** team members
- Monthly access certification by department managers
- Quarterly penetration testing of the agent's API surface
- {{RETENTION_POLICY}} retention on all audit logs

---

## Results & Metrics

### Quantitative Outcomes

| Metric | Before | After ({{MONTHS}} months) | Improvement |
|---|---|---|---|
| Avg. onboarding time | {{OLD_ONBOARD_DAYS}} days | {{NEW_ONBOARD_DAYS}} hours | **{{IMPACT_ONBOARD}}%** reduction |
| Offboarding completion (24h) | {{OLD_PCT}}% | {{NEW_PCT}}% | **{{IMPACT_OFFBOARD}}pp** increase |
| Access-related tickets/month | {{OLD_TICKETS}} | {{NEW_TICKETS}} | **{{IMPACT_TICKETS}}%** reduction |
| Compliance findings (access) | {{OLD_FINDINGS}} | {{NEW_FINDINGS}} | **{{IMPACT_COMPLIANCE}}%** reduction |
| Staff time on access mgmt | {{OLD_HOURS}} hrs/wk | {{NEW_HOURS}} hrs/wk | **{{IMPACT_HOURS}}%** reduction |

### Qualitative Outcomes

- **Employee experience:** New hires reported satisfaction scores of **{{SAT_SCORE}}/5** on Day 1 readiness
- **Audit readiness:** Zero access-related findings in the subsequent **{{FRAMEWORK}}** audit
- **Team morale:** IT staff redirected from ticket-churn to strategic projects

### ROI Summary

```
Implementation cost:        ${{IMPLEMENT_MONTHS * MONTHLY_COST}}
Annual operational savings:  ${{ANNUAL_SAVINGS}}
Payback period:             {{PAYBACK_MONTHS}} months
3-year NPV:                 ${{NPV}}
```

---

## Lessons Learned

### What Went Well

1. **Policy-first approach.** Defining the access matrix *before* writing workflows forced productive conversations with HR and department heads early.
2. **Parallel-run validation.** Running the agent alongside manual processes for two weeks surfaced edge cases without risking user experience.
3. **Immutable audit logs.** Having a complete execution trail from day one made the compliance audit straightforward.

### What We'd Do Differently

1. **Start with offboarding.** It's lower-risk than provisioning and delivers immediate security value. We'd reverse the phase order.
2. **Invest in self-service sooner.** End users wanted a "request access" portal from day one. Budgeting for that upfront would have reduced ticket volume faster.
3. **Over-communicate policy changes.** Several mid-cycle matrix updates confused end users. A simple changelog broadcast would have helped.

### Replicable Patterns

- The HRIS-triggered agent pattern is applicable to any lifecycle event (transfers, parental leave, contractor conversion).
- The approval-gate pattern maps cleanly to any governance-heavy workflow (e.g., data access requests, infrastructure changes).

---

## Appendix

### Related Templates

- [Client Discovery Checklist](../client-discovery-checklist.md)
- [Deployment Checklist](../deployment-checklist.md)
- [Architecture Decision Records](../architecture/{{ADR_TEMPLATE}})

### Contact

For questions about this case study, contact: **{{OWNER_NAME}}** ({{OWNER_EMAIL}})  
Template version: **{{TEMPLATE_VERSION}}**
