# Build Order

This document records the intended infrastructure deployment order for the SBXQIAO Infrastructure Lab.

The goal is to keep the build sequence simple, staged, and reproducible.

---

## Build Principles

The build order follows these principles:
- core services first
- validate each stage before moving forward
- keep infrastructure dependencies clear
- document the actual running environment
- avoid introducing new complexity before the current phase is stable

---

## Current Lab State

The lab currently operates as a single forest and single domain environment:

- Forest: `SBXQIAO.LAB`
- Domain: `SBXQIAO.LAB`

The environment is being built as a multi-site lab.

Current known sites:
- `SBX` — primary site
- `SBY` — secondary site

At the current stage, the active implementation focus is the `SBX` site.

---

## Phase 1 — Core Site Foundation

### Objective
Establish the primary site with routing, Active Directory, and DNS.

### Build Order
1. Deploy pfSense for SBX site routing and gateway services
2. Deploy `sbx-dc1`
3. Configure Active Directory Domain Services
4. Configure DNS
5. Validate domain controller health
6. Validate DNS resolution
7. Validate basic client-to-domain communication

### Outcome
This phase creates the minimum core infrastructure required for domain-based services.

---

## Phase 2 — Linux Member Services

### Objective
Introduce Linux member-server integration and file services.

### Build Order
1. Deploy `sbx-lx1`
2. Configure network settings
3. Join `sbx-lx1` to `SBXQIAO.LAB`
4. Validate AD member-server integration
5. Enable SSH for remote administration
6. Configure Samba file services
7. Validate domain-authenticated Samba access
8. Validate Linux-side permissions and service stability

### Outcome
This phase establishes Linux integration and shared-file service capability inside the SBX site.

---

## Phase 3 — Client Validation Layer

### Objective
Add client systems to validate real user access and service behaviour.

### Build Order
1. Validate `sbx-cl1` as the first Windows client workstation
2. Deploy `sbx-cl2`
3. Join `sbx-cl2` to `SBXQIAO.LAB`
4. Validate domain logon from `sbx-cl2`
5. Validate access to `\\sbx-lx1\Shared`
6. Validate second-client read and write access
7. Validate cross-user Samba behaviour

### Outcome
This phase confirms that the environment works from the user side, not only from the server side.

---

## Phase 4 — Service Stabilisation and Documentation

### Objective
Stabilise the running services and bring documentation into line with the actual environment.

### Build Order
1. Finalise infrastructure host records
2. Finalise service records
3. Record validation outcomes
4. Record checkpoints and operational notes
5. Review build dependencies and unresolved issues

### Outcome
This phase ensures the documented state matches the real implemented state.

---

## Phase 5 — Secondary Site Planning

### Objective
Prepare for expansion beyond the primary site.

### Current Position
A secondary site named `SBY` exists in the design, but detailed implementation work should only begin after the SBX site is stable and fully documented.

### Planned Direction
- define SBY deployment objectives
- confirm subnet and gateway usage
- define host roles for SBY
- define replication and site-service expectations
- plan secondary domain controller deployment

### Note
Detailed SBY design should be confirmed before build work begins.

---

## Current Priority

The current priority order is:

1. keep `sbx-dc1` stable
2. keep `sbx-lx1` stable
3. join and validate `sbx-cl2`
4. complete SBX documentation
5. move to confirmed SBY planning

---

## Validation Rule

No major new build phase should begin until the previous phase has been validated and documented.

This rule exists to keep the lab reproducible and easier to troubleshoot.

---

## Notes

This file records deployment order only.

Detailed host configuration belongs in:
- `docs/infrastructure/hosts/`

Detailed service records belong in:
- `docs/infrastructure/services/`

Operational testing and checkpoints belong in:
- `docs/operations/`

---

## SBY Site Build Order

1. Confirm SBY design records are current in the repo
2. Define `sby-dc1` planned state
3. Create the `SBY-DC1` VM
4. Install the operating system
5. Set hostname to `sby-dc1`
6. Configure static IP, gateway, DNS, and suffix
7. Apply updates and confirm stable local admin access
8. Verify network connectivity to `sbx-dc1`
9. Verify DNS resolution for the domain and `sbx-dc1`
10. Confirm time is synchronised closely enough for Kerberos
11. Join the server to `sbxqiao.lab`
12. Promote `sby-dc1` to additional domain controller and DNS server
13. Validate AD DS health, DNS health, and replication
14. Validate site/subnet mapping
15. Update documentation immediately after the build reaches a stable state