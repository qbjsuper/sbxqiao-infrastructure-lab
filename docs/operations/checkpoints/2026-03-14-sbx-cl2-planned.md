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

# SBXQIAO Lab Checkpoint

**Date:** 2026-03-14  
**Stage:** sbx-cl2 Join and Samba Validation  
**Site:** SBX  
**Host:** sbx-cl2

---

## Objective

Join `sbx-cl2` to `SBXQIAO.LAB` and validate Samba access from a second SBX workstation.

---

## Current Status

- `sbx-cl2` virtual machine created
- domain join in progress
- Samba validation in progress using a second user context

---

## Validation Goals

- confirm domain join success
- confirm domain logon success
- verify access to `\\sbx-lx1\Shared`
- verify second-client file creation
- verify cross-user file modification behaviour

---

## Notes

This checkpoint should be updated after:
- domain join result is confirmed
- secure channel is verified
- Samba read/write testing is completed
# SBXQIAO Lab Checkpoint

**Date:** 2026-03-14  
**Stage:** sbx-cl2 Join and Samba Validation Complete  
**Site:** SBX  
**Host:** sbx-cl2

---

## Objective

Join `sbx-cl2` to `SBXQIAO.LAB` and validate Samba access from a second SBX workstation.

---

## Completed Work

- `sbx-cl2` virtual machine created
- `sbx-cl2` joined to `SBXQIAO.LAB`
- domain logon completed using a second user context
- Samba share access tested against `\\sbx-lx1\Shared`
- read and write behaviour validated
- cross-user file modification validated

---

## Validation Outcome

The following goals were achieved:
- domain join success
- domain logon success
- second-client Samba access success
- file creation success
- cross-user file modification success

---

## Conclusion

`sbx-cl2` is now operational as the second SBX workstation.

The Samba validation stage for the current SBX environment is complete and passed from a second client and second user context.

---

## Next Step

Proceed with documentation consolidation for the completed SBX stage, then move to confirmed planning work for the SBY site.