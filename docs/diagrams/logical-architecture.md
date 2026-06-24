# Logical platform architecture

## Purpose

This diagram shows the logical layers of the Hermes Enterprise Reference Architecture and how they relate to each other. It is intended for both technical and non-technical audiences.

## Logical architecture diagram

```mermaid
flowchart TB
    subgraph Client["Client / Operator"]
        A["Human operator<br/>or client user"]
    end

    subgraph AgentLayer["Agent interface layer"]
        B["Agent interface<br/>Hermes-style control surface"]
    end

    subgraph Orchestration["Orchestration layer"]
        C["LangGraph supervisor<br/>workflow router"]
    end

    subgraph Automation["Workflow automation"]
        D["n8n<br/>repeatable workflows"]
    end

    subgraph Gateway["Gateway layer"]
        E["Tool Proxy<br/>governed MCP gateway"]
        F["LiteLLM<br/>model gateway"]
    end

    subgraph Memory["Memory layer"]
        G["Mem0 + Qdrant<br/>durable semantic memory"]
        H["Session search<br/>historical retrieval"]
    end

    subgraph Observability["Observability layer"]
        I["Langfuse<br/>LLM traces & evals"]
        J["OpenTelemetry<br/>distributed tracing"]
    end

    subgraph Models["Model providers"]
        K["Local inference"]
        L["Cloud model APIs"]
    end

    subgraph Tools["Approved external tools"]
        M["GitHub / Git"]
        N["Ticketing / helpdesk"]
        O["Email / calendar"]
        P["Monitoring / RMM"]
        Q["Browser / APIs"]
    end

    A --> B
    B --> C
    C --> E
    C --> D
    C --> F
    C --> G
    C --> H
    E --> M
    E --> N
    E --> O
    E --> P
    E --> Q
    F --> K
    F --> L
    C --> I
    C --> J
    D --> I

    style Client fill:#e8f4f8,stroke:#2196F3
    style AgentLayer fill:#e3f2fd,stroke:#1976D2
    style Orchestration fill:#fff3e0,stroke:#F57C00
    style Automation fill:#fff8e1,stroke:#F9A825
    style Gateway fill:#e8f5e9,stroke:#388E3C
    style Memory fill:#f3e5f5,stroke:#7B1FA2
    style Observability fill:#fce4ec,stroke:#C62828
    style Models fill:#e0f2f1,stroke:#00695C
    style Tools fill:#fffde7,stroke:#F57F17
```

## Layer summary

| Layer | Role |
|---|---|
| Client / Operator | The human who interacts with the platform |
| Agent interface | The main human-agent interaction surface |
| Orchestration | Routes tasks, preserves context, propagates trace IDs |
| Workflow automation | Scheduled and event-driven repeatable workflows |
| Tool gateway | Enforces deny-by-default policy, approval gates, audit |
| Model gateway | Routes model calls, captures cost, enables fallback |
| Memory | Durable recall with classification, dedupe, and review |
| Observability | Traces, debugging, cost tracking, eval context |
| Model providers | Local or cloud inference endpoints |
| External tools | GitHub, ticketing, email, monitoring, browser, APIs |
