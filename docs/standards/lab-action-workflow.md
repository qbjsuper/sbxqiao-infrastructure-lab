# Lab Action Workflow Standard

## Purpose

This standard defines the required workflow for all infrastructure actions in the SBXQIAO Infrastructure Lab.

Its purpose is to ensure that every meaningful lab change is:
- planned before implementation
- executed in a controlled order
- validated after implementation
- documented in the repository
- captured in a checkpoint when a stable milestone is reached

This workflow applies to Windows, Linux, networking, Active Directory, DNS, Samba, pfSense, Hyper-V, and future site expansion work.

---

## Core Rule

No meaningful infrastructure action is considered complete until it has been:

1. designed
2. implemented
3. validated
4. documented
5. checkpointed when stable

Build first and document later is not the lab standard.

Documented and validated change is the lab standard.

---

## Workflow Summary

Every meaningful lab action must follow this sequence:

1. define the objective
2. review dependencies and current state
3. update or create design documentation if required
4. prepare rollback or checkpoint position
5. implement the change
6. validate the result
7. update operational documentation
8. record known issues or follow-up actions
9. create a checkpoint record if a stable milestone was reached

---

## Action Categories

This workflow applies differently depending on the type of action.

### Standard Change
A normal planned change to infrastructure.

Examples:
- deploy a new VM
- join a Linux host to the domain
- create a new Samba share
- change DNS settings
- update a validation document

### Troubleshooting Change
A corrective action taken to fix a fault or unexpected behaviour.

Examples:
- fix time sync issues
- repair DNS resolution
- correct Samba authentication
- repair trust or domain join problems

### Design Change
A change that affects architecture, standards, naming, addressing, or role boundaries.

Examples:
- introduce a new site
- change subnet allocation
- revise naming standards
- update admin-tier rules

### Validation-Only Action
An action that does not change infrastructure but verifies current health.

Examples:
- run `dcdiag`
- test Samba access from a client
- verify Linux Kerberos operation
- confirm DNS SRV records

---

## Required Workflow

## 1. Define the Objective

Before making a change, clearly state:

- what is being changed
- why the change is needed
- which systems are affected
- whether the action is planned, corrective, or validation-only

Required output:
- a short objective statement in the relevant document or work note

Example:
> Configure `sbx-lx1` as an AD-integrated Samba member server and validate access from `sbx-cl2`.

---

## 2. Review Dependencies and Current State

Before implementation, confirm the environment is ready.

Check as applicable:
- DNS working
- correct IP settings
- domain controller reachable
- time synchronisation healthy
- credentials available
- target host reachable
- previous checkpoint exists if risk is moderate or high

Questions to answer:
- what must already work before this action can succeed?
- what will break if this dependency is missing?
- what is the rollback point?

Required output:
- dependency note in the deployment record, host record, or validation file

---

## 3. Update or Create Design Documentation if Required

If the action changes intended architecture or standards, update design docs before implementation.

Examples:
- adding a new host
- adding a new subnet
- adding a new service
- changing host role boundaries
- changing naming or account rules

Documents commonly affected:
- `docs/architecture/*`
- `docs/standards/*`
- `docs/infrastructure/sites/*`
- `docs/infrastructure/networking/*`

Rule:
If the design changes, the design documentation must change first.

---

## 4. Prepare Rollback or Checkpoint Position

Before any moderate-risk or high-risk action, prepare a recovery path.

Examples:
- Hyper-V checkpoint before major system reconfiguration
- config backup before service edits
- export of relevant settings before change
- copy of known working config file

Examples of actions that should normally have a rollback position:
- domain join or rejoin
- major Samba configuration change
- DNS service change
- promotion or demotion of domain controller
- firewall or routing change
- authentication stack change on Linux

Required output:
- note rollback or checkpoint reference in deployment or host documentation

---

## 5. Implement the Change

Perform the change in a controlled order.

Rules:
- use the smallest sensible change set
- avoid unrelated changes in the same action
- record commands used where useful
- note service restarts, package installs, configuration file edits, and GUI actions
- keep implementation traceable

Implementation should answer:
- what was changed?
- where was it changed?
- in what order?
- by which account or privilege level?

Required output:
- implementation notes in the relevant deployment, host, or validation document

---

## 6. Validate the Result

Every change must be tested.

Validation must confirm both:
- the intended function works
- no critical dependency was broken

Validation types include:
- service health validation
- authentication validation
- name resolution validation
- client access validation
- security boundary validation
- replication or connectivity validation

Examples:
- after DNS changes, test forward lookup and SRV records
- after Linux AD integration, test Kerberos, `wbinfo`, and `getent`
- after Samba changes, test share access from both Linux and Windows
- after AD changes, test `dcdiag`, DC discovery, and sign-in behaviour

Rule:
A change without validation is incomplete.

Required output:
- update or create a validation record under `docs/operations/validation/`

---

## 7. Update Operational Documentation

After validation, update the repo so that documentation matches reality.

Documents commonly affected:
- `docs/deployment/*`
- `docs/infrastructure/hosts/*`
- `docs/infrastructure/services/*`
- `docs/operations/validation/*`

Minimum documentation updates:
- current state
- key configuration details
- validation result
- known issues
- last reviewed date

Rule:
The repo must describe the actual implemented state, not only the intended design.

---

## 8. Record Known Issues and Follow-Up Actions

If the action is only partially successful, record that clearly.

Examples:
- service working locally but not yet from client
- domain join successful but login not yet validated
- DNS working from Windows but not yet from Linux
- workaround applied pending proper fix

Rules:
- do not mark partial work as complete
- record what works
- record what does not work
- record what should happen next

Required output:
- known issues note in the relevant document
- next actions in deployment journal or checkpoint record

---

## 9. Create a Checkpoint Record When Stable

When a meaningful stable state is reached, create a checkpoint record.

A checkpoint should be created after:
- first successful domain controller build
- Linux host successfully joined to domain
- Samba working from client side
- major DNS or time issues resolved
- new site reaches baseline readiness
- any stage that would be difficult to reconstruct from memory later

Checkpoint records should include:
- stable state reached
- what changed
- what was validated
- open issues still remaining
- next recommended step
- checkpoint date

Checkpoint location:
- `docs/operations/checkpoints/`

Rule:
Stable milestones must be checkpointed.

---

## Required Behaviour by Action Type

## Planned Build Work
Must include:
- objective
- dependency review
- implementation note
- validation
- documentation update
- checkpoint if stable

## Troubleshooting Work
Must include:
- problem statement
- symptom evidence
- suspected cause
- corrective action
- post-fix validation
- documentation update
- checkpoint if a major issue was resolved

## Validation-Only Work
Must include:
- validation scope
- commands or test method
- result
- operational judgement
- remediation note if failed

## Design Work
Must include:
- rationale
- affected standards or architecture docs
- implementation dependency if any
- date of standard change

---

## Documentation Mapping

Use the following repo areas during the workflow.

### Design
Use:
- `docs/architecture/`
- `docs/standards/`

### Build and change history
Use:
- `docs/deployment/`

### Host and service state
Use:
- `docs/infrastructure/hosts/`
- `docs/infrastructure/services/`
- `docs/infrastructure/networking/`
- `docs/infrastructure/sites/`

### Validation evidence
Use:
- `docs/operations/validation/`

### Stable milestone history
Use:
- `docs/operations/checkpoints/`

### Journal / working notes
Use:
- journal or work note areas if maintained separately

Rule:
Working notes may help during the task, but final truth must be written into the repo documentation.

---

## Change Severity Guidance

## Low Risk
Examples:
- minor documentation correction
- non-impacting note update
- adding validation evidence after successful testing

Expected workflow:
- objective
- implement
- validate if relevant
- document

## Moderate Risk
Examples:
- Samba config change
- DNS client config change
- Linux authentication stack change
- firewall rule adjustment

Expected workflow:
- objective
- dependency review
- rollback/checkpoint
- implement
- validate
- document

## High Risk
Examples:
- domain controller promotion or demotion
- AD DNS structural change
- subnet redesign
- major routing change
- identity/authentication redesign

Expected workflow:
- design update first
- dependency review
- rollback/checkpoint
- controlled implementation
- full validation
- documentation update
- checkpoint record

---

## Evidence Standard

Where practical, record:
- command used
- purpose of the command
- important result
- pass/fail judgement
- date tested

Evidence should be:
- concise
- reproducible
- technically meaningful

Do not dump large amounts of raw output without interpretation.

Summarise what the result means operationally.

---

## Completion Criteria

A lab action is complete only when all applicable items below are true:

- objective is clear
- dependencies were reviewed
- implementation was performed
- result was validated
- documentation was updated
- known issues were recorded
- checkpoint was created if a stable milestone was reached

If validation failed, the action is not complete.

If documentation was not updated, the action is not complete.

---

## Standard Operating Rule

For this lab, the required operating model is:

**Design → Prepare → Implement → Validate → Document → Checkpoint**

This is the default workflow for all meaningful technical actions in the environment.

---

## Review Information

- Standard Owner: `Bojie`
- Applies To: `SBXQIAO Infrastructure Lab`
- Status: active
- Last Reviewed: `2026-03-15`


## Action Record Requirement

All meaningful lab actions must be recorded using one of the following templates:

- `templates/lab-action-record-template.md`
- `templates/lab-action-quick-template.md`

Use the full template when:
- the change is moderate risk or high risk
- troubleshooting is complex
- multiple systems are affected
- the action changes design, identity, or service behaviour

Use the quick template when:
- the action is low risk
- the scope is narrow
- the change is easy to validate
- no design change is involved