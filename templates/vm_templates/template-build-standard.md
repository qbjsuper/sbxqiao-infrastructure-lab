# VM Template Build Standard

This document defines the operating standard for VM templates used in the lab.

---

## Principles

- templates must be clean and reusable
- templates must not contain site-specific production identities
- templates should be updated before reuse when practical
- templates should be checkpointed after major baseline preparation

---

## Workflow

1. build base VM
2. update operating system
3. install baseline tools
4. validate boot and access
5. create template checkpoint
6. convert to reusable template standard
7. deploy cloned VM from template
8. specialise cloned VM for actual role

---

## Documentation Rule

Every infrastructure host should record:

- source template
- deployment method
- hostname
- IP address
- assigned role
- validation state