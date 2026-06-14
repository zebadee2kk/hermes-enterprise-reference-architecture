# Forward roadmap

## Purpose

This roadmap turns the Hermes enterprise reference architecture into a practical, reusable delivery framework.

It is designed to align with the private `hermes-mgmt` management plane while remaining safe for a public/client-facing reference repository.

## Phase 0 — Foundation documentation

### Goal

Make the repository useful to a new reader in under ten minutes.

### Tasks

- [ ] Complete initial architecture overview.
- [ ] Complete security and governance model.
- [ ] Complete deployment pattern docs.
- [ ] Complete client appliance profiles.
- [ ] Add diagrams for logical architecture and deployment patterns.
- [ ] Add glossary for non-technical clients.

### Exit criteria

A client or technical reviewer can understand what the platform is, what it does, and how it is governed.

## Phase 1 — Reference diagrams and show-home pack

### Goal

Turn the documentation into a visual architecture pack for sales, consulting, and client workshops.

### Tasks

- [ ] Add logical architecture diagram.
- [ ] Add client appliance network/data-flow diagram.
- [ ] Add governance flow diagram.
- [ ] Add memory/observability/eval flow diagram.
- [ ] Add one-page executive summary.
- [ ] Add slide/deck outline for client presentation.

### Exit criteria

The repo can be used as the basis for a client discovery meeting or architecture workshop.

## Phase 2 — Deployment blueprints

### Goal

Create deployment-ready blueprints for common scenarios.

### Tasks

- [ ] Local lab blueprint.
- [ ] Hyper-V VM blueprint.
- [ ] VMware VM blueprint.
- [ ] Proxmox VM blueprint.
- [ ] VPS/cloud blueprint.
- [ ] Managed jump-box blueprint.
- [ ] Backup and restore blueprint.

### Exit criteria

A delivery engineer can use the docs to plan a deployment without needing private HamNet/Hermes details.

## Phase 3 — Governance templates

### Goal

Standardise how client deployments are approved, operated, and reviewed.

### Tasks

- [ ] Approval gate template.
- [ ] Tool access register template.
- [ ] Memory policy template.
- [ ] Client onboarding checklist.
- [ ] Monthly review checklist.
- [ ] Incident/runbook template.
- [ ] Data protection review checklist.

### Exit criteria

Every deployment can start with the same governance pack and then adapt to client needs.

## Phase 4 — Evaluation and observability pack

### Goal

Make quality and safety measurable.

### Tasks

- [ ] Define Promptfoo baseline eval suite.
- [ ] Define Langfuse trace attribute conventions.
- [ ] Define LiteLLM cost reporting pattern.
- [ ] Define memory quality review process.
- [ ] Define regression gates for prompts/workflows.

### Exit criteria

A deployment has a repeatable method to prove it is working, safe, and improving.

## Phase 5 — Client appliance productisation

### Goal

Turn the architecture into repeatable consulting offerings.

### Tasks

- [ ] Package read-only reconnaissance appliance.
- [ ] Package content operations appliance.
- [ ] Package support desk co-pilot appliance.
- [ ] Package finance/admin automation appliance.
- [ ] Package managed AI jump-box.
- [ ] Package executive reporting appliance.
- [ ] Add pricing/scope assumptions in a private companion repo if needed.

### Exit criteria

The architecture is ready to support client proposals and delivery scopes.

## Phase 6 — Public credibility and portfolio use

### Goal

Use the repository as a public proof point for AI architecture capability.

### Tasks

- [ ] Add clear public README positioning.
- [ ] Add examples without exposing real client data.
- [ ] Add comparison with traditional automation/RPA/MSP approaches.
- [ ] Add case-study template.
- [ ] Link from relevant portfolio/show-home sites.

### Exit criteria

The repo can be shown to clients, recruiters, and partners as evidence of practical agentic AI architecture work.

## Priority order

1. Keep this repo public-safe.
2. Finish diagrams and executive summary.
3. Add deployment blueprints.
4. Add governance templates.
5. Add eval/observability pack.
6. Productise appliance profiles.

## Non-goals

- Do not include live credentials or private configuration.
- Do not duplicate the private `hermes-mgmt` repo.
- Do not include client-identifiable data.
- Do not publish pricing or commercial terms unless deliberately intended.
- Do not position the architecture as fully autonomous or unsupervised.
