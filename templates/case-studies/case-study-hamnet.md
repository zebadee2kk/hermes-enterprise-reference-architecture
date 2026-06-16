# Case Study: Private HamNet Infrastructure Deployment for Secure Remote Operations

> **Classification:** Public-Safe Template  
> **Redaction Status:** Placeholder content — no real client data  
> **Last Updated:** {{DATE}}

---

## ⚠️ Public-Safety & Redaction Guidance

Before publishing any derivative of this case study:

- [ ] **Remove all IP addresses, subnet ranges, and CIDR blocks** — replace with RFC 5737 documentation ranges (e.g., 192.0.2.0/24)
- [ ] **Remove or fictionalise all DNS names, hostnames, and domain names**
- [ ] **Remove all VPN endpoints, gateway addresses, and tunnel configurations**
- [ ] **Replace real geographic locations with region-level descriptors** (e.g., "US West Coast primary site")
- [ ] **Remove any cryptographic parameters, key lengths beyond public defaults, or certificate details**
- [ ] Confirm no operational procedures detail specific defensive postures
- [ ] Have the final draft reviewed by the Security team before publication

---

## Problem Statement

**Client Profile:** {{CLIENT}} — a {{INDUSTRY}}-sector organisation with distributed operations across **_{{NUM_SITES}}_** sites

### The Challenge

{{CLIENT}} operated critical workloads requiring network isolation from the public internet, but their existing setup was unsustainable:

- **Legacy VPN sprawl:** {{NUM_LEGACY_VPN}} point-to-point VPN tunnels were manually configured, fragile, and difficult to audit. Mean time to repair (MTTR) for a tunnel failure was **{{MTTR_LEGACY}} hours**.
- **Access inconsistency:** Remote staff and contractors used a patchwork of {{NUM_AUTH_METHODS}} different authentication mechanisms, some without MFA.
- **Scalability wall:** Adding a new site required **{{WEEKS_NEW_SITE}} weeks** of calendar time for hardware procurement, shipping, and manual configuration.
- **Visibility gaps:** Central IT had no unified view of network health or traffic flows across sites. Incident response relied on phone calls and intermittent ping checks.

### Constraints

| Constraint | Detail |
|---|---|
| Security posture | Air-gred or private-only — no public internet dependency for control plane |
| Compliance | Must satisfy **{{FRAMEWORK}}** network segmentation requirements |
| Budget | **${{BUDGET}}** CAPEX + **${{OPEX_ANNUAL}}/yr** operating |
| Staffing | {{NUM_NETWORK_STAFF}} network engineers |
| Timeline | **{{WEEKS}} weeks** to first production site |
| Compatibility | Must support {{LEGACY_PROTO}} legacy application protocols |

---

## Solution Architecture

### High-Level Topology

```
                    ┌────────────────────┐
                    │  Hermes Control     │
                    │  Plane ({{REGION}}) │
                    └────────┬───────────┘
                             │  ({{MGMT_TRANSPORT}})
              ┌──────────────┼──────────────┐
              │              │              │
        ┌─────▼─────┐ ┌─────▼─────┐ ┌─────▼─────┐
        │  Site A    │ │  Site B    │ │  Site C    │
        │ (Primary)  │ │ (Secondary)│ │ (Remote)   │
        │ {{CITY_A}} │ │ {{CITY_B}} │ │ {{CITY_C}} │
        └───────────┘ └───────────┘ └───────────┘
```

### Components

1. **Mesh Overlay Network**  
   A full-mesh encrypted overlay running **{{OVERLAY_TECH}}** (e.g., WireGuard, OpenVPN, Tailscale) across all sites. Peer discovery is managed by a central controller; data plane traffic flows directly between peers (no hairpinning through a hub).

2. **Central Control Plane (Hermes)**  
   Hermes agent manages configuration distribution, health monitoring, and key rotation. All configuration is declarative and stored in a Git repository.

3. **Identity-Aware Access**  
   Every device and user is authenticated via {{IDP}}. Access policies enforce least-privilege segmentation (e.g., Site B IoT devices cannot reach Site A databases).

4. **Health Monitoring & Alerting**  
   Continuous latency, throughput, and loss measurements between all peers. Anomalies trigger alerts via {{ALERT_TOOL}}.

### Network Segments

| Segment | Purpose | Access |
|---|---|---|
| `mgmt` ({{VLAN_MGMT}}) | Infrastructure management | Network team only |
| `workload` ({{VLAN_WORK}}) | Application traffic | Authorised users + services |
| `iot` ({{VLAN_IOT}}) | Sensor/OT data collection | Unidirectional to workload DMZ |
| `partner` ({{VLAN_PARTNER}}) | Third-party access | Time-bound, audited |

---

## Deployment Details

### Phase 1 — Pilot (Weeks 1–{{W1}})

- Deployed overlay nodes at **Site A** (primary) and **Site B** (secondary)
- Migrated **{{PILOT_SERVICES}}** services from legacy VPN to the overlay
- Validated performance benchmarks against SLA targets
- Configured monitoring dashboards in {{MONITORING_TOOL}}

### Phase 2 — Expansion (Weeks {{W2_START}}–{{W2_END}})

- Rolled out to **{{NUM_PHASE2_SITES}}** additional sites
- Decommissioned **{{NUM_DECOM_VPN}}** legacy VPN tunnels
- Implemented automated key rotation (every **{{KEY_ROTATION_DAYS}}** days)
- Onboarded **{{NUM_PHASE2_USERS}}** remote users

### Phase 3 — Hardening (Weeks {{W3_START}}–{{W3_END}})

- Enabled mutual TLS between all overlay peers
- Deployed network policy enforcement at the edge ({{FW_TOOL}})
- Conducted penetration test — **{{PENTEST_FINDINGS}}** findings, all remediated within **{{REMEDIATION_DAYS}}** days
- Documented and handed over runbooks to operations

### Key Rotation Schedule

| Key Type | Rotation Frequency | Method |
|---|---|---|
| Peer tunnel keys | Every **{{PEER_KEY_DAYS}}** days | Automated via Hermes |
| Control plane API keys | Every **{{API_KEY_DAYS}}** days | Automated via Hermes |
| User/device certificates | Every **{{CERT_DAYS}}** days | {{IDP}} lifecycle |
| Pre-shared keys (if any) | Every **{{PSK_DAYS}}** days | Manual + PR review |

---

## Results & Metrics

### Quantitative Outcomes

| Metric | Before | After ({{MONTHS}} months) | Improvement |
|---|---|---|---|
| MTTR (network failure) | {{MTTR_OLD}} hrs | {{MTTR_NEW}} mins | **{{IMPACT_MTTR}}%** reduction |
| Time to onboard new site | {{OLD_SITE_WEEKS}} wks | {{NEW_SITE_DAYS}} days | **{{IMPACT_SITE_TIME}}%** reduction |
| Auth mechanisms (unique) | {{OLD_AUTH_COUNT}} | 1 ({{IDP}}) | Standardised |
| VPN tunnels (active) | {{OLD_TUNNELS}} | {{NEW_TUNNELS}} | **{{IMPACT_TUNNELS}}%** reduction |
| Sites with full visibility | {{OLD_VIS_PCT}}% | 100% | **{{IMPACT_VIS}}pp** increase |
| Monthly network incidents | {{OLD_INCIDENTS}} | {{NEW_INCIDENTS}} | **{{IMPACT_INCIDENTS}}%** reduction |

### Qualitative Outcomes

- **Operational simplicity:** Network team reported that "adding a site feels like checking a box now."
- **Security confidence:** The pen test validated that network segmentation was correctly enforced end-to-end.
- **Legacy protocol support:** All {{LEGACY_PROTO}} applications migrated without modification — the overlay is Layer 3 transparent.

### Cost Summary

```
Legacy VPN annual cost:     ${{LEGACY_ANNUAL_COST}}
New overlay annual cost:    ${{NEW_ANNUAL_COST}}
Annual savings:             ${{ANNUAL_SAVINGS}}
Implementation cost:        ${{IMPLEMENT_COST}}
Payback period:             {{PAYBACK_MONTHS}} months
```

---

## Lessons Learned

### What Went Well

1. **Declarative configuration in Git.** Having the entire network topology as code meant every change was peer-reviewed and auditable. Rollback was a `git revert`.
2. **Phased legacy migration.** Moving service-by-service (rather than site-by-site) reduced risk: if the overlay had issues, only one workload was affected.
3. **Monitoring-first mindset.** Deploying observability *before* migration meant we had a clean baseline to compare against.

### What We'd Do Differently

1. **Provision cellular failover earlier.** A single WAN link failure at Site C would have taken down its overlay tunnel. We added LTE failover in Phase 3 — should have been Phase 1.
2. **Set MTU expectations upfront.** Some legacy applications used jumbo frames and broke on the overlay. A pre-migration audit would have caught this.
3. **Document the "why" better.** New team members later questioned design decisions that weren't captured in ADRs. We retrofitted documentation after the fact.

### Replicable Patterns

- The overlay-as-code pattern (Git + Hermes) is applicable to any multi-site or multi-cloud network.
- The phased service migration approach works for any infrastructure transition where downtime must be minimised.

---

## Appendix

### Related Templates

- [Deployment Patterns - HamNet](../../docs/deployment-patterns.md#hamnet)
- [Security & Governance - Network Segmentation](../../docs/security-and-governance.md#network-segmentation)
- [Architecture - Hybrid Connectivity](../architecture/{{HAMNET_ADR}})

### Glossary

| Term | Definition |
|---|---|
| HamNet | Private encrypted overlay network for isolated operations |
| Overlay | A virtual network layered on top of physical infrastructure |
| MTTR | Mean Time To Repair |

### Contact

For questions about this case study, contact: **{{OWNER_NAME}}** ({{OWNER_EMAIL}})  
Template version: **{{TEMPLATE_VERSION}}**
