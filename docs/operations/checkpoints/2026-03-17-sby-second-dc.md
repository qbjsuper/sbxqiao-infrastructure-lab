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
This confirmed:

sbx-dc1 could now be located by AD SRV record lookup

DNS discovery required for domain join and promotion was working

4. SBY internet access issue resolved

A separate issue was found where SBY hosts lacked internet access even though local and inter-site addressing were correct.

Root cause:

pfSense-SBY WAN had no valid upstream gateway assigned

Fix:

assigned the correct upstream WAN gateway on pfSense-SBY

Result:

internet access from SBY was restored

5. sby-dc1 joined to the domain

SBY-DC1 was joined successfully to:

sbxqiao.lab

Post-join verification confirmed:

domain membership working

domain authentication working

domain controller discovery working

Verification outcome included:

whoami returned sbxqiao\administrator

systeminfo showed Domain: sbxqiao.lab

nltest /dsgetdc:sbxqiao.lab returned SBX-DC1

6. AD Sites and Services prepared correctly

After site and subnet preparation, sby-dc1 identified as:

SBY

Verified with:

nltest /dsgetsite

Output:

SBY

7. sby-dc1 promoted as additional domain controller

SBY-DC1 was promoted as an additional writable domain controller in sbxqiao.lab.

Promotion result:

AD DS installed and running

DNS installed and running

Global Catalog enabled

site placement correct: SBY

8. DNS client settings updated after promotion

After promotion, sby-dc1 DNS client settings were adjusted to:

Preferred DNS: 172.16.51.10

Alternate DNS: 172.16.50.10

This allows sby-dc1 to use its own DNS service first while retaining sbx-dc1 as fallback.

9. Post-promotion DNS validation completed

Local DNS on sby-dc1 was verified through loopback.

Successful local tests included:

nslookup sbxqiao.lab 127.0.0.1

and

nslookup -type=SRV _ldap._tcp.dc._msdcs.sbxqiao.lab 127.0.0.1

Results confirmed:

the local DNS service is working

the AD zone is loaded

SRV records exist for both DCs:

sby-dc1.sbxqiao.lab

sbx-dc1.sbxqiao.lab

10. Replication validated

Replication checks confirmed that SBY-DC1 is receiving inbound replication successfully from SBX-DC1.

Naming contexts confirmed as successful:

DC=sbxqiao,DC=lab

CN=Configuration,DC=sbxqiao,DC=lab

CN=Schema,CN=Configuration,DC=sbxqiao,DC=lab

DC=DomainDnsZones,DC=sbxqiao,DC=lab

DC=ForestDnsZones,DC=sbxqiao,DC=lab

repadmin /replsummary showed zero failures.

Current confirmed health state
sby-dc1

Confirmed as:

writable DC

DNS server

Global Catalog

correctly site-aware for SBY

replicating successfully with SBX-DC1

sbx-dc1

Confirmed as:

writable DC

DNS server

Global Catalog

correctly site-aware for SBX

Directory topology

The domain now has:

two writable domain controllers

two AD sites

working inter-site replication

working local DNS service at both sites

This is the first stable multi-site AD checkpoint.

Important design notes
Domain controller OU placement

Although a custom tiering structure exists under SBX-T0, domain controller computer objects should remain in the built-in:

OU=Domain Controllers

unless DC-specific GPO scope and inheritance are deliberately redesigned.

DNS delegation during promotion

The GUI promotion wizard showed:

Update DNS Delegation: No

This was expected and correct, because the task was:

adding an additional DC to the existing domain sbxqiao.lab

and not:

creating a child domain requiring delegation from a parent DNS zone

Firewall posture

The inter-site ruleset is currently broader than the likely final hardened state.

This is intentional during bring-up so that:

AD DNS discovery

promotion

replication

troubleshooting

can be validated without premature filtering.

Hardening should be done only after full stability is confirmed.

Lessons learned

Successful ICMP across IPsec does not prove that AD traffic is fully permitted.

Simple DNS zone lookup success does not prove that AD SRV discovery is working.

A server can have correct local IP, gateway, and DNS settings but still lack internet access if the edge firewall/router has no upstream gateway.

AD Sites and Subnets must be configured explicitly; otherwise systems fall back to Default-First-Site-Name.

Post-promotion DNS client settings must be updated so a DC uses itself appropriately.

Current next-step candidates

The following tasks are logical next actions:

run broader dcdiag validation on both DCs

validate DNS health more thoroughly

review Sites and Services replication settings and site links

decide DHCP service placement for SBY

document recovery and operational standards for the two-site design

begin adding more site-local systems that use SBY-DC1 as preferred DNS

Snapshot conclusion

As of this checkpoint, the lab is no longer a single-DC environment.

It is now a functioning two-site, two-DC Active Directory lab with:

site-aware topology

local DNS service at both sites

working inter-site replication

working IPsec connectivity between sites