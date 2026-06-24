# Deployment pattern comparison

## Purpose

This diagram compares the five reference deployment patterns to help clients and delivery teams select the right starting point.

## Pattern comparison diagram

```mermaid
flowchart LR
    subgraph Patterns["Deployment patterns"]
        P1["1. Local lab<br/>Workstation"]
        P2["2. Client appliance<br/>VM at client site"]
        P3["3. Cloud / VPS<br/>Hosted platform"]
        P4["4. Enterprise<br/>Segmented"]
        P5["5. Managed jump-box<br/>MSP support"]
    end

    subgraph Selection["Selection factors"]
        S1["Personal use"]
        S2["SMB client"]
        S3["Content / web"]
        S4["Regulated org"]
        S5["MSP delivery"]
    end

    P1 -.-> S1
    P2 -.-> S2
    P3 -.-> S3
    P4 -.-> S4
    P5 -.-> S5

    style Patterns fill:#e3f2fd,stroke:#1976D2
    style Selection fill:#e8f5e9,stroke:#388E3C
```

## Pattern comparison table

| Pattern | Best for | Always-on | Client data locality | Governance complexity |
|---|---|---|---|---|
| Local lab / workstation | Development, experiments, demos | No | Local | Low |
| Client appliance VM | SMB/MSP client delivery | Yes | On-site | Medium |
| Cloud / VPS hosted | Website/content, external integrations | Yes | Cloud | Medium |
| Enterprise segmented | Regulated, larger organisations | Yes | Per policy | High |
| Managed AI jump-box | MSP support, remote operations | Yes | On-site | Medium-High |
