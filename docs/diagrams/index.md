# Architecture Diagrams

This directory contains visual architecture diagrams for the Hermes Enterprise Reference Architecture. All diagrams are written in [Mermaid](https://mermaid.js.org/) format and render natively in GitHub Markdown.

## Diagram index

| # | Diagram | File | What it shows |
|---|---------|------|---------------|
| 1 | System Architecture | [`system-architecture.mmd`](#1-system-architecture) | Full platform component map — users, agents, orchestration, gateways, memory, tools, observability |
| 2 | Deployment Topology | [`deployment-topology.mmd`](#2-deployment-topology) | How the platform maps onto Hyper-V, VMware, Proxmox, bare-metal, and cloud VPS hosts |
| 3 | Data Flow | [`data-flow.mmd`](#3-data-flow) | End-to-end sequence of a user request through auth, model calls, tool execution, workflow triggers, and memory |
| 4 | Security Boundaries | [`security-boundaries.mmd`](#4-security-boundaries) | Trust zones — internet, DMZ, secure agent zone, data vault — and how traffic crosses boundaries |
| 5 | Governance & Approval Flow | [`governance-flow.mmd`](#5-governance--approval-flow) | State machine of an agent run from idle through auth, policy check, approval gates, mutation, and memory writes |

---

## 1. System Architecture

```mermaid
flowchart TB
    subgraph Users["👤 Users"]
        OPERATOR["Operator /<br/>Client User"]
    end

    subgraph Interface["Agent Interface Layer"]
        HERMES["Hermes Agent<br/>Control Surface"]
        WORKOS["WorkOS Control Plane<br/>Identity, Access & Policy"]
        APPROVAL["Approval Gate Engine"]
        WORKOS <--> APPROVAL
    end

    subgraph Orchestration["Orchestration & Workflow"]
        LANGGRAPH["LangGraph Supervisor<br/>Workflow Router"]
        N8N["n8n Workflow Automation<br/>Scheduled & Event-Driven"]
    end

    subgraph Gateway["Model & Tool Gateways"]
        LITELLM["LiteLLM Gateway<br/>Model Routing, Fallback,<br/>Budget Control"]
        TOOLPROXY["Tool Proxy /<br/>Governed MCP Gateway"]
    end

    subgraph Models["Model Providers"]
        LOCAL["Local Inference<br/>Ollama / llama.cpp"]
        CLOUD["Cloud Models<br/>Anthropic / OpenAI / etc."]
        LITELLM --> LOCAL
        LITELLM --> CLOUD
    end

    subgraph Memory["Memory Layer"]
        MEM0["Mem0 + Qdrant<br/>Durable Semantic Memory"]
        SESSION["Session Search<br/>Keyword Retrieval"]
        ARCHIVAL["Archival Summaries<br/>Letta / Long-Term"]
        WORKING["Working / Native Memory<br/>Immediate Steering Facts"]
    end

    subgraph Tools["Approved Tools"]
        GITHUB["GitHub"]
        NETBOX["NetBox"]
        TICKETING["Ticketing / Helpdesk"]
        EMAIL["Email / Calendar"]
        BROWSER["Web Browser"]
        APIS["Internal / External APIs"]
    end

    subgraph Observability["Observability & Evaluation"]
        LANGFUSE["Langfuse<br/>LLM Traces & Cost"]
        OTEL["OpenTelemetry<br/>Distributed Tracing"]
        PROMPTFOO["Promptfoo<br/>Regression & Safety Evals"]
    end

    OPERATOR --> HERMES
    WORKOS --> HERMES
    HERMES --> LANGGRAPH
    LANGGRAPH --> LITELLM
    LANGGRAPH --> TOOLPROXY
    LANGGRAPH --> N8N
    TOOLPROXY --> GITHUB
    TOOLPROXY --> NETBOX
    TOOLPROXY --> TICKETING
    TOOLPROXY --> EMAIL
    TOOLPROXY --> BROWSER
    TOOLPROXY --> APIS
    HERMES --> WORKING
    LANGGRAPH --> MEM0
    LANGGRAPH --> SESSION
    LANGGRAPH --> ARCHIVAL
    LANGGRAPH --> LANGFUSE
    LITELLM --> LANGFUSE
    LANGFUSE --> OTEL
    LANGGRAPH --> PROMPTFOO
```

### How to read it

This is the top-level component map. Key points:

- **Users** interact only with the Hermes Agent interface and WorkOS control plane.
- **LangGraph** is the orchestration brain — every model call, tool call, and workflow trigger flows through it.
- **LiteLLM** abstracts all model providers (local and cloud) and captures cost/usage data.
- **Tool Proxy** is the single enforcement boundary — no tool call reaches an external system without passing through it.
- **Memory** is layered: working memory for immediate context, Mem0+Qdrant for durable facts, session search for historical retrieval, archival for long summaries.
- **Observability** (Langfuse + OpenTelemetry) traces every major action end to end.

---

## 2. Deployment Topology

```mermaid
flowchart TB
    subgraph HyperV["🖥️ Hyper-V Deployment"]
        direction TB
        HV_HOST["Windows Host<br/>(Hyper-V Manager)"]
        HV_VM["Hermes Appliance VM"]
        HV_NET["Dedicated VLAN /<br/>Firewall Policy"]
        HV_HOST --> HV_NET
        HV_NET --> HV_VM
    end

    subgraph VMware["☁️ VMware / vSphere Deployment"]
        direction TB
        VM_HOST["vSphere / ESXi Host"]
        VM_VM["Hermes Appliance VM"]
        VM_NET["Dedicated VLAN /<br/>Firewall Policy"]
        VM_HOST --> VM_NET
        VM_NET --> VM_VM
    end

    subgraph Proxmox["🐧 Proxmox Deployment"]
        direction TB
        PX_HOST["Proxmox Host"]
        PX_VM["Hermes Appliance VM"]
        PX_NET["Dedicated VLAN /<br/>Firewall Policy"]
        PX_HOST --> PX_NET
        PX_NET --> PX_VM
    end

    subgraph BareMetal["⚙️ Bare-Metal Appliance"]
        direction TB
        BM_HOST["Dedicated Mini-PC / Server"]
        BM_NET["Isolated Network Segment"]
        BM_HOST --> BM_NET
    end

    subgraph CloudVPS["☁️ Cloud / VPS Deployment"]
        direction TB
        CV_HOST["Cloud Instance<br/>DigitalOcean / AWS / Azure"]
        CV_TUNNEL["Cloudflare Tunnel / VPN"]
        CV_HOST --> CV_TUNNEL
    end

    subgraph SoftwareStack["📦 Shared Software Stack (All Deployments)"]
        direction LR
        H["Hermes Agent"]
        LG["LangGraph"]
        N8N["n8n"]
        LL["LiteLLM"]
        M["Memory<br/>Mem0 + Qdrant"]
        TP["Tool Proxy"]
        O["Langfuse / OTel"]
        H --> LG
        H --> N8N
        LG --> LL
        LG --> TP
        LG --> M
        LG --> O
    end

    HyperV --> SoftwareStack
    VMware --> SoftwareStack
    Proxmox --> SoftwareStack
    BareMetal --> SoftwareStack
    CloudVPS --> SoftwareStack
```

### How to read it

All five deployment patterns run the **same software stack**. The difference is only the placement:

| Deployment | Best for |
|---|---|
| **Hyper-V** | SMB clients already running Windows Server |
| **VMware** | Regulated environments with existing vSphere estates |
| **Proxmox** | Cost-sensitive or open-source-first deployments |
| **Bare-metal** | Maximum performance, air-gapped options |
| **Cloud VPS** | Always-on webhooks, content workflows, public integrations |

Each deployment should use a dedicated VLAN or firewall policy to isolate agent traffic.

---

## 3. Data Flow

```mermaid
sequenceDiagram
    autonumber
    participant U as 👤 User / Operator
    participant W as WorkOS Control Plane
    participant H as Hermes Agent Interface
    participant LG as LangGraph Supervisor
    participant TP as Tool Proxy / MCP
    participant N8N as n8n Workflow Engine
    participant LL as LiteLLM Gateway
    participant MP as Model Provider
    participant M as Memory Layer
    participant L as Langfuse / OTel
    participant T as External Tools

    rect rgb(220, 240, 255)
        Note over U,W: Authentication & Policy Check
        U->>W: Authenticate / Request Session
        W-->>U: Session Token + Policy Bundle
    end

    rect rgb(255, 245, 220)
        Note over U,M: User Request Flow
        U->>H: Submit Task / Chat Message
        H->>LG: Forward with Run Context + Trace ID
    end

    rect rgb(255, 235, 235)
        Note over LG,LL: Model Call (if needed)
        LG->>LL: Route Model Request
        LL->>MP: Call Model API
        MP-->>LL: Model Response
        LL-->>LG: Response + Usage Data
        LL->>L: Emit Trace (tokens, cost, model)
    end

    rect rgb(235, 255, 235)
        Note over LG,T: Tool Execution (if needed)
        LG->>TP: Request Tool Call (with policy check)
        TP->>TP: Validate Allow / Deny / Approval
        alt Approved Read-Only Tool
            TP->>T: Execute Read Operation
            T-->>TP: Data Response
            TP-->>LG: Result + Audit Log
            TP->>L: Log Tool Call
        else Approval-Gated Mutation
            TP-->>H: Request Human Approval
            H-->>U: Prompt Approval
            U-->>H: Grant / Deny
            H-->>TP: Approval Decision
            alt Approved
                TP->>T: Execute Mutation
                T-->>TP: Confirmation
                TP-->>LG: Result + Audit Log
                TP->>L: Log Tool Call + Approval
            else Denied
                TP-->>LG: Tool Denied
            end
        end
    end

    rect rgb(245, 235, 255)
        Note over LG,N8N: Workflow Trigger (if applicable)
        LG->>N8N: Trigger Workflow
        N8N->>T: Execute Workflow Steps
        T-->>N8N: Results
        N8N-->>LG: Workflow Complete
        N8N->>L: Log Workflow Run
    end

    rect rgb(255, 255, 230)
        Note over LG,M: Memory Interaction
        LG->>M: Read Relevant Memory
        M-->>LG: Contextual Facts
        LG->>M: Write New Memory (classified)
        M->>M: Deduplicate / Namespace
        M->>L: Log Memory Write
    end

    rect rgb(220, 255, 220)
        Note over H,U: Response
        LG-->>H: Final Result + Trace Reference
        H-->>U: Display Response + Approval Trails
    end
```

### How to read it

This sequence diagram traces one complete agentic run from start to finish:

1. **Authentication** — WorkOS validates the session and returns a policy bundle.
2. **Request intake** — The user's task enters through the Hermes interface and is forwarded to LangGraph with a trace ID.
3. **Model call** — LiteLLM routes to the appropriate model provider, captures usage data, and emits a trace to Langfuse.
4. **Tool execution** — The Tool Proxy enforces policy. Read-only tools execute immediately; mutations require explicit human approval.
5. **Workflow automation** — n8n runs scheduled or event-driven workflows, with every step logged.
6. **Memory operations** — Memory is read for context and written back after classification, deduplication, and namespacing.
7. **Response** — The final output returns to the user with full traceability.

---

## 4. Security Boundaries

```mermaid
flowchart TB
    subgraph INTERNET["🌐 Untrusted Zone — Internet"]
        WEBHOOK_IN["Incoming Webhooks"]
        CLOUD_MODELS["Cloud Model APIs"]
        EXTERNAL_APIs["External / Public APIs"]
    end

    subgraph DMZ["🟡 DMZ / Access Zone"]
        WAF["WAF / Reverse Proxy"]
        TUNNEL["VPN / Tailscale /<br/>Cloudflare Tunnel"]
        WORKOS_V["WorkOS Control Plane<br/>Identity & Access Management"]
    end

    subgraph SECURE["🟢 Secure Zone — Agent Platform"]
        direction TB
        subgraph APPROVALZONE["Approval & Governance Zone"]
            H["Hermes Agent Interface"]
            APPROVAL["Approval Gate Engine"]
            POLICY["Tool Policy Registry<br/>Allow / Deny / Approval"]
        end
        
        subgraph EXECUTEZONE["Execution Zone"]
            LG["LangGraph Supervisor"]
            TP["Tool Proxy /<br/>Governed MCP"]
            TOOLS["Tool Integrations<br/>GitHub, NetBox,<br/>Email, etc."]
        end
    end

    subgraph DATAVAULT["🔒 Data Vault Zone"]
        direction TB
        LITELLM_SEC["LiteLLM Gateway"]
        MEM_SEC["Memory Layer<br/>Mem0 + Qdrant"]
        SECRETS["Secret Store<br/>Vault / OS Keyring"]
        LOGS["Langfuse / OTel<br/>Traces & Audit Logs"]
    end

    INTERNET --> WAF
    WAF --> TUNNEL
    TUNNEL --> WORKOS_V
    CLOUD_MODELS <-.->|"Encrypted TLS"| LITELLM_SEC
    EXTERNAL_APIs <-..->|"Via Tool Proxy Only"| TP
    
    WORKOS_V -->|"Authenticated Session"| H
    H --> APPROVAL
    APPROVAL --> POLICY
    POLICY -->|"Policy Check"| TP
    LG -->|"Trace Forward"| LOGS
    MEM_SEC -->|"Memory Audit"| LOGS
    TP -->|"Tool Audit"| LOGS
    SECRETS -->|"Inject at Runtime"| H
    SECRETS -->|"Inject at Runtime"| TP
    
    H --> LG
    LG --> TP
    TP --> TOOLS
    LG --> LITELLM_SEC
    LG --> MEM_SEC

    style INTERNET fill:#f9f,stroke:#333,stroke-width:2px,color:#333
    style DMZ fill:#ffe6cc,stroke:#d79b00,stroke-width:2px,color:#333
    style SECURE fill:#d5e8d4,stroke:#82b366,stroke-width:2px,color:#333
    style DATAVAULT fill:#dae8fc,stroke:#6c8ebf,stroke-width:2px,color:#333
```

### How to read it

This diagram shows four trust zones with enforced boundaries:

| Zone | Purpose | Access |
|---|---|---|
| **Internet** | Untrusted — webhooks, cloud APIs, public endpoints | No direct access to agent platform |
| **DMZ** | Access control — WAF, VPN tunnel, identity | Only authenticated sessions pass through |
| **Secure Zone** | Agent execution — orchestration, approval gates, tool policy | Internal only, policy-enforced |
| **Data Vault** | Sensitive data — secrets, memory, traces | Runtime injection only, never exposed |

Key principles:
- **No direct internet-to-agent path.** All traffic passes through the DMZ.
- **Secrets are injected at runtime** from a vault — they never appear in code, prompts, or logs.
- **External API calls** go exclusively through the Tool Proxy.
- **Audit logs** capture every cross-boundary event.

---

## 5. Governance & Approval Flow

```mermaid
stateDiagram-v2
    [*] --> Idle: System Ready

    Idle --> RequestReceived: User Submits Task
    RequestReceived --> AuthCheck: Authenticate via WorkOS

    AuthCheck --> PolicyEval: Session Valid
    AuthCheck --> Rejected: Session Invalid
    
    PolicyEval --> ReadOnlyCheck: Evaluate Tool Policy
    ReadOnlyCheck --> ModelCall: Read-Only Tool Requested
    ReadOnlyCheck --> MutationCheck: Mutating Tool Requested
    ReadOnlyCheck --> NoTool: No Tool Needed

    ModelCall --> ReadOnlyExecute
    ReadOnlyExecute --> LogTrace
    
    NoTool --> ModelCall

    MutationCheck --> ApprovalRequested: Mutation Detected
    ApprovalRequested --> ApprovalPending: Await Operator
    ApprovalPending --> Approved: Operator Grants Approval
    ApprovalPending --> Rejected: Operator Denies
    
    Approved --> MutatingExecute
    MutatingExecute --> LogTrace
    Rejected --> LogTrace

    LogTrace --> MemoryOps: Context Available
    MemoryOps --> MemoryClassify: Classify Content
    MemoryClassify --> MemoryWrite: Durable Fact / Procedure / Failure Mode
    MemoryClassify --> MemorySkip: Ephemeral / Secret / Chat
    MemoryWrite --> ResponseReady
    MemorySkip --> ResponseReady

    LogTrace --> ResponseReady: Direct Response
    ResponseReady --> Observable: Emit to Langfuse / OTel
    Observable --> Idle: Return to Idle

    Rejected --> Idle: Return to Idle
```

### How to read it

This state machine shows the governance lifecycle of a single agent run:

1. **Authentication** — Every session must pass WorkOS identity check.
2. **Policy evaluation** — Each tool request is classified as read-only, mutating, or no-tool-needed.
3. **Approval gates** — Mutations require explicit operator approval before execution. Denied requests are logged and returned.
4. **Trace logging** — Every path (read, mutate, deny) produces a trace in Langfuse/OTel.
5. **Memory classification** — Content is classified before being written. Secrets and ephemeral chat are never persisted as durable memory.
6. **Observable by default** — Every run emits traces, making the full lifecycle auditable.

---

## Using these diagrams

### In GitHub

Copy any ` ```mermaid ` block into a Markdown file or issue comment — GitHub renders Mermaid natively.

### In documentation tools

- **Obsidian** — Mermaid is supported out of the box.
- **Notion** — Use a Mermaid embed block.
- **Confluence** — Use the Mermaid macro.
- **VS Code** — Install the "Markdown Preview Mermaid Support" extension.

### Exporting

To export as PNG/SVG, use any of:
- <https://mermaid.live> — paste the diagram source and export.
- The `mermaid-cli` (`@mermaid-js/mermaid-cli`) for local rendering.
- Most diagramming tools (draw.io, Lucidchart) support Mermaid import.

---

## Related documentation

- [Architecture overview](../architecture/overview.md) — detailed component descriptions
- [Deployment patterns](../deployment-patterns.md) — deployment pattern selection guide
- [Security and governance](../security-and-governance.md) — full governance model and autonomy levels
- [Memory, observability & evaluation](../memory-observability-evals.md) — memory model, tracing, and eval approach
- [Client appliance profiles](../client-appliance-profiles.md) — appliance profile definitions
- [Executive summary](../executive-summary.md) — non-technical overview for directors and prospects
