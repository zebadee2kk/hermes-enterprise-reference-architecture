# Client appliance network and data-flow diagram

## Purpose

This diagram shows how a client appliance deployment is typically placed on a client network, what traffic flows exist, and where governance boundaries sit.

## Network and data-flow diagram

```mermaid
flowchart LR
    subgraph Internet["Internet"]
        A["Cloud model APIs"]
        B["Cloud Langfuse<br/>(optional)"]
    end

    subgraph ClientNetwork["Client site network"]
        subgraph DMZ["DMZ / appliance VLAN"]
            C["Hermes AI appliance<br/>VM or physical host"]
        end

        subgraph Internal["Internal systems"]
            D["Ticketing / helpdesk"]
            E["File shares / docs"]
            F["Monitoring / RMM"]
            G["GitHub / Git server"]
            H["Email / calendar"]
        end

        subgraph Security["Security boundary"]
            I["Firewall / VLAN policy"]
        end
    end

    subgraph Remote["Remote support"]
        J["Support engineer<br/>via VPN / Tailscale"]
    end

    C -->|"model calls"| A
    C -->|"traces (redacted)"| B
    C -->|"read-only queries"| D
    C -->|"read-only access"| E
    C -->|"read-only polling"| F
    C -->|"read-only / approval-gated"| G
    C -->|"approval-gated"| H
    C --> I
    J -->|"encrypted tunnel"| I

    style Internet fill:#e3f2fd,stroke:#1976D2
    style ClientNetwork fill:#e8f5e9,stroke:#388E3C
    style DMZ fill:#fff3e0,stroke:#F57C00
    style Internal fill:#f3e5f5,stroke:#7B1FA2
    style Security fill:#fce4ec,stroke:#C62828
    style Remote fill:#e0f2f1,stroke:#00695C
```

## Traffic rules

| Flow | Direction | Policy |
|---|---|---|
| Appliance to cloud model APIs | Outbound HTTPS | Allowed |
| Appliance to cloud Langfuse | Outbound HTTPS | Allowed, redacted |
| Appliance to internal systems | Internal | Read-only by default |
| Internal systems to appliance | Internal | Not required |
| Remote support to appliance | Via VPN/tunnel | Encrypted, audited |
| Appliance to internet (other) | Outbound | Blocked by default |

## Key governance points

- The appliance sits in a dedicated VLAN or DMZ segment.
- All external traffic is outbound-only; no inbound internet connections.
- Remote support access requires an encrypted tunnel and is logged.
- Internal system access starts read-only; mutations require approval.
