# VMware appliance deployment blueprint

## Purpose

This blueprint describes a repeatable pattern for deploying the Hermes Enterprise Reference Architecture as a VMware vSphere / ESXi virtual machine. It is intended for delivery engineers scoping an SMB or internal deployment.

## Recommended VM sizing

| Band | vCPUs | RAM | Disk | Use case |
|---|---|---|---|---|
| Minimal | 2 | 8 GB | 80 GB | Single-agent, light workflows, evaluation |
| Standard | 4 | 16 GB | 120 GB | Full platform with memory, traces, and multiple workflows |
| Performance | 8 | 32 GB | 200 GB | Multi-agent, heavier models, concurrent workloads |

## Network placement

- Create a **port group** on a dedicated VLAN for the appliance.
- Use a **distributed virtual switch** (VDS) where available for consistent policy.
- Apply **security policies** on the port group:
  - Reject MAC address changes
  - Reject forged transmits
  - Enable promiscuous mode only if required for monitoring
- Place a firewall or NSX-T segment between the appliance VLAN and production networks.

## Storage and backup

- Use **thick-provisioned eager-zeroed** VMDKs for predictable performance.
- Place VMDKs on redundant datastores (RAID1/RAID5/RAID6 or vSAN).
- Use **vSphere snapshots** only for short-term rollback before updates — not as backups.
- Use a proper backup tool (Veeam, Commvault, or equivalent) for daily backups.
- Test restore monthly.

## Secrets handling

- Do **not** store secrets inside the VM or in VM notes/annotations.
- Use one of the following:
  - OS-level keyring;
  - HashiCorp Vault or cloud secret manager;
  - Client-approved password manager.
- Rotate credentials on a documented schedule.
- Never place secrets in scripts, prompts, or documentation inside the VM.

## Update and patching model

1. **Monthly**: Apply OS security updates during an approved maintenance window.
2. **Per release**: Update agent platform components after running evaluation tests.
3. **Before any update**: Take a vSphere snapshot so you can roll back.
4. **After any update**: Run smoke tests before handing back to operations.
5. **After successful validation**: Delete the snapshot within 24-48 hours.

## Support access model

- Enable **SSH** (Linux) or **RDP** (Windows) restricted to named support accounts.
- Use a **jump-host** or **VPN** — do not expose management interfaces to the internet.
- Enable **vSphere console access** only for named administrators.
- Log all remote support sessions and retain logs per the client's retention policy.

## Validation checklist

- [ ] VM created with correct sizing band
- [ ] Port group and VLAN configured
- [ ] Security policies applied to port group
- [ ] IP address assigned and DNS configured
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
| Host resource contention | Set CPU/RAM reservations; monitor via vCenter |
| Snapshot sprawl | Delete snapshots within 48 hours; use proper backup tools |
| vCenter dependency | Ensure vCenter is highly available or document offline recovery |
| Licensing | Confirm VMware licences and guest OS licences are in place |
