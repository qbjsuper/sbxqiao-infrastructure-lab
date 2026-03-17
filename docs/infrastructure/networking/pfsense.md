# pfSense

## Purpose

pfSense provides routing, firewall enforcement, NAT, and inter-site VPN connectivity for the lab.

In this lab, pfSense is used to:

- provide the default gateway for each site subnet
- provide WAN egress and outbound NAT
- enforce inter-network firewall policy
- terminate the SBX↔SBY site-to-site IPsec tunnel

## Current deployment

### pfSense-SBX

- WAN: `192.168.0.252`
- LAN: `172.16.50.1/24`
- Protected subnet: `172.16.50.0/24`

### pfSense-SBY

- WAN: `192.168.0.253`
- LAN: `172.16.51.1/24`
- Protected subnet: `172.16.51.0/24`

## SBX↔SBY IPsec

### Purpose

The site-to-site IPsec tunnel connects:

- `172.16.50.0/24` at SBX
- `172.16.51.0/24` at SBY

This is required for:

- inter-site DNS resolution
- domain join from SBY into `sbxqiao.lab`
- domain controller promotion at SBY
- later AD replication between sites

### Current protected networks

- SBX local network: `172.16.50.0/24`
- SBY local network: `172.16.51.0/24`

## Build log

### 2026-03-17 — DNS over IPsec issue during SBY domain join preparation

#### Symptom

From `sby-dc1`, basic IP connectivity to `sbx-dc1` worked, but Active Directory DNS discovery did not fully work at first.

Initial test results from `sby-dc1`:

- `ping 172.16.50.10` — success
- `nslookup sbxqiao.lab 172.16.50.10` — success
- `Test-NetConnection 172.16.50.10 -Port 53` — success
- `nslookup -type=SRV _ldap._tcp.dc._msdcs.sbxqiao.lab 172.16.50.10` — timeout

#### Interpretation

This confirmed that:

- IP routing across the tunnel was working
- at least some DNS traffic was reaching `sbx-dc1`
- the tunnel was not fully down
- the problem was a filtering/ruleset issue rather than a basic routing failure

Because domain join depends on successful AD SRV lookups, the join was paused until DNS discovery worked correctly.

#### Corrective action

Firewall policy was broadened for the inter-site path to allow `any` protocol during bring-up and validation.

#### Result

After broadening the rules, the following query succeeded from `sby-dc1`:

```powershell
nslookup -type=SRV _ldap._tcp.dc._msdcs.sbxqiao.lab 172.16.50.10
The result correctly returned:

sbx-dc1.sbxqiao.lab

172.16.50.10

Conclusion

The fault was a filtering/ruleset issue, not:

a full IPsec tunnel failure

a full DNS service outage on sbx-dc1

a basic Layer 3 reachability problem

Lesson learned

Successful ICMP across a site-to-site tunnel does not prove that Active Directory traffic is fully permitted.

Validation must include:

basic reachability

normal DNS lookups

AD SRV lookups

relevant service/port validation

2026-03-17 — SBY internet access issue traced to missing WAN upstream gateway
Symptom

sby-dc1 had:

valid IP configuration

correct default gateway

working inter-site connectivity

but no internet access

Root cause

pfSense-SBY WAN did not have a valid upstream gateway assigned.

Effect

This meant:

local subnet communication could still work

inter-site traffic across IPsec could still work

but general internet-bound traffic could not be routed upstream

Corrective action

Assigned the correct upstream gateway to the WAN interface on pfSense-SBY.

Result

Internet access was restored for SBY-connected systems.

Lesson learned

A host can have a correct local gateway configured and still fail to reach the internet if the edge router itself has no valid upstream default route.

Current operating approach

During inter-site AD bring-up, firewall policy may remain broader than the final desired state so that:

DNS

AD locator traffic

domain join traffic

DC promotion traffic

replication validation traffic

can pass reliably.

Future hardening task

After the environment is fully stable, review and reduce the inter-site ruleset to the minimum required AD and management traffic set.