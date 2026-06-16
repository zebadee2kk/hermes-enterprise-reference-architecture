# Case Study: Client AI Lab Deployment — Accelerating ML Experimentation with Managed Agent Infrastructure

> **Classification:** Public-Safe Template  
> **Redaction Status:** Placeholder content — no real client data  
> **Last Updated:** {{DATE}}

---

## ⚠️ Public-Safety & Redaction Guidance

Before publishing any derivative of this case study:

- [ ] **Remove or anonymise all model names, versions, and training datasets** — replace with generic descriptors (e.g., "a 7B-parameter LLM")
- [ ] **Remove all API keys, endpoint URLs, and service account references**
- [ ] **Replace specific cost figures with ranges** if they could reveal commercial terms
- [ ] **Remove any data governance policies, retention rules, or classification schemes** beyond what is publicly known
- [ ] **Fictionalise any researcher or team member names** unless they've consented to be quoted
- [ ] Confirm no proprietary model weights, fine-tuning data, or evaluation benchmarks are disclosed
- [ ] Have the final draft reviewed by ML/AI governance and Legal teams

---

## Problem Statement

**Client Profile:** {{CLIENT}} — a {{INDUSTRY}} company with a team of **{{NUM_RESEARCHERS}} ML researchers** and **{{NUM_ENGINEERS}} engineers**

### The Challenge

{{CLIENT}} wanted to establish an internal AI research laboratory but faced significant infrastructure and workflow friction:

- **GPU scarcity & contention:** {{NUM_GPUS}} GPUs were shared across {{NUM_TEUSERS}} users via ad hoc reservations (a Slack thread). Utilisation was under **{{OLD_UTIL_PCT}}%**, but researchers still complained about wait times.
- **Reproducibility crisis:** No standardised experiment tracking. Researchers used a mix of {{NUM_TRACKING_TOOLS}} different tools (or none at all). Replicating a result from three months prior was nearly impossible.
- **Data pipeline complexity:** Ingesting, versioning, and serving datasets required custom scripts that broke silently. **_{{PIPELINE_FAILURES_PER_MONTH}} pipeline failures per month_** went undetected for days.
- **Experiment-to-production gap:** Only **{{DEPLOYED_PCT}}%** of promising experiments ever reached a production endpoint. The rest died in notebooks.

### Constraints

| Constraint | Detail |
|---|---|
| Budget | **${{GPU_BUDGET}}/mo** for compute + **${{STORAGE_BUDGET}}/mo** for data |
| Security | All training data must remain within {{CLOUD_PROVIDER}} region **{{REGION}}** |
| Compliance | Data must comply with **{{FRAMEWORK}}** (e.g., GDPR, HIPAA) |
| Skill gap | Researchers strong in ML; limited MLOps / platform experience |
| Timeline | **{{WEEKS}} weeks** to a usable lab environment |

---

## Solution Architecture

### High-Level Design

```
┌─────────────────────────────────────────────────────────┐
│                    AI Lab Environment                     │
│                                                          │
│  ┌──────────┐   ┌──────────────┐   ┌────────────────┐  │
│  │  Data     │──▶│  Experiment  │──▶│  Model         │  │
│  │  Lake     │   │  Runner      │   │  Registry      │  │
│  │ ({{STORE}})│   │  ({{ORCH}})  │   │  ({{REGISTRY}})│  │
│  └──────────┘   └──────────────┘   └────────────────┘  │
│                        │                                  │
│                 ┌──────▼──────┐                          │
│                 │  Hermes     │                          │
│                 │  Agent      │                          │
│                 │  Controller │                          │
│                 └─────────────┘                          │
│                        │                                  │
│               ┌────────▼────────┐                        │
│               │  GPU Cluster    │                        │
│               │  ({{NUM_GPUS}}x  │                        │
│               │   {{GPU_TYPE}}) │                        │
│               └─────────────────┘                        │
└─────────────────────────────────────────────────────────┘
```

### Components

1. **Hermes Agent Controller**  
   The orchestration layer. Accepts experiment specifications (model config, dataset version, hyperparameters), schedules jobs on available GPUs, monitors progress, and registers results.

2. **Data Lake with Versioning**  
   {{STORAGE_BACKEND}} with dataset versioning ({{DATA_VERSIONING_TOOL}}). Every dataset snapshot is immutable and referenced by hash.

3. **Experiment Runner**  
   Containerised training environments ({{CONTAINER_RUNTIME}}) launched on-demand. Each run produces:
   - Trained model artifacts
   - Full hyperparameter and config snapshot
   - Metrics and evaluation results
   - Resource utilisation telemetry

4. **Model Registry**  
   {{REGISTRY_TOOL}} with stage-based lifecycle (staging → canary → production → archived). Every model card includes training data provenance.

5. **GPU Scheduler**  
   Priority-based scheduling with fair-share policies. Preemptible jobs for experimentation; reserved capacity for production training runs.

### Experiment Lifecycle

```
Propose → Schedule → Train → Evaluate → Register → Deploy(?) → Monitor
  ▲                                                       │
  └──────────────────── Retrain loop ─────────────────────┘
```

---

## Deployment Details

### Phase 1 — Platform (Weeks 1–{{W1}})

- Provisioned {{NUM_GPUS}} {{GPU_TYPE}} GPUs via {{CLOUD_PROVIDER}}
- Deployed Hermes agent controller and experiment runner
- Configured data lake with versioning for {{INITIAL_DATASETS}} datasets
- Onboarded **{{PILOT_RESEARCHERS}}** researchers for pilot

### Phase 2 — Workflow Standardisation (Weeks {{W2_START}}–{{W2_END}})

- Defined standard experiment templates ({{NUM_TEMPLATES}} templates covering {{USE_CASES}})
- Built CI/CD pipeline for model promotion (staging → production)
- Implemented automated evaluation against {{BENCHMARK_SUITE}} benchmark suite
- Migrated {{MIGRATED_EXPERIMENTS}} historical experiments into the registry

### Phase 3 — Scale & Optimise (Weeks {{W3_START}}–{{W3_END}})

- Scaled to {{TOTAL_GPUS}} GPUs across {{NUM_NODES}} nodes
- Enabled auto-scaling: GPU fleet scales to zero during off-peak hours
- Achieved **{{FINAL_UTIL_PCT}}%** GPU utilisation
- Deployed **{{PROD_MODELS}}** models to production endpoints

### Governance

- All datasets require a **data card** (source, lineage, consent status, expiry)
- All models require a **model card** (training data, intended use, limitations, fairness metrics)
- Quarterly **model review board** evaluates production models for drift and bias
- **{{RETENTION_POLICY}}** retention on experiment artifacts; models auto-archive after {{ARCHIVE_DAYS}} days without promotion

---

## Results & Metrics

### Quantitative Outcomes

| Metric | Before | After ({{MONTHS}} months) | Improvement |
|---|---|---|---|
| GPU utilisation | {{OLD_UTIL_PCT}}% | {{NEW_UTIL_PCT}}% | **{{IMPACT_UTIL}}pp** increase |
| Experiment reproducibility | ~0% (ad hoc) | **98%** (versioned) | **{{IMPACT_REPRO}}pp** increase |
| Pipeline failures (undetected) | {{OLD_PIPELINE_FAILURES}}/mo | 0 | **100%** detection |
| Experiment → production rate | {{OLD_DEPLOY_PCT}}% | {{NEW_DEPLOY_PCT}}% | **{{IMPACT_DEPLOY}}pp** increase |
| Avg. time-to-first-result | {{OLD_TTFT}} days | {{NEW_TTFT}} hours | **{{IMPACT_TTFT}}%** reduction |
| Researchers onboarded | 0 | {{TOTAL_RESEARCHERS}} | — |

### Qualitative Outcomes

- **Researcher velocity:** "I used to spend 60% of my time on infrastructure. Now it's closer to 10%." — *Lead Researcher (anonymised)*
- **Experiment confidence:** Full reproducibility and data lineage made external audits straightforward.
- **Cost efficiency:** Auto-scaling and right-sizing saved **${{SCALE_SAVINGS}}/mo** compared to static provisioning.

### Cost Summary

```
Monthly infrastructure cost:   ${{MONTHLY_INFRA_COST}}
Monthly personnel (platform):  ${{MONTHLY_PLATFORM_PERSONNEL}}
Monthly total:                 ${{MONTHLY_TOTAL}}
Cost per experiment:           ${{COST_PER_EXPERIMENT}}
Cost per production model:     ${{COST_PER_MODEL}}
```

---

## Lessons Learned

### What Went Well

1. **Start with data versioning.** Once every dataset was versioned and immutable, experiment reproducibility became automatic — not aspirational.
2. **Templates over freedom.** Researchers initially resisted standardised templates, but adoption exceeded **{{TEMPLATE_ADOPTION_PCT}}%** once they saw that templates saved them time.
3. **Agent-based scheduling.** Hermes' scheduling agent was far more flexible than a static job queue — it could handle priority preemption, affinity constraints, and spot-instance interruption gracefully.

### What We'd Do Differently

1. **Invest in monitoring earlier.** GPU-level telemetry (power, temperature, memory pressure) would have caught a subtle memory-leak issue three weeks sooner.
2. **Define model governance from day one.** Retrofitting model cards and review gates mid-project caused friction. Build governance into the platform, not as an afterthought.
3. **Budget for egress.** Training data ingress was free, but serving predictions incurred {{CLOUD_PROVIDER}} egress costs we hadn't modelled. Add this to the business case upfront.

### Replicable Patterns

- The experiment-lifecycle-as-pipeline pattern works for any research-to-production workflow (drug discovery, materials science, financial modelling).
- Hermes as a GPU scheduler replaces expensive proprietary ML platform licenses for teams that need flexibility over polish.

---

## Appendix

### Experiment Template (Minimal)

```yaml
experiment:
  name: "{{EXPERIMENT_NAME}}"
  researcher: "{{RESEARCHER_ID}}"
  model:
    type: "{{MODEL_TYPE}}"
    version: "{{MODEL_VERSION}}"
  dataset:
    name: "{{DATASET_NAME}}"
    version: "{{DATASET_HASH}}"
  compute:
    gpus: {{NUM_GPUS_NEEDED}}
    gpu_type: "{{GPU_TYPE_NEEDED}}"
    max_hours: {{MAX_TRAINING_HOURS}}
  hyperparameters:
    learning_rate: {{LR}}
    batch_size: {{BATCH_SIZE}}
    epochs: {{EPOCHS}}
```

### Related Templates

- [Evaluation Framework - AI Models](../../docs/evaluation/memory-observability-evals.md#ai-models)
- [Security & Governance - Data Handling](../../docs/security-and-governance.md#data-governance)
- [Deployment Checklist - ML Workloads](../deployment-checklist.md#ml)

### Contact

For questions about this case study, contact: **{{OWNER_NAME}}** ({{OWNER_EMAIL}})  
Template version: **{{TEMPLATE_VERSION}}**
