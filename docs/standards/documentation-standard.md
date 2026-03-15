# Documentation Standard

## Purpose

This document defines how infrastructure documentation is written and maintained in the SBXQIAO Infrastructure Lab repository.

The goal is to ensure documentation remains consistent, readable, and operationally useful as the environment grows.

---

## Documentation Principles

Documentation should follow these principles:

- accuracy over completeness
- simple structure
- reproducible infrastructure
- operational usefulness
- clear separation between design and implementation

Documentation must reflect the **actual running infrastructure**.

---

## File Categories

The repository separates documentation into several categories.

### Architecture

High-level system design.

Examples:

- network topology
- Active Directory design
- DNS architecture
- virtualization platform design

Location:


docs/architecture/


---

### Infrastructure

Records of real deployed infrastructure.

Examples:

- sites
- hosts
- networking devices
- infrastructure services

Location:


docs/infrastructure/


---

### Deployment

Step-by-step procedures used to build the infrastructure.

Examples:

- initial domain deployment
- site build procedures
- service installation steps

Location:


docs/deployment/


---

### Operations

Operational procedures used after deployment.

Examples:

- validation checks
- troubleshooting procedures
- system checkpoints

Location:


docs/operations/


---

### Standards

Rules governing how the infrastructure is designed and operated.

Examples:

- naming conventions
- IP addressing
- administrative tiering
- documentation standards

Location:


docs/standards/


---

## Host Documentation Format

Every infrastructure host document should contain the following sections:


Host Identity
Operating System
Role
Network Configuration
Template Source
Deployment Purpose
Validation Checks
Notes


This ensures every server record contains the information required to rebuild or troubleshoot the system.

---

## Site Documentation Format

Site documents should include:


Site Identity
Subnet
Gateway
Infrastructure Hosts
Purpose
Notes


---

## Template Documentation

Template documents define the baseline configuration for VM templates used in the environment.

Templates must remain **clean baseline systems** and must not contain production configuration.

---

## Operational Updates

Documentation must be updated when:

- a new host is deployed
- a service is installed
- a network change occurs
- architecture decisions change

---

## Design Goal

Documentation in this repository should allow a new administrator to understand and rebuild the environment with minimal external explanation.

## Workflow Requirement

All meaningful infrastructure actions must follow the lab action workflow standard defined in:

- `docs/standards/lab-action-workflow.md`

Required sequence:
- design
- prepare
- implement
- validate
- document
- checkpoint when stable

Documentation is not optional after technical work.
Documentation is part of the technical work.

## Action Recording Standard

Every meaningful infrastructure action must have a written action record.

Approved templates:
- `templates/lab-action-record-template.md`
- `templates/lab-action-quick-template.md`

The action record must exist before the work is considered fully complete.

