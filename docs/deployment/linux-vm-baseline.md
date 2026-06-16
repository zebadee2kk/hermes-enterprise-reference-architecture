# Generic Linux VM baseline blueprint

## Purpose

This blueprint describes a minimal, hypervisor-agnostic Linux VM baseline for the Hermes Enterprise Reference Architecture. Use it when the target hypervisor is not Hyper-V, VMware, or Proxmox, or when a generic pattern is preferred.

## Recommended VM sizing

| Band | vCPUs | RAM | Disk | Use case |
|---|---|---|---|---|
| Minimal | 2 | 8 GB | 80 GB | Single-agent, light workflows, evaluation |
| Standard | 4 | 16 GB | 120 GB | Full platform with memory, traces, and multiple workflows |
| Performance | 8 | 32 GB | 200 GB | Multi-agent, heavier models, concurrent workloads |

## OS selection

Prefer a current, supported LTS distribution:

- **Ubuntu Server 24.04 LTS** (recommended for broadest compatibility)
- **Debian 12 (bookworm)**
- **Rocky Linux 9** / **AlmaLinux 9**

## Network placement

- Place the VM on a dedicated VLAN or virtual network segment.
- Apply firewall rules at the hypervisor and VM level:
  - Default deny inbound
  - Allow only required outbound ports
  - Restrict SSH to trusted source IPs
- Use a VPN or jump-host for remote access — never expose SSH directly to the internet.

## Storage and backup

- Use a single system partition with LVM for flexibility.
- Schedule daily backups using a tool appropriate to the environment:
  - `rsync` to a NAS or backup server
  - `restic` or `borg` for deduplicated, encrypted backups
  - Cloud snapshot tools where applicable
- Test restore monthly.

## Secrets handling

- Use the OS keyring or a dedicated secret manager.
- Never store secrets in:
  - shell scripts
  - environment files committed to version control
  - prompts or documentation
  - logs or traces
- Rotate credentials on a documented schedule.

## Update and patching model

1. **Monthly**: Apply OS security updates (`apt upgrade` / `dnf update`).
2. **Per release**: Update agent platform components after evaluation tests.
3. **Before any update**: Take a snapshot or backup.
4. **After any update**: Run smoke tests.

## Support access model

- SSH with key-based authentication only.
- Disable password authentication and root login.
- Use a jump-host or VPN for all remote access.
- Log all sessions and retain per client policy.

## Validation checklist

- [ ] VM created with correct sizing band
- [ ] OS installed and hardened
- [ ] Network and firewall configured
- [ ] Secrets store configured
- [ ] Backup schedule active and tested
- [ ] Support access restricted to key-based SSH
- [ ] Monitoring and tracing installed
- [ ] Approval-to-mutation workflow verified
- [ ] Restore-from-backup tested
