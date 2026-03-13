Checkpoint: Infrastructure Documentation Baseline
Stage: Program B v1.2
Status: Stable

**Date:** 2026-03-13  
**Stage:** Samba AD Integration Stabilisation Complete  
**Site:** SBX  
**Host:** sbx-lx1  
**Domain:** SBXQIAO.LAB

---

## Objective

Stabilise Samba Active Directory integration on the Ubuntu member server and restore reliable domain-authenticated file share access before proceeding to second-site design work.

---

## Environment

| Component | Value |
|---|---|
| Server | sbx-lx1 |
| Role | Ubuntu member server |
| Domain | SBXQIAO.LAB |
| AD DC | sbx-dc1 |
| Share Path | /srv/samba/shared |
| Share Name | Shared |

---

## Initial Problem Statement

Samba share access was failing for normal domain users.

Authentication behaviour was inconsistent during earlier troubleshooting because the Linux server had overlapping identity and authentication paths through both `sssd` and `winbind`.

After identity-path cleanup, Samba authentication succeeded, but share use still failed with `NT_STATUS_ACCESS_DENIED` during file operations.

This narrowed the issue from broad AD and Samba failure to Linux filesystem authorisation on the backing directory.

---

## Work Completed

### 1. Active Directory member state validation

Confirmed that the Ubuntu server remained joined to the domain and operating as a Samba domain member.

Validated:
- `net ads testjoin` succeeded
- Samba reported `ROLE_DOMAIN_MEMBER`
- `secrets.tdb` remained present and readable by root

### 2. Identity model cleanup

Removed overlapping AD client paths and standardised the server on Samba and winbind for this member-server design.

Changes completed:
- `sssd` disabled
- `nsswitch.conf` aligned to use `winbind`
- PAM updated so:
  - Winbind NT/Active Directory authentication is enabled
  - SSS authentication is disabled

### 3. Identity resolution validation

Confirmed that AD identities and groups resolved correctly through winbind.

Validated:
- domain user lookup succeeded
- domain group lookup succeeded

### 4. Samba authentication validation

Confirmed that normal domain users could authenticate successfully to Samba.

Validated:
- share enumeration succeeded
- connection to `//localhost/Shared` succeeded
- authentication using domain credentials succeeded

### 5. Linux filesystem authorisation repair

Inspected `/srv/samba/shared` ownership, permissions, and ACL state.

Applied corrections:
- group ownership aligned to `SBXQIAO\Domain Users`
- directory permissions set to `2770`
- share ACL added for `SBXQIAO\Domain Users`
- default ACL added for inherited access on child objects

### 6. SSH service enablement

Enabled OpenSSH on `sbx-lx1` to restore stable remote administration during troubleshooting.

Validated:
- SSH service started successfully
- SSH access from the management client succeeded

### 7. End-to-end Samba validation

Validated successful share use from both server-side testing and Windows client testing.

Server-side validation:
- `ls` within share succeeded
- directory creation succeeded
- file upload succeeded

Client-side validation from CL1:
- `\\sbx-lx1\Shared` opened successfully
- Windows integrated logon was accepted without manual credential prompt
- folder creation succeeded
- file creation succeeded

---

## Key Findings

### Authentication layer
The authentication issue is resolved.

Samba accepts domain-authenticated users correctly, and the member-server trust path is functional.

### Filesystem layer
The remaining blocker was not AD join or Samba logon.

The root cause was Linux backing-directory authorisation on `/srv/samba/shared`.

### Client behaviour
Windows client access from CL1 used integrated domain logon successfully.

After clearing prior SMB sessions, the share still opened without prompting for credentials, indicating expected domain-style SMB single sign-on rather than saved-credential reuse.

---

## Result

The Samba file share on `sbx-lx1` is now operational for domain-authenticated access.

Validated working outcomes:
- AD member state healthy
- winbind identity resolution healthy
- Samba authentication healthy
- share listing healthy
- directory creation healthy
- file creation healthy
- Windows client access healthy

---

## Remaining Note

A later refinement test is still recommended to verify cross-user collaborative write behaviour.

Current validation confirms that the authenticated user who creates content can successfully write to the share.

A follow-up test from a second standard domain user on a second workstation should confirm whether another authorised user can modify files created by the first user.

This is now a refinement task, not an open outage.

---

## Engineering Conclusion

Samba AD integration on `sbx-lx1` is stabilised and operational.

The original Samba access incident can be considered resolved.

The lab is now clear to move to the next planned stage:
- second workstation validation if desired
- SBY second-site Active Directory design
- site, subnet, and replication planning

---

## Next Actions

1. Build or use a second client workstation for cross-user collaborative share validation
2. Record final Samba configuration in infrastructure documentation
3. Proceed to SBY second-site Active Directory design
4. Define sites, subnets, site links, and replication behaviour