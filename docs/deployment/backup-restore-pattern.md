# Backup and restore pattern

## Purpose

This document describes a repeatable backup and restore pattern for client appliance deployments. It applies to all hypervisor patterns in this repository.

## Backup scope

Back up the following:

| What | How often | Notes |
|---|---|---|
| VM snapshot / checkpoint | Before every update | Short-term rollback only |
| VM full image | Weekly | Hypervisor-native export or image-level backup |
| Application data (memory DB, configs, traces) | Daily | Application-consistent backup |
| Runbooks and documentation | On change | Version-controlled or manual copy |
| Secret store metadata | Daily | Not the secrets themselves — only the list of stored entries |

## Backup principles

- **3-2-1 rule**: Three copies, two different media, one off-site.
- **Encryption**: Encrypt backups at rest and in transit.
- **Retention**: Keep daily backups for 30 days, weekly for 90 days, monthly for 1 year.
- **Separation**: Client backups must be logically separated from other clients.

## Restore procedure

### VM-level restore

1. Stop the running appliance VM (if any).
2. Restore the most recent valid VM image snapshot.
3. Verify network connectivity.
4. Start the appliance and wait for platform services to come up.
5. Verify traces are being recorded (Langfuse receiving spans).
6. Verify no test/placeholder data leaked into the production restore.

### Application-data restore

1. Stop application services.
2. Restore memory database (Qdrant / Mem0) from backup.
3. Restore configuration files.
4. Verify secret store is accessible.
5. Restart application services.
6. Run smoke tests.

## Testing schedule

| Test | Frequency | Owner |
|---|---|---|
| VM snapshot restore | Monthly | Delivery engineer |
| Application-data restore | Monthly | Delivery engineer |
| Full disaster recovery drill | Quarterly | Operations |
