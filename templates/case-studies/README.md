# Case Study Templates

> **Location:** `templates/case-studies/`  
> **Purpose:** Reusable, public-safe case study templates for portfolio and show-home material.  
> **Maintainer:** {{MAINTAINER_NAME}} ({{MAINTAINER_EMAIL}})

---

## Available Templates

| Template | Scenario | Primary Use Case |
|---|---|---|
| [case-study-workos.md](./case-study-workos.md) | Enterprise Identity & Access Automation | Agentic workflow for IAM provisioning/deprovisioning |
| [case-study-hamnet.md](./case-study-hamnet.md) | Private HamNet Infrastructure | Secure multi-site network overlay deployment |
| [case-study-client-lab.md](./case-study-client-lab.md) | Client AI Lab | Managed GPU infrastructure for ML experimentation |
| [case-study-support-desk.md](./case-study-support-desk.md) | Support Desk Co-Pilot | AI-augmented ticket resolution |
| [case-study-content-ops.md](./case-study-content-ops.md) | Content Operations | Multi-channel content automation |

---

## How to Use These Templates

1. **Copy** the relevant template to a new file (e.g., `case-study-acme-corp.md`).
2. **Replace all `{{PLACEHOLDER}}` values** with real (or fictionalised) data.
3. **Complete the redaction checklist** at the top of the file before sharing externally.
4. **Have the draft reviewed** by the relevant domain owner, Legal, and Marketing.
5. **Publish** only after all placeholders are resolved and approvals are obtained.

## Placeholder Conventions

| Pattern | Meaning | Example |
|---|---|---|
| `{{CLIENT}}` | Client or organisation name (anonymise for public) | "Acme Corp" → "a mid-market fintech" |
| `{{DATE}}` | Date of last update | `2025-01-15` |
| `{{NUM_*}}` | Numeric value (count, percentage, etc.) | `42`, `15%` |
| `{{OLD_*}} / {{NEW_*}}` | Before/after comparison values | `12 hrs` / `2 hrs` |
| `{{IMPACT_*}}` | Calculated improvement | `83%` reduction |
| `{{*_*}}` | General placeholder (context-dependent) | `{{CLOUD_PROVIDER}}` → `AWS` |

## Redaction Rules (All Templates)

1. **Never publish real client names** without written approval.
2. **Never publish internal IPs, hostnames, or URLs** — use RFC 5737 ranges or fictional examples.
3. **Never publish API keys, tokens, or credentials** — even if they appear in screenshots.
4. **Generalise specific figures** that could reveal commercial terms (use ranges or percentages).
5. **Remove security-sensitive configurations** (firewall rules, auth mechanisms, network topology details).
6. **Anonymise all personal names** unless explicit consent is obtained.

## Contributing a New Template

1. Create a new file in this directory: `case-study-<scenario>.md`.
2. Follow the structure: Problem Statement → Solution Architecture → Deployment Details → Results/Metrics → Lessons Learned.
3. Include the redaction guidance header.
4. Use `{{PLACEHOLDER}}` syntax for all variable content.
5. Add an entry to the table above.
6. Submit a PR with the `documentation` and `client-facing` labels.

---

*Template version: {{TEMPLATE_VERSION}}*
