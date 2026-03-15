# Lab Action Record Template

## Action Title

> Write a short, precise title for the action.

Example:
> Configure `sbx-lx1` as an AD-integrated Samba member server

---

## 1. Action Summary

### Objective
- State exactly what is being changed, fixed, or validated.

### Action Type
- [ ] Standard Change
- [ ] Troubleshooting Change
- [ ] Design Change
- [ ] Validation-Only Action

### Reason
- Explain why this action is required.

### Scope
- List the affected hosts, services, sites, or documents.

Example:
- Host: `sbx-lx1`
- Client: `sbx-cl2`
- Service: Samba
- Domain: `sbxqiao.lab`

---

## 2. Current State

Describe the current environment before the action.

Include:
- what already works
- what is broken or incomplete
- any known constraints
- any known risks

Example:
- `sbx-lx1` is domain-joined
- Samba service is installed
- local access works
- Windows client validation is incomplete

---

## 3. Dependencies

Record what must already be working before this action can succeed.

Check as applicable:
- [ ] IP configuration correct
- [ ] DNS resolution working
- [ ] domain controller reachable
- [ ] time sync healthy
- [ ] credentials available
- [ ] target host reachable
- [ ] previous stable checkpoint exists
- [ ] rollback path prepared

Dependency Notes:
- Record any dependency details here.

---

## 4. Design Impact

Does this action change intended architecture, standards, naming, addressing, or role boundaries?

- [ ] No design change
- [ ] Yes, design documentation updated first

Affected documents:
- list any updated design or standards documents

Example:
- `docs/architecture/network-topology.md`
- `docs/standards/naming-convention.md`

---

## 5. Rollback / Recovery Position

Record the rollback or recovery position before implementation.

Examples:
- Hyper-V checkpoint created
- configuration backup copied
- previous config retained
- snapshot reference recorded

Rollback Details:
- Record exactly what recovery option exists.

---

## 6. Implementation Plan

Write the implementation steps in order before making the change.

1.
2.
3.
4.

Implementation Notes:
- Record the intended order and any cautions.

---

## 7. Implementation Performed

Record what was actually done.

For each meaningful step, note:
- command or GUI action
- target system
- account or privilege used
- important result

Example:
1. Edited `/etc/samba/smb.conf` on `sbx-lx1`
2. Restarted `smbd` and `winbind`
3. Tested share access locally
4. Tested share access from `sbx-cl2`

Actual Change Notes:
- Record differences between the plan and actual work.

---

## 8. Validation Plan

List the tests required to confirm success.

- [ ] service health test
- [ ] DNS test
- [ ] authentication test
- [ ] client access test
- [ ] permissions / security boundary test
- [ ] rollback not required after validation

Validation Steps:
1.
2.
3.
4.

Validation Documents:
- list the validation files that should be updated

Example:
- `docs/operations/validation/linux-ad-integration.md`
- `docs/operations/validation/samba-access.md`

---

## 9. Validation Results

Record what happened during testing.

### Test 1
- Test:
- Expected Result:
- Actual Result:
- Status: Pass / Fail / Partial

### Test 2
- Test:
- Expected Result:
- Actual Result:
- Status: Pass / Fail / Partial

### Test 3
- Test:
- Expected Result:
- Actual Result:
- Status: Pass / Fail / Partial

Validation Summary:
- Give a short operational judgement.

---

## 10. Documentation Updates Required

Record which repo files must be updated after the action.

- [ ] deployment record updated
- [ ] host record updated
- [ ] service record updated
- [ ] validation record updated
- [ ] checkpoint created if stable
- [ ] standards updated if required

Files Updated:
- list the exact file paths

Example:
- `docs/deployment/sbx-build.md`
- `docs/infrastructure/hosts/sbx-lx1.md`
- `docs/operations/validation/samba-access.md`

---

## 11. Known Issues / Follow-Up

Record anything still incomplete.

Examples:
- function works locally but not yet from client
- authentication works but permissions still need tuning
- DNS is correct on Windows but not yet verified on Linux

Known Issues:
- 
- 
- 

Next Actions:
1.
2.
3.

---

## 12. Completion Status

Overall Status:
- [ ] Complete
- [ ] Complete with minor follow-up
- [ ] Partial
- [ ] Failed
- [ ] Validation pending

Checkpoint Required:
- [ ] Yes
- [ ] No

Checkpoint File:
- record file path if created

---

## 13. Record Metadata

- Date:
- Performed By:
- Reviewed By:
- Site:
- Host(s):
- Related Ticket / Work Note: