# Case Study: Support Desk Co-Pilot — Reducing Ticket Resolution Time with AI-Assisted Agents

> **Classification:** Public-Safe Template  
> **Redaction Status:** Placeholder content — no real client data  
> **Last Updated:** {{DATE}}

---

## ⚠️ Public-Safety & Redaction Guidance

Before publishing any derivative of this case study:

- [ ] **Remove or anonymise all customer names, ticket IDs, and internal employee names**
- [ ] **Remove any specific product names or versions** beyond what is publicly disclosed
- [ ] **Remove all internal system URLs, API endpoints, and integration identifiers**
- [ ] **Replace exact SLA/OLA figures** with ranges if they reveal commercial commitments
- [ ] **Remove any reference to security incidents, breach details, or vulnerability information**
- [ ] Confirm the co-prompt does not have access to production customer data in a way that violates privacy obligations
- [ ] Have the final draft reviewed by Support leadership and Legal

---

## Problem Statement

**Client Profile:** {{CLIENT}} — a {{INDUSTRY}} company with a {{NUM_AGENTS}}-person support team handling **_{{VOLUME}}_ tickets/month**

### The Challenge

{{CLIENT}}'s support organisation was struggling to maintain quality while volume grew:

- **Rising first-response time (FRT):** Average FRT had climbed from **{{OLD_FRT}} hours** to **{{NEW_FRT}} hours** over the past year, breaching the **{{SLA_FRT}}** SLA on **{{SLA_BREACH_PCT}}%** of tickets.
- **Knowledge fragmentation:** Troubleshooting knowledge was spread across {{NUM_WIKI_PAGES}} wiki pages, {{NUM_CONFLUENCE_SPACES}} Confluence spaces, and institutional memory in senior agents' heads. New agents took **{{RAMP_MONTHS}} months** to reach independent productivity.
- **Tier-1 overload:** **{{TIER1_DEFLECTABLE_PCT}}%** of Tier-1 tickets were potentially deflectable with better self-service or automated diagnosis, but no system existed to do so.
- **Agent burnout:** Annual agent turnover was **{{TURNOVER_PCT}}%**. Exit interviews cited "repetitive work" and "constant firefighting" as top reasons.

### Constraints

| Constraint | Detail |
|---|---|
| Budget | **${{BUDGET}}** for tooling and implementation |
| SLA | Must maintain ≤ **{{SLA_TARGET}}** hour FRT for P1/P2 tickets |
| Timeline | **{{WEEKS}} weeks** to pilot go-live |
| Staffing | No dedicated engineer for the support tooling — must be maintainable by the support ops team |
| Data privacy | Cannot send customer PII to external AI providers without consent |

---

## Solution Architecture

### High-Level Design

```
                        ┌─────────────────┐
                        │   Agent Desktop  │
                        │   ({{DESKTOP}})  │
                        └────────┬────────┘
                                 │
                    ┌────────────▼────────────┐
                    │   Hermes Co-Pilot Agent  │
                    │                          │
                    │  ┌─────────────────────┐ │
                    │  │  Suggestion Engine   │ │
                    │  │  (Resolutions, KB)   │ │
                    │  └─────────────────────┘ │
                    │  ┌─────────────────────┐ │
                    │  │  Auto-Triage         │ │
                    │  │  (Priority, Route)   │ │
                    │  └─────────────────────┘ │
                    │  ┌─────────────────────┐ │
                    │  │  Draft Response Gen  │ │
                    │  │  (Human-in-the-loop) │ │
                    │  └─────────────────────┘ │
                    └──────────────────────────┘
                                 │
                 ┌───────────────┼───────────────┐
                 │               │               │
          ┌──────▼──────┐ ┌─────▼──────┐ ┌─────▼──────┐
          │  Knowledge  │ │  Ticketing │ │  RAG Store │
          │  Base ({{KB}})│ │  System    │ │  ({{VECTOR}})│
          └─────────────┘ └────────────┘ └────────────┘
```

### Components

1. **Hermes Co-Pilot Agent**  
   Sits alongside the human agent. On each new ticket, it:
   - Classifies priority and suggests routing
   - Retrieves relevant KB articles and past resolutions
   - Drafts a response for the agent to review/edit/send
   - Logs all suggestions for quality analysis

2. **Knowledge Ingestion Pipeline**  
   Periodically ingests and embeds content from {{KB_SYSTEM}} into a vector store ({{VECTOR_DB}}). Content is chunked, tagged with metadata (product, version, last-updated), and re-indexed on a **{{REINDEX_FREQUENCY}}** schedule.

3. **Human-in-the-Loop Guardrail**  
   No response is sent to a customer without explicit agent approval. The agent can accept, edit, or reject suggestions. Rejection reasons are logged for model improvement.

4. **Analytics Dashboard**  
   Tracks suggestion acceptance rate, FRT impact, deflection rate, and agent satisfaction. Accessible to support leadership.

---

## Deployment Details

### Phase 1 — Knowledge Foundation (Weeks 1–{{W1}})

- Ingested {{KB_DOC_COUNT}} KB articles into the vector store
- Defined chunking strategy and metadata schema
- Built retrieval evaluation suite ({{EVAL_QUERIES}} test queries)
- Achieved **{{RETRIEVAL_PRECISION}}%** precision@5 on the eval suite

### Phase 2 — Pilot (Weeks {{W2_START}}–{{W2_END}})

- Deployed co-pilot to {{PILOT_AGENTS}} agents in the {{PILOT_TEAM}} team
- Ran in "suggestion only" mode — no automated actions
- Collected agent feedback via weekly surveys
- Tuned prompt templates based on rejection patterns

### Phase 3 — Production (Weeks {{W3_START}}–{{W3_END}})

- Rolled out to all {{TOTAL_AGENTS}} agents
- Enabled auto-triage for incoming tickets
- Integrated with {{TICKETING_SYSTEM}} for automated tagging and routing
- Established monthly model review cadence

### Data Privacy Controls

| Control | Implementation |
|---|---|
| PII redaction | All ticket text is scanned for PII before reaching the LLM; matches are replaced with `{{REDACTED_TYPE}}` tokens |
| Data residency | LLM inference runs in {{CLOUD_PROVIDER}} region **{{REGION}}** |
| Retention | Suggestion logs retained for **{{LOG_RETENTION_DAYS}}** days; no customer data persisted beyond ticket resolution |
| Access | Co-pilot output visible only to the assigned agent; no cross-ticket data leakage |

---

## Results & Metrics

### Quantitative Outcomes

| Metric | Before | After ({{MONTHS}} months) | Improvement |
|---|---|---|---|
| Avg. first-response time | {{OLD_FRT}} hrs | {{NEW_FRT}} hrs | **{{IMPACT_FRT}}%** reduction |
| Avg. resolution time | {{OLD_RT}} hrs | {{NEW_RT}} hrs | **{{IMPACT_RT}}%** reduction |
| Tier-1 deflection rate | {{OLD_DEFLECT_PCT}}% | {{NEW_DEFLECT_PCT}}% | **{{IMPACT_DEFLECT}}pp** increase |
| Agent ramp time | {{OLD_RAMP}} months | {{NEW_RAMP}} months | **{{IMPACT_RAMP}}%** reduction |
| Suggestion acceptance rate | — | {{ACCEPT_PCT}}% | — |
| Agent turnover (annual) | {{OLD_TURNOVER}}% | {{NEW_TURNOVER}}% | **{{IMPACT_TURNOVER}}pp** reduction |
| CSAT score | {{OLD_CSAT}} | {{NEW_CSAT}} | **{{IMPACT_CSAT}}pp** increase |

### Qualitative Outcomes

- **Agent experience:** "It's like having a senior agent whispering in my ear." — *Support Agent (anonymised)*
- **Knowledge equity:** New agents could resolve issues that previously required months of tribal knowledge.
- **Consistency:** Response quality became more uniform across agents and shifts.

### ROI Summary

```
Implementation cost:           ${{IMPLEMENT_COST}}
Annual agent time saved:       {{HOURS_SAVINGS}} hours (${{COST_SAVINGS}})
Annual total benefit:          ${{TOTAL_ANNUAL_BENEFIT}}
Payback period:                {{PAYBACK_MONTHS}} months
```

---

## Lessons Learned

### What Went Well

1. **Human-in-the-loop from day one.** Starting with "suggestion only" built trust. Agents who saw the co-pilot as an assistant (not a replacement) were far more receptive.
2. **Knowledge hygiene matters.** The co-pilot is only as good as its KB. Investing in KB cleanup *before* ingestion was the highest-ROI activity in the project.
3. **Feedback loops.** Logging rejections and reasons gave us a continuous improvement signal. We shipped prompt improvements weekly.

### What We'd Do Differently

1. **Set expectations with leadership early.** Initial executive expectations were "fully autonomous support." Resetting to "augmented agent" took a difficult conversation.
2. **Invest in KB governance upfront.** Stale KB articles led to stale suggestions. A KB owner/curation process should have been part of Phase 1.
3. **Measure agent satisfaction continuously.** We waited until Phase 3 to survey agents. Earlier feedback would have accelerated adoption.

### Replicable Patterns

- The co-pilot pattern (suggest → review → send) applies to any high-stakes text generation workflow (legal, medical, financial advice).
- The knowledge ingestion + RAG pipeline is reusable for any internal search or documentation challenge.

---

## Appendix

### Related Templates

- [Client Discovery Checklist](../client-discovery-checklist.md)
- [Deployment Checklist - AI Workloads](../deployment-checklist.md#ai)
- [Evaluation Framework - AI Quality](../../docs/evaluation/memory-observability-evals.md#ai-quality)

### Contact

For questions about this case study, contact: **{{OWNER_NAME}}** ({{OWNER_EMAIL}})  
Template version: **{{TEMPLATE_VERSION}}**
