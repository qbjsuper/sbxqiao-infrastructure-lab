# SBXQIAO Lab Checkpoint

**Date:** 2026-03-14  
**Stage:** sbx-cl2 Planning  
**Site:** SBX  
**Host:** sbx-cl2

---

## Objective

Plan and document `sbx-cl2` as the second workstation in the SBX site before deployment.

---

## Reason for Deployment

`sbx-cl2` is planned to provide a clean second workstation for Samba validation.

This workstation will be used to:
- validate second-client access to `\\sbx-lx1\Shared`
- verify cross-user file access behaviour
- avoid using `sbx-dc1` as a client test platform
- support future Group Policy and client administration testing

---

## Planned Role

`sbx-cl2` will be a domain-joined Windows client workstation in `SBXQIAO.LAB`.

---

## Planned Network

| Item | Value |
|---|---|
| Network | `172.16.50.0/24` |
| Gateway | `172.16.50.1` |
| DNS | `172.16.50.10` |
| Domain Controller | `sbx-dc1.sbxqiao.lab` |

---

## Planned Validation

After deployment, `sbx-cl2` will be used to verify:
- domain join success
- domain logon success
- Samba share access
- cross-user read behaviour
- cross-user write behaviour

---

## Conclusion

Documentation for `sbx-cl2` has been prepared before deployment.

The next step is to build the workstation and record the actual deployment and validation results.