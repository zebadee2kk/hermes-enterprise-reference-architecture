# Remote support access pattern

## Purpose

This document describes a secure remote support access pattern for client appliance deployments. It applies to all hypervisor patterns in this repository.

## Access model principles

- **No direct internet exposure** of management interfaces.
- **Encrypted tunnels only** (VPN, Tailscale, WireGuard).
- **Named individuals only** — no shared accounts.
- **Just-in-time access** — enable for a session, disable after.
- **Full session logging** for audit.

## Recommended approach

### 1. VPN / tunnel setup

Use one of the following:

| Option | Notes |
|---|---|
| WireGuard | Simple, fast, modern. Good for point-to-point tunnels. |
| Tailscale | Mesh VPN with ACLs. Good for multi-client MSP use. |
| OpenVPN | Widely supported. Good for existing VPN infrastructure. |
| IPsec site-to-site | Good for enterprise clients with existing VPN infrastructure. |

### 2. Jump-host pattern

For clients who do not want a full VPN:

1. Deploy a small jump-host (VM or Raspberry Pi) on the client network.
2. Support engineers connect to the jump-host via SSH.
3. From the jump-host, they access the appliance via its internal IP.
4. The jump-host is the only device that needs inbound access from the support team.

### 3. Session logging

- Log all SSH sessions (commands, timestamps, user IDs).
- Forward logs to the client's SIEM or log retention system if required.
- Retain logs per the client's retention policy (minimum 90 days recommended).

## Access request workflow

1. Support engineer requests access via the ticketing system.
2. Client approver grants access for a defined time window.
3. VPN credentials or jump-host access is provisioned.
4. Engineer performs the work.
5. Access is revoked after the session or when the time window expires.
6. Session log is attached to the ticket.

## Hard rules

- Never share credentials between support engineers.
- Never leave persistent backdoors or unapproved access paths.
- Never access client systems without an approved ticket or request.
- Never export client data during a support session unless explicitly approved.
