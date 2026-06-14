# Deployment patterns

## Purpose

This document describes common deployment patterns for a Hermes-style governed agentic AI platform.

The goal is to make deployments repeatable without pretending every client has the same infrastructure, risk appetite, or integration needs.

## Pattern 1 — Local lab / development workstation

### Use case

Best for experimentation, prompt development, memory tuning, and workflow design.

### Typical components

- Hermes/operator interface on local machine;
- LiteLLM local gateway;
- local or remote model providers;
- Qdrant/Mem0 for memory;
- optional local n8n;
- Langfuse cloud or local dev instance;
- local Tool Proxy in read-only mode.

### Pros

- Fast iteration;
- low cost;
- good for private development;
- simple rollback.

### Cons

- not always-on unless the workstation is always-on;
- less suitable for client production workloads;
- risk of mixing personal and client contexts if poorly segmented.

## Pattern 2 — Client appliance VM

### Use case

Best for SMB/MSP client delivery where the agent needs local access to systems but should be isolated and supportable.

### Typical placement

- Hyper-V, VMware, Proxmox, or dedicated mini-PC;
- dedicated VLAN or firewall policy;
- remote support via VPN or approved secure tunnel;
- local integrations to ticketing, documents, ERP/CRM, file shares, or monitoring.

### Typical components

- Hermes/operator interface;
- Tool Proxy;
- LiteLLM gateway;
- memory layer;
- n8n workflows;
- Langfuse/OTel traces;
- backup and restore process;
- client-specific approval policy.

### Pros

- client data remains near the client;
- easier network integration;
- manageable appliance model;
- strong fit for recurring support contracts.

### Cons

- needs patching and lifecycle management;
- needs remote access model;
- local hardware/VM constraints matter.

## Pattern 3 — Cloud/VPS hosted platform

### Use case

Best for website/content workflows, external integrations, public APIs, and scheduled automation that does not require deep local network access.

### Typical components

- hosted n8n;
- LiteLLM gateway;
- Langfuse;
- GitHub/website/content integrations;
- secure secret store;
- optional memory layer;
- Cloudflare Tunnel or equivalent access control.

### Pros

- always-on;
- easy external webhook access;
- good for content and publishing workflows;
- simple to snapshot/backup.

### Cons

- may not suit sensitive client data;
- requires careful internet exposure controls;
- cloud costs and provider dependencies.

## Pattern 4 — Enterprise segmented deployment

### Use case

Best for larger organisations with formal governance, security review, identity controls, and internal change approval processes.

### Typical components

- separated dev/test/prod environments;
- formal identity integration;
- client-approved secret manager;
- SIEM/log forwarding;
- private model endpoints or approved cloud models;
- change-management integration;
- separate audit and evaluation storage.

### Pros

- aligns with enterprise governance;
- supports regulated or audited environments;
- clearer responsibility model.

### Cons

- slower setup;
- more stakeholders;
- heavier documentation and approval requirements.

## Pattern 5 — Managed AI jump-box

### Use case

Best for MSP-style support where the agent assists with investigation, documentation, runbooks, and controlled remote operations.

### Typical capabilities

- read-only inventory review;
- ticket/context summarisation;
- configuration comparison;
- runbook suggestion;
- approved workflow triggering;
- evidence collection;
- operator handover notes.

### Strong controls required

- no arbitrary shell by default;
- no credential dumping;
- all mutating actions approval-gated;
- strong session logging;
- per-client isolation;
- client-owned access controls.

## Selection guide

| Requirement | Best starting pattern |
|---|---|
| Personal experimentation | Local lab |
| SMB client with local systems | Client appliance VM |
| Website/content automation | Cloud/VPS hosted |
| Regulated enterprise | Enterprise segmented |
| MSP support and remote ops | Managed AI jump-box |

## Minimum deployment checklist

Before any deployment is considered live:

- [ ] owner named;
- [ ] deployment pattern selected;
- [ ] network boundary documented;
- [ ] approved tools listed;
- [ ] hard-blocked tools listed;
- [ ] secret store selected;
- [ ] backup process tested;
- [ ] logging/tracing enabled;
- [ ] memory policy enabled;
- [ ] approval model agreed;
- [ ] recovery procedure documented.
