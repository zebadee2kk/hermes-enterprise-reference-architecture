# Proxmox appliance deployment blueprint

## Purpose

This blueprint describes a repeatable pattern for deploying the Hermes Enterprise Reference Architecture as a Proxmox VE virtual machine or LXC container. It is intended for delivery engineers scoping an SMB, homelab, or internal deployment.

## Recommended VM sizing

| Band | vCPUs | RAM | Disk | Use case |
|---|---|---|---|---|
| Minimal | 2 | 8 GB | 80 GB | Single-agent, light workflows, evaluation |
| Standard | 4 | 16 GB | 120 GB | Full platform with memory, traces, and multiple workflows |
| Performance | 8 | 32 GB | 200 GB | Multi-agent, heavier models, concurrent workloads |

## Network placement

- Create a dedicated **Linux Bridge** or **VLAN-aware bridge** for the appliance network.
- Assign the appliance to a dedicated VLAN or virtual network segment.
- Use **Proxmox firewall** at the datacenter, host, and VM level:
  - Default deny inbound
  - Allow only required outbound ports
  - Restrict SSH/management to trusted source IPs
- Place a physical or virtual firewall between the appliance segment and production networks.

## Storage and backup

- Use **ZFS** or **Ceph** for redundant local storage where possible.
- Use **thick-provisioned** qcow2 or raw disks for predictable performance.
- Schedule **Proxmox Backup Server (PBS)** snapshots:
  - Daily incremental backups
  - Weekly full backups
  - Monthly off-site sync
- Test restore monthly — a backup that cannot be restored is not a backup.

## Secrets handling

- Do **not** store secrets inside the VM disk image or in Proxmox VM notes.
- Use one of the following:
  - OS-level keyring;
  - HashiCorp Vault or cloud secret manager;
  - Client-approved password manager.
- Rotate credentials on a documented schedule.
- Never place secrets in scripts, prompts, or documentation inside the VM.

## Update and patching model

1. **Monthly**: Apply OS security updates during an approved maintenance window.
2. **Per release**: Update agent platform components after running evaluation tests.
3. **Before any update**: Take a Proxmox snapshot so you can roll back.
4. **After any update**: Run smoke tests before handing back to operations.
5. **After successful validation**: Delete the snapshot within 24-48 hours.

## Support access model

- Enable **SSH** with key-based authentication only (disable password auth).
- Use a **jump-host** or **VPN** — do not expose SSH to the internet.
- Enable **Proxmox web console** access only for named administrators via the Proxmox firewall.
- Log all remote support sessions and retain logs per the client's retention policy.

## Validation checklist

- [ ] VM or LXC created with correct sizing band
- [ ] Bridge and VLAN configured
- [ ] Proxmox firewall rules applied (default deny inbound)
- [ ] IP address assigned and DNS configured
- [ ] OS hardened and patched
- [ ] Secrets store configured and empty of plaintext credentials
- [ ] PBS backup schedule active and tested
- [ ] Support access restricted to named accounts with key-based auth
- [ ] Monitoring and tracing agents installed
- [ ] Approval-to-mutation workflow verified
- [ ] Restore-from-backup tested

## Risks and trade-offs

| Risk | Mitigation |
|---|---|
| Single-host failure | Use Proxmox HA clusters or maintain off-site backups |
| ZFS memory usage | Monitor ARC size; allocate sufficient RAM for both ZFS and workloads |
| Snapshot sprawl | Delete snapshots within 48 hours; rely on PBS for real backups |
| LXC container isolation | Use unprivileged containers; do not use LXC for multi-tenant without additional isolation |
