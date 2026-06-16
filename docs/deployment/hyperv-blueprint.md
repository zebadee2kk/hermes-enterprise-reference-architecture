# Hyper-V appliance deployment blueprint

## Purpose

This blueprint describes a repeatable pattern for deploying the Hermes Enterprise Reference Architecture as a Hyper-V virtual machine. It is intended for delivery engineers scoping an SMB or internal deployment.

## Recommended VM sizing

| Band | vCPUs | RAM | Disk | Use case |
|---|---|---|---|---|
| Minimal | 2 | 8 GB | 80 GB | Single-agent, light workflows, evaluation |
| Standard | 4 | 16 GB | 120 GB | Full platform with memory, traces, and multiple workflows |
| Performance | 8 | 32 GB | 200 GB | Multi-agent, heavier models, concurrent workloads |

## Network placement

- Create a dedicated **internal** or **private** Hyper-V switch for the appliance VLAN.
- Do **not** place the appliance on an internet-facing switch without a firewall in front.
- Assign a static IP or use DHCP reservation on the appliance VLAN.
- Configure Windows Firewall on the host to restrict access to the appliance.

## Storage and backup

- Use a **fixed-size VHDX** for predictable performance.
- Store the VHDX on a redundant volume (RAID1/RAID5 or storage spaces).
- Take **daily checkpoints** (production checkpoints, not standard snapshots).
- Export weekly backups to a separate host or NAS.
- Test restore monthly — a backup that cannot be restored is not a backup.

## Secrets handling

- Do **not** store secrets inside the VM disk image in plaintext.
- Use one of the following:
  - OS-level keyring / Credential Manager;
  - HashiCorp Vault or cloud secret manager;
  - Client-approved password manager.
- Rotate any credentials stored in the VM on a documented schedule.
- Never place secrets in scripts, prompts, or README files inside the VM.

## Update and patching model

1. **Monthly**: Apply OS security updates during an approved maintenance window.
2. **Per release**: Update agent platform components (LangGraph, n8n, LiteLLM, memory) after running evaluation tests.
3. **Before any update**: Take a production checkpoint so you can roll back.
4. **After any update**: Run smoke tests before handing back to operations.

## Support access model

- Enable ** PowerShell Remoting** or **WinRM** over HTTPS with client-approved certificates.
- Restrict Remote Desktop to named support accounts only.
- Use a **jump-host** or **VPN** — do not expose RDP directly to the internet.
- Log all remote support sessions and retain logs per the client’s retention policy.

## Validation checklist

- [ ] VM created with correct sizing band
- [ ] Network switch and VLAN configured
- [ ] Static IP or DHCP reservation assigned
- [ ] OS hardened and patched
- [ ] Secrets store configured and empty of plaintext credentials
- [ ] Backup schedule active and tested
- [ ] Support access restricted to named accounts
- [ ] Monitoring and tracing agents installed
- [ ] Approval-to-mutation workflow verified
- [ ] Restore-from-backup tested

## Risks and trade-offs

| Risk | Mitigation |
|---|---|
| Hyper-V host overload | Monitor host CPU, RAM, and disk I/O; do not co-locate with other heavy VMs |
| VHDX corruption | Use fixed-size VHDX on redundant storage; maintain off-host backups |
| Windows licensing | Confirm client has appropriate Windows Server or Hyper-V guest licences |
| Snapshot sprawl | Remove old production checkpoints promptly; do not use as backups |
