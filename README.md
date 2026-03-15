# SBXQIAO Infrastructure Lab
Enterprise-style infrastructure lab built on Hyper-V to practise Windows Server, Linux administration, networking, and multi-site Active Directory operations.
---
## Current Design State
The lab currently uses a single forest and single domain design.
- Forest: `sbxqiao.lab`
- Domain: `sbxqiao.lab`
The environment is being developed as a multi-site lab.
### Sites
- **SBX** — Primary site
- **SBY** — Secondary site

### Core Services
- Active Directory Domain Services
- DNS
- pfSense routing and firewall
- Linux AD member integration
- Samba file services
- Time synchronisation validation
---
## Current Infrastructure

### SBX Site

- Subnet: `172.16.50.0/24`
- Gateway: `172.16.50.1`
- `sbx-dc1` — Domain Controller / DNS
- `sbx-lx1` — Linux member server / Samba
- `sbx-cl2` — Windows client workstation / Samba validation

### SBY Site

- Subnet: `172.16.51.0/24`
- Gateway: `172.16.51.1`
- `sbx-dc2` — Planned additional Domain Controller / DNS

---
## Repository Structure
- `docs/architecture/` — design documents
- `docs/infrastructure/` — implemented sites, hosts, and services
- `docs/deployment/` — build order and deployment runbooks
- `docs/operations/` — validation, checkpoints, and troubleshooting
- `templates/` — VM templates and documentation templates
- `journal/` — operational journal and checkpoints

---
## Build Philosophy
This repository is organised to support:
- template-based VM deployment
- staged infrastructure rollout
- validation after each phase
- repeatable administration practice
- clear separation between design, implementation, and operations

---

## Lab Operating Workflow

All meaningful lab actions follow this standard workflow:

**Design → Prepare → Implement → Validate → Document → Checkpoint**

See:
- `docs/standards/lab-action-workflow.md`

### Before implementation
- review the current relevant documentation
- confirm the planned action matches the documented design
- create missing planning, build, validation, or checkpoint records if required

### After implementation
- update the relevant infrastructure record
- update the relevant deployment or operations record
- record the validation outcome
- record any remaining issues or follow-up actions
- create a checkpoint when the environment reaches a stable state

### Documentation standard
The documentation must reflect the real implemented environment, not only the intended plan.

Infrastructure work is not considered fully complete until the related documentation is updated.
