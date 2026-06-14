# Memory, observability and evaluation

## Purpose

This document defines the reference approach for memory, observability, and evaluation in a governed Hermes-style agentic platform.

These three areas must be designed together:

- memory determines what the agent can remember;
- observability determines what the operator can understand;
- evaluation determines whether changes make the system better or worse.

## Memory model

### Memory layers

| Layer | Purpose | Example implementation |
|---|---|---|
| Working/native memory | Compact facts needed to steer current behaviour | Hermes native memory / local context file |
| Durable semantic memory | Long-lived facts, procedures, preferences, failure modes | Mem0 + Qdrant |
| Session search | Retrieve exact historical notes and session content | SQLite FTS / document index |
| Archival summaries | Long-term summaries of closed sessions | Letta or equivalent |
| Temporal graph memory | Relationships and facts that change over time | Graphiti/Zep, only where justified |

### Memory write policy

Before writing memory, classify the content:

| Class | Store? | Notes |
|---|---|---|
| Durable fact | Yes | Long-lived preference, system fact, stable procedure |
| Procedure | Yes | Runbook, repeatable operational rule |
| Failure mode | Yes | Error and remediation likely to recur |
| Task state | Maybe | Store only if needed across sessions |
| Session note | Archive | Prefer session summary rather than durable memory |
| Ephemeral chat | No | Do not pollute memory |
| Secret / token / credential | Never | Redact and block |
| Client-sensitive data | Only in approved client store | Do not store in public or personal memory |

### Memory hygiene

A mature deployment should include:

- deduplication before writes;
- namespace mapping;
- confidence/recency metadata;
- source attribution where possible;
- periodic stale-memory review;
- Markdown export for human review;
- deletion/archival process for obsolete facts.

## Observability model

### What to trace

Each agentic run should capture:

- run ID;
- user/task request;
- workflow name;
- model provider and model name;
- prompt/template version;
- token and cost information;
- tool calls and policy decisions;
- memory reads and writes;
- approval gates;
- workflow automations triggered;
- errors and retries;
- final output and confidence notes.

### Reference tools

| Need | Candidate tool |
|---|---|
| LLM traces | Langfuse |
| Model gateway callbacks | LiteLLM + Langfuse integration |
| Distributed trace schema | OpenTelemetry |
| Logs/metrics where needed | Existing client observability stack |

### Trace context

Trace context should be propagated through:

```text
Agent interface -> LangGraph -> LiteLLM -> model provider
                -> Tool Proxy -> external tools
                -> n8n -> workflow steps
                -> memory layer
```

The trace should show not only the model call, but also why a tool was allowed or blocked.

## Evaluation model

### Evaluation types

| Eval type | Purpose |
|---|---|
| Smoke eval | Confirms a workflow still runs |
| Regression eval | Confirms output quality has not degraded |
| Safety eval | Tests prompt injection, secret leakage, and unsafe tool use |
| Cost eval | Compares model cost/performance |
| Memory eval | Checks recall accuracy and stale-memory handling |
| Tool eval | Confirms approval and policy behaviour |

### Reference tool

Promptfoo or an equivalent eval runner can be used for:

- prompt regression tests;
- model comparisons;
- red-team checks;
- CI-style gates before changing workflows, prompts, or models.

### Minimum eval suite

For a first governed deployment, create tests for:

1. read-only GitHub/repository analysis;
2. memory recall and deduplication;
3. refusal to expose or store secrets;
4. approval requirement before mutation;
5. content generation with brand/style constraints;
6. client support scenario with safe escalation;
7. cost/model fallback behaviour.

## Operational cadence

| Cadence | Activity |
|---|---|
| Daily | Review failed runs and high-cost traces |
| Weekly | Memory hygiene and eval review |
| Monthly | Governance, tool access, and prompt review |
| Per release | Run smoke/regression/safety evals |

## Design warning

Do not add more tools, memory engines, or autonomous agents to compensate for poor observability. First make the existing platform understandable, traceable, and testable.
