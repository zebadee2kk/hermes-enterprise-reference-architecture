# Client appliance profiles

## Purpose

This document defines reusable client appliance profiles for packaging the Hermes enterprise reference architecture into practical offerings.

Each profile should be treated as a starting point. The final design must be adapted to the client’s systems, risk appetite, data sensitivity, and support model.

## Profile 1 — Read-only reconnaissance appliance

### Purpose

Give the agent safe read-only visibility into selected systems so it can produce documentation, summaries, risk findings, and improvement plans.

### Typical integrations

- GitHub/Git repositories;
- documentation systems;
- monitoring dashboards;
- NetBox or inventory source;
- ticketing/helpdesk system;
- website CMS read-only access.

### Allowed actions

- read approved sources;
- summarise findings;
- create draft reports;
- recommend actions;
- identify missing documentation.

### Blocked actions

- production changes;
- ticket closure;
- email sending;
- repo writes;
- firewall/network changes;
- credential access.

## Profile 2 — Content operations appliance

### Purpose

Research, draft, review, publish, and manage content workflows for websites and social channels with approval gates.

### Typical integrations

- CMS/WordPress;
- GitHub website repo;
- n8n publishing workflow;
- brand/style documentation;
- SEO/content research tools;
- image/document asset store.

### Allowed actions

- research topics;
- draft articles/pages;
- prepare metadata;
- create content calendars;
- create draft CMS entries;
- trigger approval workflow.

### Approval-gated actions

- publish content;
- update live pages;
- send newsletters;
- post to social accounts.

## Profile 3 — Support desk co-pilot appliance

### Purpose

Assist support teams with triage, summarisation, runbook suggestions, and ticket workflow automation.

### Typical integrations

- helpdesk/ticketing system;
- knowledge base;
- RMM/monitoring read-only views;
- client documentation;
- email inbox or shared mailbox.

### Allowed actions

- summarise tickets;
- identify similar issues;
- suggest resolution steps;
- draft ticket replies;
- draft KB articles;
- escalate based on rules.

### Approval-gated actions

- send customer replies;
- close tickets;
- run remediation scripts;
- modify KB articles;
- change monitoring or RMM policies.

## Profile 4 — Finance/admin automation appliance

### Purpose

Reduce manual processing for statements, invoices, reconciliations, renewals, and admin workflows.

### Typical integrations

- email inbox;
- document drop folder;
- accounting platform;
- spreadsheet store;
- n8n workflow;
- approval queue.

### Allowed actions

- extract data from documents;
- classify documents;
- draft reconciliations;
- flag anomalies;
- prepare import files;
- create approval summaries.

### Approval-gated actions

- posting transactions;
- changing supplier/customer records;
- sending payment instructions;
- deleting records.

## Profile 5 — Managed AI jump-box

### Purpose

Provide a secure local management point for agent-assisted client support, especially where remote SSH/support access is currently manual.

### Typical integrations

- VPN/Tailscale/approved remote access;
- local monitoring;
- documentation store;
- NetBox/inventory;
- GitHub/Git repo;
- ticketing system.

### Allowed actions

- collect evidence;
- read approved logs;
- compare configuration snapshots;
- draft runbook steps;
- produce handover notes;
- trigger pre-approved read-only diagnostics.

### Approval-gated actions

- restart services;
- change configuration;
- deploy updates;
- run scripts;
- modify firewall/VPN/identity settings.

## Profile 6 — Executive/management reporting appliance

### Purpose

Produce recurring management reports from operational systems, support metrics, content metrics, security posture, or project status.

### Typical integrations

- GitHub project/issues;
- helpdesk/ticketing data;
- monitoring dashboards;
- spreadsheet/reporting data;
- CRM/ERP exports;
- Langfuse/cost data.

### Allowed actions

- read metrics;
- generate weekly/monthly summaries;
- create draft reports;
- highlight anomalies;
- recommend follow-up actions.

### Approval-gated actions

- sending reports externally;
- changing dashboards;
- creating public-facing updates.

## Packaging guidance

For each client appliance, produce:

- scope statement;
- network/data-flow diagram;
- integration list;
- approval policy;
- blocked operations list;
- backup/restore runbook;
- support access model;
- client handover pack;
- monthly review checklist.

## Commercial note

The appliance profiles should become reusable consulting products. The architecture is the same, but the profile and integrations change by client need.
