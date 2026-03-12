# Architecture Overview
This document provides a high-level summary of the SBXQIAO Infrastructure Lab.
---
## Purpose
The lab is designed to simulate a small enterprise infrastructure environment for learning and practising real operational skills.
Primary focus areas include:
- Active Directory
- DNS
- Hyper-V virtualisation
- firewall and routing
- Linux administration
- Samba
- multi-site design
- validation and troubleshooting
---
## Current Architecture Model
The environment currently uses:
- one forest
- one domain
- multiple sites
- pfSense as the routing and firewall platform
- Hyper-V as the virtualisation platform
---
## Domain Model
- Forest: `sbxqiao.lab`
- Domain: `sbxqiao.lab`
This is a single-forest, single-domain design expanded through site separation rather than additional domains.
---
## Site Model
### SBX
Primary site.
- Subnet: `172.16.50.0/24`
- Gateway: `172.16.50.1`
### SBY
Secondary site.
- Subnet: `172.16.51.0/24`
- Gateway: `172.16.51.1`
---
## Current Stage
### Implemented
- `sbx-dc1` deployed as domain controller and DNS server
- `sbx-lx1` deployed as Linux member server
- Samba integration in progress and under validation
- core site infrastructure for SBX operational
### Planned
- deploy `sbx-dc2` in SBY
- implement multi-site AD mapping
- validate cross-site replication
- expand documentation and validation standards
---
## Design Intent
The lab should be treated as an enterprise-style staged deployment, not just a collection of test VMs.
Each build phase should follow this sequence:
1. design
2. deploy
3. validate
4. document
5. checkpoint
