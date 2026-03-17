# sby-dc1

## Role

`SBY-DC1` is the secondary site domain controller for site SBY in the `sbxqiao.lab` domain.

Current roles:

- Active Directory Domain Services
- DNS Server
- Global Catalog

## Identity

- Hostname: `sby-dc1`
- FQDN: `sby-dc1.sbxqiao.lab`
- Domain: `sbxqiao.lab`
- Site: `SBY`

## Network configuration

- Subnet: `172.16.51.0/24`
- IP address: `172.16.51.10`
- Default gateway: `172.16.51.1`

### Current DNS client settings

- Preferred DNS: `172.16.51.10`
- Alternate DNS: `172.16.50.10`

This was changed after DC promotion so that the server uses its own local DNS service first and `sbx-dc1` as fallback.

## Build history

### Stage 1 — Initial deployment

`sby-dc1` was built as a Windows Server member server in the SBY subnet.

Initial intended purpose:

- join existing domain `sbxqiao.lab`
- later become an additional domain controller for site SBY

### Stage 2 — Domain join preparation troubleshooting

Before the join, testing showed:

- inter-site ping to `sbx-dc1` worked
- normal DNS lookup to `sbx-dc1` worked
- AD SRV lookup initially failed across the tunnel

The issue was traced to firewall/ruleset scope on the inter-site path. After broadening the rules, AD SRV lookup succeeded and domain join could proceed safely.

### Stage 3 — Domain join completed

`sby-dc1` was joined successfully to:

- domain: `sbxqiao.lab`

Post-join verification confirmed:

- domain membership successful
- DC discovery successful
- domain authentication working

### Stage 4 — AD site awareness confirmed

After AD Sites and Services preparation, `sby-dc1` correctly identified its site as:

- `SBY`

This was verified using:

```powershell
nltest /dsgetsite

### Stage 5 — Promoted as additional domain controller

sby-dc1 was promoted as an additional writable domain controller in the existing domain sbxqiao.lab.

Promotion result:

AD DS running

DNS running

Global Catalog enabled

site placement correct: SBY

### Stage 6 — Post-promotion validation

The following post-promotion checks succeeded:

Get-Service NTDS, DNS

Get-ADDomainController -Filter * | Select-Object HostName, Site, IsGlobalCatalog

repadmin /replsummary

repadmin /showrepl

local DNS lookup using 127.0.0.1

local AD SRV lookup using 127.0.0.1

Replication from SBX-DC1 to SBY-DC1 was successful for:

domain partition

configuration partition

schema partition

DomainDnsZones

ForestDnsZones

Current state

sby-dc1 is now a healthy second DC in the lab.

Current confirmed state:

writable DC

DNS server

Global Catalog

site-aware for SBY

replicating with SBX-DC1

able to answer local AD DNS queries

OU placement note

Although a custom OU structure exists under SBX-T0, the DC should remain in the built-in Domain Controllers OU unless DC-specific GPO scope is deliberately redesigned.