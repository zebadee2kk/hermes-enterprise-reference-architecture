# Langfuse and LiteLLM observability blueprint

## Purpose

This blueprint describes how to instrument a governed Hermes-style platform so that model calls, tool calls, workflow steps, memory interactions, and cost are observable end-to-end. It uses **LiteLLM** as the model gateway and **Langfuse** as the LLM tracing platform, with **OpenTelemetry** context conventions for correlation across components.

The goal is to make it possible to:

- debug a single agent run and see every model call, tool decision, and memory write;
- review monthly cost, token usage, and error rates across clients;
- alert on anomalous cost, latency spikes, or trace-volume drops;
- evaluate agent behaviour with trace-driven test sets.

## Architecture overview

```
Agent / LangGraph
       |
       v
  LiteLLM gateway  <--- Langfuse callback plugin
       |
       v
  Model provider / local inference
       |
       v
  Langfuse traces (spans, generations, scores)

Tool Proxy / n8n workflows
       |
       v
  Langfuse traces (tool-call spans, workflow spans)
       |
       v
  OpenTelemetry trace context propagation

Langfuse UI / API
       |
       v
  Dashboards, alerts, prompt management, evals
```

## Prerequisites

| Component | Minimum version | Purpose |
|---|---|---|
| LiteLLM | 1.x+ | Model gateway with callback support |
| Langfuse | 2.x (self-hosted or cloud) | Trace and prompt platform |
| Docker Compose | 2.x+ | Langfuse deployment |
| OpenTelemetry SDK | 1.x+ (per-language) | Trace context propagation |
| Hermes / agent runtime | current | Agent interface |

## 1. Langfuse deployment (docker compose)

### Minimal self-hosted deployment

```yaml
# docker-compose.langfuse.yml
version: "3.9"

services:
  langfuse-worker:
    image: langfuse/langfuse-worker:latest
    restart: always
    environment:
      DATABASE_URL: postgresql://langfuse:langfuse@postgres:5432/langfuse
      NEXTAUTH_SECRET: ${NEXTAUTH_SECRET}
      NEXTAUTH_URL: ${NEXTAUTH_URL:-http://localhost:3000}
      SALT: ${SALT}
      ENCRYPTION_KEY: ${ENCRYPTION_KEY:-0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef}
      TELEMETRY_ENABLED: ${TELEMETRY_ENABLED:-true}
      LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES: ${LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES:-false}
    depends_on:
      postgres:
        condition: service_healthy

  langfuse-web:
    image: langfuse/langfuse-web:latest
    restart: always
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgresql://langfuse:langfuse@postgres:5432/langfuse
      NEXTAUTH_SECRET: ${NEXTAUTH_SECRET}
      NEXTAUTH_URL: ${NEXTAUTH_URL:-http://localhost:3000}
      SALT: ${SALT}
      ENCRYPTION_KEY: ${ENCRYPTION_KEY:-0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef}
      TELEMETRY_ENABLED: ${TELEMETRY_ENABLED:-true}
      LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES: ${LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES:-false}
    depends_on:
      postgres:
        condition: service_healthy

  postgres:
    image: postgres:15
    restart: always
    environment:
      POSTGRES_USER: langfuse
      POSTGRES_PASSWORD: langfuse
      POSTGRES_DB: langfuse
    volumes:
      - langfuse_pg_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U langfuse"]
      interval: 5s
      timeout: 5s
      retries: 5

volumes:
  langfuse_pg_data:
```

### Required secrets

Generate secrets before first run. Do not commit these values.

```bash
# .env (do not commit)
NEXTAUTH_SECRET=$(openssl rand -hex 32)
SALT=$(openssl rand -hex 32)
ENCRYPTION_KEY=$(openssl rand -hex 64)  # 32 bytes hex
NEXTAUTH_URL=https://langfuse.internal.example.com
```

### Production hardening

- Place behind a reverse proxy (Caddy, Nginx, or Traefik) with TLS termination.
- Run PostgreSQL on managed infrastructure or a dedicated volume with daily backups.
- Enable `LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES=false` unless a specific feature is needed.
- Restrict network access to Langfuse ports to trusted internal ranges only.
- Replace the default `ENCRYPTION_KEY` with a securely generated 64-char hex string.
- Set up retention policies in Langfuse project settings to avoid unbounded storage growth.

## 2. LiteLLM callback configuration

### config.yaml

```yaml
# litellm.config.yaml
model_list:
  - model_name: primary-model
    litellm_params:
      model: openai/gpt-4o
      api_base: https://api.openai.com/v1
      api_key: os.environ/OPENAI_API_KEY
  - model_name: fallback-model
    litellm_params:
      model: anthropic/claude-sonnet-4-20250514
      api_key: os.environ/ANTHROPIC_API_KEY
  - model_name: local-model
    litellm_params:
      model: ollama/llama3
      api_base: http://ollama:11434

litellm_settings:
  drop_params: true
  num_retries: 2
  request_timeout: 120
  fallbacks:
    - primary-model:
        - fallback-model

callbacks:
  - langfuse:
      public_key: os.environ/LANGFUSE_PUBLIC_KEY
      secret_key: os.environ/LANGFUSE_SECRET_KEY
      host: os.environ/LANGFUSE_HOST
```

### Callback behaviour

When the Langfuse callback is configured, LiteLLM automatically creates:

- A **Trace** per request (identified by a trace ID).
- A **Generation** span for each model call (input, output, model name, token usage, latency, cost).
- **Nested spans** for retries and fallbacks.
- **Tags** and **metadata** from the request context.

### Trace ID propagation

For a single agent run, generate a trace ID at the agent entry point and pass it through:

```python
import uuid
from langfuse import Langfuse

trace_id = str(uuid.uuid4())
langfuse = Langfuse()

trace = langfuse.trace(
    id=trace_id,
    name="agent-run",
    user_id="operator-id",
    tags=["production", "client-tag"],
    metadata={"deployment": "client-appliance-01"}
)

# Pass trace_id to LiteLLM via metadata
response = litellm.completion(
    model="primary-model",
    messages=[{"role": "user", "content": user_input}],
    metadata={
        "trace_id": trace_id,
        "langfuse_session_id": session_id,
        "langfuse_tags": ["agent-step-1"]
    }
)
```

## 3. Trace context conventions

### OpenTelemetry trace propagation

Use W3C Trace Context (`traceparent` / `tracestate`) headers to correlate spans across components:

```
Agent  -->  LiteLLM  -->  Model Provider
  |            |                |
  v            v                v
Langfuse trace (trace_id spans all components)
```

### Recommended span hierarchy

```
trace: agent-run-{uuid}
  span: langgraph-supervisor
    span: agent-step-1
      span: llm-generation (via LiteLLM callback)
      span: tool-call (Tool Proxy)
        span: tool-execution
      span: memory-read
      span: memory-write
    span: agent-step-2
      span: llm-generation (via LiteLLM callback)
      span: n8n-workflow-trigger
  span: post-processing
    span: evaluation-score
```

### Standard trace fields

| Field | Description | Example |
|---|---|---|
| `trace_id` | Unique run identifier | `uuid-v4` |
| `session_id` | Conversation or session identifier | `session-2025-06-01-abc` |
| `user_id` | Operator or client identifier | `operator-jane` |
| `deployment` | Deployment tag | `client-appliance-01` |
| `model` | Model used | `gpt-4o` |
| `usage.prompt_tokens` | Input token count | `1523` |
| `usage.completion_tokens` | Output token count | `487` |
| `usage.total_tokens` | Total token count | `2010` |
| `cost` | Estimated cost in USD | `0.012` |
| `latency_ms` | End-to-end latency | `2340` |
| `tags` | Free-form labels | `["production", "client-tag"]` |

## 4. Trace IDs across components

### Agent to LangGraph

Generate the trace ID at the agent entry point. Pass it as `configurable` in LangGraph's `RunnableConfig`:

```python
config = RunnableConfig(
    configurable={"trace_id": trace_id, "session_id": session_id},
    tags=["agent", "production"],
    metadata={"deployment": "client-appliance-01"}
)
```

### Agent to Tool Proxy

Include the trace ID in the Tool Proxy request context so tool calls appear as child spans:

```python
tool_result = tool_proxy.call(
    tool_name="read_ticket",
    params={"ticket_id": "12345"},
    context={"trace_id": trace_id, "session_id": session_id}
)
```

### Agent to n8n

Pass the trace ID as a header or query parameter to n8n webhook triggers:

```python
requests.post(
    "https://n8n.internal.example.com/webhook/memory-hygiene",
    json={"action": "compact", "session_id": session_id},
    headers={"X-Trace-ID": trace_id}
)
```

n8n workflows should propagate this ID into any downstream calls (HTTP nodes, model calls via the OpenAI node, etc.).

### Agent to memory layer

Tag memory reads and writes with the trace ID so that memory interactions are visible in the trace timeline:

```python
memory.search(
    query="previous decisions about client X",
    filters={"session_id": session_id},
    trace_id=trace_id
)
```

## 5. Cost and token reporting

### LiteLLM cost tracking

LiteLLM automatically calculates cost per model call when pricing is configured:

```yaml
# In litellm.config.yaml
model_list:
  - model_name: primary-model
    litellm_params:
      model: openai/gpt-4o
      api_key: os.environ/OPENAI_API_KEY
    model_info:
      cost_per_token: 0.00001  # per-token cost
```

### Langfuse cost dashboards

In Langfuse, use the **Dashboard** tab to create:

- **Token usage by model** — bar chart, grouped by model name.
- **Cost by day** — time series, filtered by deployment tag.
- **Cost by client** — bar chart, grouped by `user_id` or custom tag.
- **Latency distribution** — histogram of generation latency.

### Budget alerts

Set budget limits in LiteLLM to prevent runaway costs:

```yaml
litellm_settings:
  max_budget: 100.0       # USD per month
  budget_duration: "30d"
  soft_budget: 80.0       # warn at 80%
```

When the soft budget is exceeded, emit a Langfuse score or tag so alerts can trigger.

## 6. Redaction expectations

Never log or trace raw secrets, credentials, or client-identifiable data. Apply redaction at the gateway level:

### What to redact

| Data type | Action |
|---|---|
| API keys | Never log; pass via environment variables only |
| Credentials / tokens | Redact before trace emission |
| Client PII | Redact or hash before storage |
| Full prompt content | Optional: store only prompt hash for sensitive deployments |
| Tool output with secrets | Redact specific fields (passwords, tokens, keys) |

### LiteLLM redaction

Use LiteLLM's `turn_off_message_logging` for sensitive deployments:

```yaml
litellm_settings:
  turn_off_message_logging: true  # disables raw message logging to callbacks
```

When message logging is off, token counts and metadata are still captured, but prompt/response content is not sent to Langfuse.

### Langfuse prompt management

For prompt versioning without exposing sensitive content:

- Store prompt templates (not filled prompts) in Langfuse Prompt Management.
- Use variables (`{client_name}`, `{ticket_id}`) rather than hardcoded values.
- Set prompt access controls to restrict who can view prompt content.

## 7. Trace-driven evaluation patterns

### Offline evaluation with Promptfoo

Use trace data from Langfuse to build evaluation datasets:

1. Export traces from Langfuse (filtered by tag, date range, or score).
2. Extract input/output pairs as test cases.
3. Run regression tests with Promptfoo:

```yaml
# promptfooconfig.yaml
prompts:
  - file://prompts/agent-prompt.md
providers:
  - openai:gpt-4o
  - openai:gpt-4o-mini
tests:
  - file://evals/from-langfuse-traces.yaml
```

### Online evaluation with Langfuse scores

Attach evaluation scores to traces for continuous monitoring:

```python
# After an agent run, score the output
langfuse.score(
    trace_id=trace_id,
    name="accuracy",
    value=0.92,
    comment="Fact-checked against source documents"
)

langfuse.score(
    trace_id=trace_id,
    name="safety",
    value=1.0,
    comment="No policy violations detected"
)
```

### Evaluation metrics to track

| Metric | Description | Target |
|---|---|---|
| `accuracy` | Factual correctness of agent output | > 0.90 |
| `safety` | No policy violations | 1.0 |
| `latency_p95` | 95th percentile response time | < 5000ms |
| `cost_per_run` | Average cost per agent run | < $0.05 |
| `tool_success_rate` | Percentage of successful tool calls | > 0.95 |
| `fallback_rate` | Percentage of calls using fallback model | < 0.10 |

## 8. Alerting

### Recommended alerts

Configure alerts in Langfuse (or via webhook to PagerDuty/Slack/email):

| Alert | Condition | Severity |
|---|---|---|
| Cost spike | Daily cost > 2x 7-day average | Warning |
| Error rate | Generation error rate > 5% over 15 min | Critical |
| Latency spike | P95 latency > 10s over 15 min | Warning |
| Trace volume drop | Trace count < 50% of expected over 1 hour | Critical |
| Budget threshold | Monthly cost > 80% of budget | Warning |
| Budget exceeded | Monthly cost > 100% of budget | Critical |
| Fallback spike | Fallback model usage > 20% over 15 min | Warning |

### Langfuse webhook configuration

```bash
# In Langfuse project settings, configure webhook:
# URL: https://hooks.slack.com/services/YOUR/WEBHOOK/URL
# Events: trace.create, score.create, generation.create
```

### n8n alerting workflow

For deployments using n8n, create a scheduled workflow that queries the Langfuse API and sends alerts:

```
Schedule trigger (every 15 min)
  -> HTTP request (Langfuse API: /api/public/observations)
  -> Filter (error rate > 5% OR cost anomaly)
  -> Slack / email notification
```

## 9. Minimal dashboards

### Debug dashboard (per-trace view)

When investigating a single agent run, the trace view should show:

1. **Trace timeline** — all spans in order with latency.
2. **Model calls** — input/output, token usage, cost per call.
3. **Tool calls** — tool name, parameters, result, success/failure.
4. **Memory interactions** — reads and writes with relevance scores.
5. **Scores** — evaluation scores attached to the trace.
6. **Metadata** — deployment, session, operator, tags.

### Monthly review dashboard

For governance and cost review:

1. **Total cost by deployment** — bar chart.
2. **Token usage trend** — daily time series.
3. **Model distribution** — pie chart of model usage.
4. **Error rate trend** — daily time series.
5. **Top sessions by cost** — table.
6. **Evaluation score trends** — accuracy, safety over time.

## 10. Validation checklist

Before considering observability production-ready:

- [ ] Langfuse deployed and accessible via HTTPS.
- [ ] LiteLLM callback configured and traces appearing in Langfuse.
- [ ] Trace ID propagation working across agent, Tool Proxy, and n8n.
- [ ] Cost tracking enabled with correct per-model pricing.
- [ ] Redaction configured for sensitive fields.
- [ ] Retention policies set in Langfuse project settings.
- [ ] Budget limits configured in LiteLLM.
- [ ] Alerts configured for cost, latency, and error rate.
- [ ] At least one evaluation metric being scored per run.
- [ ] Monthly review dashboard created.
- [ ] Backup process for Langfuse PostgreSQL database tested.

## 11. Risks and trade-offs

| Risk | Mitigation |
|---|---|
| Langfuse becomes a single point of observability | Export traces to a secondary store periodically |
| Trace volume grows unbounded | Set retention policies; use sampling for high-volume deployments |
| Callback latency adds overhead to model calls | Langfuse callbacks are async by default; monitor callback latency |
| Sensitive data in traces | Enable message logging redaction; use prompt hashing |
| Cost of self-hosted Langfuse | Start with Langfuse Cloud free tier; migrate to self-hosted at scale |
| LiteLLM callback failures silently drop traces | Monitor callback error logs; set up alerting on callback failures |

## References

- [LiteLLM callbacks documentation](https://docs.litellm.ai/docs/observability/callbacks)
- [Langfuse documentation](https://langfuse.com/docs)
- [OpenTelemetry trace context (W3C)](https://www.w3.org/TR/trace-context/)
- [Promptfoo documentation](https://www.promptfoo.dev/docs/)
