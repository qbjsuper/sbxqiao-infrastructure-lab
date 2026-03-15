# Documentation Standard

## Purpose

This document defines how infrastructure documentation is written, placed, and maintained in the SBXQIAO Infrastructure Lab repository.

The goal is to ensure documentation remains consistent, readable, and operationally useful as the environment grows.

---

## Documentation Principles

Documentation should follow these principles:

- accuracy over completeness
- simple structure
- reproducible infrastructure
- operational usefulness
- clear separation between design and implementation

Documentation must reflect the actual running infrastructure.

---

## File Categories

### Architecture
High-level system design.

Examples:
- network topology
- Active Directory design
- DNS architecture
- virtualization platform design

Location:
- `docs/architecture/`

### Infrastructure
Records of real deployed infrastructure.

Examples:
- sites
- hosts
- networking devices
- infrastructure services

Location:
- `docs/infrastructure/`

### Deployment
Step-by-step procedures used to build infrastructure.

Examples:
- initial domain deployment
- site build procedures
- service installation steps

Location:
- `docs/deployment/`

### Operations
Operational records used after deployment.

Examples:
- validation checks
- troubleshooting procedures
- system checkpoints

Location:
- `docs/operations/`

### Standards
Rules governing how the infrastructure is designed and documented.

Examples:
- naming conventions
- IP addressing
- administrative tiering
- documentation standards
- workflow standards

Location:
- `docs/standards/`

---

## Host Documentation Format

Every host document should contain:

- Host Identity
- Operating System
- Role
- Network Configuration
- Template Source
- Deployment Purpose
- Validation Checks
- Notes

---

## Site Documentation Format

Every site document should contain:

- Site Identity
- Subnet
- Gateway
- Infrastructure Hosts
- Purpose
- Notes

---

## Template Documentation

Template documents define the baseline configuration for VM templates used in the environment.

Templates must remain clean baseline systems and must not contain production configuration.

---
## Template Use Rule

Templates exist only to help create recurring documentation types used in `docs/`.

Approved template categories:
- host records
- site records
- validation records
- VM baseline templates where applicable

Templates must not duplicate workflow standards or create a second documentation layer.

If a template is not being used regularly, it should be removed.

## Operational Update Rule

---

Documentation must be updated when:

- a new host is deployed
- a service is installed
- a network change occurs
- an architecture decision changes

---

## Workflow Reference

Lab workflow rules are defined in:

- `docs/standards/lab-action-workflow.md`

This file defines where documentation belongs and how it should be structured.