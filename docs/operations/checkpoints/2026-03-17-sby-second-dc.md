# Checkpoint — 2026-03-17 — SBY second domain controller established

## Summary

The lab has now reached a functional **two-site Active Directory state**.

`SBY-DC1` was successfully:

- joined to the existing `sbxqiao.lab` domain
- mapped to site `SBY`
- promoted as an additional writable domain controller
- configured as a DNS server
- confirmed as a Global Catalog
- validated for inbound replication from `SBX-DC1`

This marks the completion of the initial SBY AD bring-up.

---

## Current domain state

- Forest: `sbxqiao.lab`
- Domain: `sbxqiao.lab`

## Active Directory sites

- `SBX` → `172.16.50.0/24`
- `SBY` → `172.16.51.0/24`

## Domain controllers

### SBX-DC1

- Hostname: `sbx-dc1`
- FQDN: `sbx-dc1.sbxqiao.lab`
- IP: `172.16.50.10`
- Site: `SBX`
- Role: writable DC, DNS, Global Catalog

### SBY-DC1

- Hostname: `sby-dc1`
- FQDN: `sby-dc1.sbxqiao.lab`
- IP: `172.16.51.10`
- Site: `SBY`
- Role: writable DC, DNS, Global Catalog

---

## Network and routing state

### SBX

- pfSense-SBX WAN: `192.168.0.252`
- pfSense-SBX LAN: `172.16.50.1`
- Protected subnet: `172.16.50.0/24`

### SBY

- pfSense-SBY WAN: `192.168.0.253`
- pfSense-SBY LAN: `172.16.51.1`
- Protected subnet: `172.16.51.0/24`

### Inter-site VPN

SBX and SBY are connected by a site-to-site IPsec tunnel protecting:

- `172.16.50.0/24`
- `172.16.51.0/24`

This tunnel is currently functioning for:

- inter-site ICMP
- inter-site DNS
- domain join traffic
- domain controller promotion traffic
- AD replication traffic

---

## Work completed

### 1. SBY network baseline established

`SBY-DC1` was configured with:

- IP: `172.16.51.10`
- Gateway: `172.16.51.1`
- Initial DNS: `172.16.50.10`

This provided the required baseline to join the existing domain across the inter-site tunnel.

### 2. IPsec filtering issue identified during AD DNS testing

Initial behaviour from `sby-dc1`:

- `ping 172.16.50.10` succeeded
- `nslookup sbxqiao.lab 172.16.50.10` succeeded
- `Test-NetConnection 172.16.50.10 -Port 53` succeeded
- `nslookup -type=SRV _ldap._tcp.dc._msdcs.sbxqiao.lab 172.16.50.10` timed out

This showed:

- basic routed connectivity existed
- at least some DNS traffic existed
- AD-specific DNS discovery was still blocked

### 3. Inter-site firewall policy broadened for bring-up

The inter-site ruleset was broadened to permit `any` protocol during the validation phase.

After that change, this succeeded from `sby-dc1`:

```powershell
nslookup -type=SRV _ldap._tcp.dc._msdcs.sbxqiao.lab 172.16.50.10