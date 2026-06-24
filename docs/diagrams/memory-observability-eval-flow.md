# Memory, observability, and evaluation flow

## Purpose

This diagram shows how memory reads/writes, observability traces, and evaluation signals flow through the platform.

## Memory, observability, and evaluation diagram

```mermaid
flowchart TB
    subgraph Agent["Agent / workflow"]
        A["Agent run<br/>or workflow step"]
    end

    subgraph MemoryFlow["Memory flow"]
        B["Memory policy<br/>classifier"]
        C{"Content<br/>class?"}
        D["Durable memory<br/>Mem0 + Qdrant"]
        E["Session archive<br/>summary only"]
        F["Redact /<br/>do not store"]
    end

    subgraph TraceFlow["Observability flow"]
        G["Langfuse trace<br/>span + cost"]
        H["OpenTelemetry<br/>context propagation"]
        I["Log / metric<br/>export"]
    end

    subgraph EvalFlow["Evaluation flow"]
        J["Promptfoo<br/>eval runner"]
        K["Regression<br/>gate"]
        L["Monthly<br/>quality review"]
    end

    A --> B
    B --> C
    C -->|"Durable fact / procedure"| D
    C -->|"Session note"| E
    C -->|"Secret / ephemeral"| F
    A --> G
    A --> H
    A --> I
    G --> J
    J --> K
    K --> L

    style Agent fill:#e3f2fd,stroke:#1976D2
    style MemoryFlow fill:#f3e5f5,stroke:#7B1FA2
    style TraceFlow fill:#e8f5e9,stroke:#388E3C
    style EvalFlow fill:#fff3e0,stroke:#F57C00
```

## Trace context propagation

```mermaid
sequenceDiagram
    participant Op as Operator
    participant Agent as Agent interface
    participant LG as LangGraph
    participant TP as Tool Proxy
    participant LM as LiteLLM
    participant MP as Model provider
    participant LF as Langfuse

    Op->>Agent: Submit task
    Agent->>LG: Run workflow
    LG->>LM: Model call (trace_id)
    LM->>MP: Inference request
    MP-->>LM: Response
    LM->>LF: Trace span + cost
    LG->>TP: Tool call (trace_id)
    TP-->>LG: Tool result
    LG->>LF: Tool trace span
    LG-->>Agent: Workflow result
    Agent-->>Op: Response + trace link
```
