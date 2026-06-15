# Governance and approval flow diagram

## Purpose

This diagram shows how an action request flows through the governance and approval system before any mutation is performed.

## Approval flow diagram

```mermaid
flowchart TD
    A["Agent receives<br/>user request"] --> B{"Tool policy<br/>check"}
    B -->|"Read-only tool"| C["Execute read<br/>Log to trace"]
    B -->|"Tool blocked"| D["Deny action<br/>Return explanation"]
    B -->|"Approval-gated tool"| E["Create approval<br/>request"]
    E --> F["Approval gate<br/>captures context"]
    F --> G{"Human<br/>approver review"}
    G -->|"Approved"| H["Execute mutation<br/>Log with approval ID"]
    G -->|"Denied"| I["Deny action<br/>Log denial reason"]
    G -->|"Timed out"| J["Deny action<br/>Escalate to operator"]
    H --> K["Post-action<br/>verification"]
    K --> L["Audit entry<br/>complete"]
    
    style A fill:#e3f2fd,stroke:#1976D2
    style B fill:#fff3e0,stroke:#F57C00
    style C fill:#e8f5e9,stroke:#388E3C
    style D fill:#ffebee,stroke:#C62828
    style E fill:#fff8e1,stroke:#F9A825
    style F fill:#e3f2fd,stroke:#1976D2
    style G fill:#fff3e0,stroke:#F57C00
    style H fill:#e8f5e9,stroke:#388E3C
    style I fill:#ffebee,stroke:#C62828
    style J fill:#ffebee,stroke:#C62828
    style K fill:#f3e5f5,stroke:#7B1FA2
    style L fill:#e0f2f1,stroke:#00695C
```

## Approval gate data captured

Every approval gate should record:

- Requested action
- Reason for the request
- Affected system
- Risk level
- Expected change
- Rollback plan
- Operator approval status
- Timestamp and actor
