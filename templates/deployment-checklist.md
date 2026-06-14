# Deployment checklist

## Deployment identity

- Client/project:
- Deployment pattern:
- Owner:
- Technical lead:
- Date:

## Scope

- Primary use case:
- In-scope systems:
- Out-of-scope systems:
- Allowed data types:
- Blocked data types:

## Infrastructure

- Host platform:
- CPU/RAM/storage:
- Network/VLAN:
- Firewall rules:
- DNS/hostname:
- Backup target:
- Remote access method:

## Core components

- [ ] Agent interface installed/configured
- [ ] LiteLLM/model gateway configured
- [ ] Memory layer configured
- [ ] Tool Proxy/governed tool gateway configured
- [ ] n8n/workflow automation configured
- [ ] Langfuse/observability configured
- [ ] Prompt/eval suite baseline configured
- [ ] Secret store configured

## Security controls

- [ ] Dedicated service account(s)
- [ ] Least-privilege access
- [ ] Secrets stored outside repo
- [ ] Logs redacted
- [ ] Tool allowlist documented
- [ ] Hard-block list documented
- [ ] Approval gates tested
- [ ] Backup/restore tested

## Validation

- [ ] Read-only tool test
- [ ] Memory write/read test
- [ ] Secret redaction test
- [ ] Approval-gated mutation test
- [ ] Trace visibility test
- [ ] Workflow automation test
- [ ] Failure/retry test
- [ ] Restore test

## Handover

- [ ] Architecture summary delivered
- [ ] Admin/operator guide delivered
- [ ] Support process agreed
- [ ] Review cadence agreed
- [ ] Known limitations documented
- [ ] Next-phase backlog created
