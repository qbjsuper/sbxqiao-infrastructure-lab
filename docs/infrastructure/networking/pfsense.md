# pfSense

## Purpose

pfSense provides inter-site routing, firewall enforcement, and WAN/LAN edge services for the lab.

In the current lab design, pfSense is used to:

- provide default gateway services for each site subnet
- provide internet/NAT access where required
- enforce firewall policy between networks
- terminate the site-to-site IPsec tunnel between SBX and SBY

## Current deployment

### SBX pfSense

- Hostname: `pfSense-SBX`
- WAN: `192.168.0.252`
- LAN: `172.16.50.1/24`
- Protected subnet: `172.16.50.0/24`

### SBY pfSense

- Hostname: `pfSense-SBY`
- WAN: `192.168.0.253`
- LAN: `172.16.51.1/24`
- Protected subnet: `172.16.51.0/24`

## Site-to-site IPsec

### Purpose

The SBX↔SBY IPsec tunnel provides routed connectivity between:

- `172.16.50.0/24` at SBX
- `172.16.51.0/24` at SBY

This tunnel is required so that:

- `sby-dc1` can reach `sbx-dc1`
- inter-site DNS queries can work
- domain join can be performed from SBY into the existing `sbxqiao.lab` domain
- later AD replication traffic can pass between sites

### Current Phase 2 intent

The current protected networks are:

- SBX local network: `172.16.50.0/24`
- SBY local network: `172.16.51.0/24`

These selectors are intended to permit routed subnet-to-subnet communication across the tunnel.

## Build note — 2026-03-17

### Issue observed

During preparation to join `sby-dc1` to the `sbxqiao.lab` domain, basic connectivity between the two sites was confirmed, but Active Directory DNS discovery did not fully work at first.

### Environment at time of issue

- Forest: `sbxqiao.lab`
- Domain: `sbxqiao.lab`
- `sbx-dc1`: `172.16.50.10`
- `sby-dc1`: `172.16.51.10`

### Initial test results from `sby-dc1`

The following tests were performed against `sbx-dc1`:

ping 172.16.50.10

Result: success

nslookup sbxqiao.lab 172.16.50.10

Result: success

Test-NetConnection 172.16.50.10 -Port 53

Result: success

nslookup -type=SRV _ldap._tcp.dc._msdcs.sbxqiao.lab 172.16.50.10

Result: timeout

Interpretation

This showed that:

basic routed connectivity across IPsec was working

at least some DNS traffic was reaching sbx-dc1

the failure was not a complete tunnel outage

the problem was more likely a firewall or ruleset scope issue than a routing issue

Because domain join depends on successful AD SRV record discovery, the join was paused.

Corrective action

Firewall policy was broadened to allow any protocol for the current subnet-to-subnet inter-site path during bring-up.

This broad permit approach was used temporarily to remove premature filtering while validating core AD functionality.

Result after firewall change

After broadening the firewall rules, the SRV lookup succeeded:

nslookup -type=SRV _ldap._tcp.dc._msdcs.sbxqiao.lab 172.16.50.10

Returned result:

_ldap._tcp.dc._msdcs.sbxqiao.lab resolved successfully

SRV target: sbx-dc1.sbxqiao.lab

returned IP: 172.16.50.10

Technical conclusion

The issue was a filtering/ruleset problem, not:

a complete IPsec tunnel failure

a total DNS service failure on sbx-dc1

a basic Layer 3 reachability failure

This confirmed an important operating point for the lab:

successful ICMP across a site-to-site tunnel does not prove that Active Directory traffic is fully permitted.

Validation must include:

basic IP reachability

standard DNS lookup

AD SRV lookup

relevant TCP port testing

later replication validation

Current operating approach

During inter-site AD bring-up, firewall rules may remain broader than the final desired state so that:

DNS

AD locator traffic

domain join traffic

replication validation traffic

can pass reliably.

Rule tightening should only happen after:

domain join succeeds

DNS remains stable

replication and AD health checks pass

Future hardening task

After the environment is stable, review and reduce the inter-site rule set to the minimum required ports and protocols for:

DNS

Kerberos

LDAP/LDAPS

SMB/RPC

Global Catalog

AD replication

time synchronisation if required

This hardening should be done only after the working baseline has been fully validated.

### Internet access issue — SBY WAN upstream gateway missing

During validation of `sby-dc1`, the server had a correct local IP configuration:

- IP address assigned
- subnet mask assigned
- default gateway assigned
- DNS server assigned

However, internet access was not working.

The root cause was not the server configuration itself. The issue was that `pfSense-SBY` WAN did not have a valid upstream gateway assigned.

As a result:

- local subnet communication could still work
- inter-site traffic across the IPsec path could still work
- but general internet-bound traffic could not be routed upstream from SBY

After assigning the correct upstream gateway to the WAN interface on `pfSense-SBY`, internet connectivity was restored.

### Technical lesson

A host can have a correct local gateway configured and still fail to reach the internet if the edge router itself has no valid upstream default route.