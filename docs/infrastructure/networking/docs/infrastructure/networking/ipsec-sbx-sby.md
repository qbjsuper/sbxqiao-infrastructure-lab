# SBX–SBY IPsec Site-to-Site Build

## Purpose

This document defines the first-pass site-to-site IPsec design between the SBX and SBY lab sites.

The goal of this stage is to establish working routed connectivity between:

- `172.16.50.0/24` at SBX
- `172.16.51.0/24` at SBY

This stage is a **network transport build first**. It is not yet a full multi-site Active Directory service expansion.

## Current Design Context

### Physical host layer

The physical Hyper-V hosts remain in **workgroup** and are not joined to `sbxqiao.lab`.

Hosts:

- `QIAO-AU` — primary Hyper-V host
- `BOJIE_MS_A2` — secondary Hyper-V host

This separation is intentional. Only the lab virtual machines participate in the domain.

### Site status before IPsec

#### SBX

- LAN subnet: `172.16.50.0/24`
- pfSense LAN IP: `172.16.50.1`
- Domain controller: `sbx-dc1`
- Current services on `sbx-dc1`:
  - Active Directory Domain Services
  - DNS
  - DHCP

#### SBY

- LAN subnet: `172.16.51.0/24`
- pfSense LAN IP: `172.16.51.1`
- Domain controller: `sby-dc1`
- `sby-dc1` is deployed but not yet configured
- `pfSense-SBY` is installed and basic WAN/LAN connectivity is being prepared

## Underlay / WAN Transport

### WAN addressing

The site-to-site IPsec tunnel runs over the home-router underlay network:

- underlay network: `192.168.0.0/24`
- home router / gateway: `192.168.0.1`

WAN IP assignments:

- `pfSense-SBX` WAN: `192.168.0.252`
- `pfSense-SBY` WAN: `192.168.0.253`

### Underlay validation outcome

The following was confirmed before IPsec configuration:

- `192.168.0.252` can reach `192.168.0.1`
- `192.168.0.253` can reach `192.168.0.1`
- WAN mask, gateway, and Hyper-V external vSwitch attachment are correct
- peer WAN-to-WAN ping initially failed because inbound ICMP was blocked on the WAN interface
- temporary WAN ICMP allow rules were added to permit peer testing
- after the rule change, `192.168.0.252` and `192.168.0.253` could ping each other

This confirms the underlay path is working and IPsec build can proceed.

## IPsec Design Scope

### Protected networks

The first-pass tunnel protects only the site LANs:

- SBX local network: `172.16.50.0/24`
- SBY remote network: `172.16.51.0/24`

On the reverse side:

- SBY local network: `172.16.51.0/24`
- SBX remote network: `172.16.50.0/24`

### VPN model

This implementation uses:

- site-to-site IPsec
- IKEv2
- policy-based subnet-to-subnet tunnel
- mutual pre-shared key authentication

## Phase 1 Standard

The following Phase 1 baseline is used on both sites.

- Key Exchange version: `IKEv2`
- Internet Protocol: `IPv4`
- Authentication method: `Mutual PSK`
- My identifier: `My IP address`
- Peer identifier: `Peer IP address`
- Encryption: `AES-256`
- Hash: `SHA-256`
- Diffie-Hellman group: `14`
- NAT Traversal: automatic / default
- Lifetime: pfSense default unless changed during future tuning

### Phase 1 peer definitions

#### SBX Phase 1

- interface: `WAN`
- local peer: `192.168.0.252`
- remote gateway: `192.168.0.253`

Suggested description:

- `SBX-to-SBY-P1`

#### SBY Phase 1

- interface: `WAN`
- local peer: `192.168.0.253`
- remote gateway: `192.168.0.252`

Suggested description:

- `SBY-to-SBX-P1`

## Phase 2 Standard

The following Phase 2 baseline is used on both sites.

- Mode: `Tunnel IPv4`
- Protocol: `ESP`
- Encryption: `AES-256`
- Hash: `SHA-256`
- PFS group: `14`
- Lifetime: pfSense default unless changed during future tuning

### Phase 2 selectors

#### SBX Phase 2

- local network: `172.16.50.0/24`
- remote network: `172.16.51.0/24`

Suggested description:

- `SBX-LAN-to-SBY-LAN`

#### SBY Phase 2

- local network: `172.16.51.0/24`
- remote network: `172.16.50.0/24`

Suggested description:

- `SBY-LAN-to-SBX-LAN`

## Firewall Rule Model

### WAN rules

Temporary WAN ICMP rules were used only to verify underlay peer reachability between the two pfSense WAN addresses.

These rules are testing aids and should not be treated as the long-term application traffic policy.

For normal pfSense IPsec operation, the tunnel establishment traffic is handled by pfSense when IPsec is enabled and correctly configured.

### IPsec rules

Traffic that passes through the established tunnel is controlled on:

- `Firewall > Rules > IPsec`

Initial testing rule set should be minimal.

Recommended first test rule on both firewalls:

- Action: `Pass`
- Interface: `IPsec`
- Address family: `IPv4`
- Protocol: `ICMP`
- Source: `any`
- Destination: `any`

Suggested description:

- `Allow ICMP over IPsec for testing`

This permits first-pass validation without opening unnecessary application traffic.

## Validation Plan

### Stage 1 — tunnel establishment

Confirm that:

- Phase 1 comes up
- Phase 2 comes up
- Security Associations appear in `Status > IPsec`

### Stage 2 — gateway-to-gateway test

Test:

- `172.16.50.1` to `172.16.51.1`
- `172.16.51.1` to `172.16.50.1`

This verifies basic encrypted transport between both site firewalls.

### Stage 3 — inside host test

After gateway tests succeed, test site host connectivity, for example:

- `sbx-dc1` to `sby-dc1`

At this stage, do not assume domain service functionality yet. This only verifies IP transport.

## Operational Notes

- This is the first-pass transport build only
- `sby-dc1` is not yet considered a configured site service node
- multi-site AD validation comes after stable IPsec transport
- any broad WAN allow rules should be avoided
- use narrow, explicit rules for testing and future service enablement

## Next Stage After Successful IPsec

After stable IPsec transport is verified, the next major tasks are expected to be:

1. configure `sby-dc1`
2. validate DNS reachability between sites
3. define AD Sites and Services mapping
4. validate replication path only after site service readiness is complete