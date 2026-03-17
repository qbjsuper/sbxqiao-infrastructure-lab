[2026-03-17 16:06 AEST]

Understood. That changes the management status in a clear way:

`pfSense-SBY` remote GUI access from the big PC is now working, because you added an IPsec firewall rule permitting access from the `172.16.50.0/24` network. For now, you are intentionally keeping this broad because it is the easiest configuration, and the big PC is acting as your main management console.

Use this as the updated checkpoint text.

---

## Checkpoint — SBX↔SBY IPsec Working, pfSense-SBY GUI Reachable from SBX Side

### Current architecture position

The lab remains a **single forest / single domain** design:

* Forest: `sbxqiao.lab`
* Domain: `sbxqiao.lab`

Physical Hyper-V hosts remain in **workgroup** and are **not** joined to the domain.

Physical hosts:

* `QIAO-AU` = big PC, primary Hyper-V host
* `BOJIE_MS_A2` = mini PC, secondary Hyper-V host

Only the lab VMs participate in the domain.

---

### Hyper-V management state

Centralised multi-host Hyper-V management from the big PC is still working.

Working method:

```cmd
runas /netonly /user:BOJIE_MS_A2\hvadmin "C:\Windows\System32\mmc.exe C:\Windows\System32\virtmgmt.msc"
```

This starts Hyper-V Manager on the big PC using network-only credentials for the mini PC, so one console can manage both Hyper-V hosts. It is required because the two physical hosts are in workgroup mode, not in a domain or cluster. You should expect the MMC console to open normally on the big PC and allow connections to both `QIAO-AU` and `BOJIE_MS_A2`.

This remains a **management-plane workaround** for workgroup hosts, not clustering.

---

### Site state before AD expansion

#### SBX

* Site subnet: `172.16.50.0/24`
* `pfSense-SBX` LAN IP: `172.16.50.1`
* `pfSense-SBX` WAN IP: `192.168.0.252`
* `sbx-dc1` is active
* `sbx-dc1` currently provides:

  * AD DS
  * DNS
  * DHCP

#### SBY

* Site subnet: `172.16.51.0/24`
* `pfSense-SBY` LAN IP: `172.16.51.1`
* `pfSense-SBY` WAN IP: `192.168.0.253`
* `sby-dc1` is deployed but **not yet configured**
* `pfSense-SBY` is installed and has working IPsec transport with SBX

---

### Underlay / WAN reachability milestone

Home-router underlay:

* network: `192.168.0.0/24`
* gateway: `192.168.0.1`

Validated outcomes:

* `192.168.0.252` can ping `192.168.0.1`
* `192.168.0.253` can ping `192.168.0.1`
* WAN IP, mask, gateway, and external vSwitch attachment were confirmed correct
* peer WAN-to-WAN ping initially failed because inbound ICMP on WAN was blocked
* temporary WAN ICMP allow rules were added on both pfSense firewalls
* after that, `192.168.0.252` and `192.168.0.253` could ping each other

Conclusion: **underlay transport is working**

---

### IPsec milestone completed

A first-pass site-to-site IPsec tunnel between SBX and SBY is working.

#### Protected networks

* SBX LAN: `172.16.50.0/24`
* SBY LAN: `172.16.51.0/24`

#### Phase 1 baseline

* IKE version: `IKEv2`
* Internet Protocol: `IPv4`
* Authentication: `Mutual PSK`
* Encryption: `AES-256`
* Hash: `SHA-256`
* DH Group: `14`

#### Phase 2 baseline

* Mode: `Tunnel IPv4`
* Protocol: `ESP`
* Encryption: `AES-256`
* Hash: `SHA-256`
* PFS Group: `14`

#### Confirmed tunnel status

On pfSense status pages:

* **Phase 1:** `Established`
* **Phase 2:** `Installed`

This confirms the tunnel is operational.

---

### Key troubleshooting findings during IPsec build

#### 1. WAN peer ping failure

Initial problem:

* `192.168.0.252` and `192.168.0.253` could not ping each other

Root cause:

* WAN-side ICMP was blocked by firewall policy

Resolution:

* temporary WAN ICMP allow rules were added on both pfSense firewalls

#### 2. Tunnel up but no traffic passing

Initial problem:

* Phase 1 and Phase 2 were active
* traffic across the tunnel still failed

Observed behaviour:

* SBY was sending
* SBX was receiving
* SBX was not sending matching return traffic

Root cause:

* **IPsec firewall rule direction was reversed**

Important pfSense behaviour:

* rules on the **IPsec** tab apply to traffic as it **enters from the remote side**
* the rule source must therefore be the **remote subnet**

Correct rule direction:

**On SBX**

* source: `172.16.51.0/24`
* destination: `172.16.50.0/24`

**On SBY**

* source: `172.16.50.0/24`
* destination: `172.16.51.0/24`

After correcting the IPsec rule direction, cross-site ping worked.

Conclusion: **tunnel healthy; initial traffic failure was policy direction, not tunnel establishment**

---

### Current confirmed transport state

Now working:

* encrypted subnet-to-subnet transport between:

  * `172.16.50.0/24`
  * `172.16.51.0/24`
* ICMP across the tunnel
* first-pass network transport milestone completed

This is still a **transport success checkpoint**, not yet a full multi-site AD checkpoint.

---

### Management-plane status update

`pfSense-SBY` GUI access from the big PC is now working.

#### Current working approach

A rule was added on `pfSense-SBY` to allow management access from the SBX-side network over IPsec.

Practical outcome:

* the big PC can now log in to `pfSense-SBY`
* for now, access is being left open to the `172.16.50.0/24` network because it is the easiest configuration
* operationally, the big PC is being used as the primary console for this management path

#### Design note

This is acceptable as a **lab-stage convenience rule**, but it should be recorded as broader than a hardened design.

A later hardening step would usually reduce access to one of these:

* a dedicated SBX admin VM IP
* a small admin management subnet
* specific admin source objects only

For now, keeping it open to the `.50` site is a deliberate convenience choice.

---

### Current unresolved items

Not yet completed:

* `sby-dc1` configuration
* DNS reachability validation across sites
* AD Sites and Services mapping
* AD replication validation
* later hardening of pfSense management access scope

---

### Repo wording to use

#### Established

* WAN underlay working
* IPsec Phase 1 established
* IPsec Phase 2 installed
* encrypted transport between SBX and SBY LANs working
* ICMP over tunnel working
* `pfSense-SBY` GUI reachable from SBX side over IPsec
* big PC can log in to `pfSense-SBY` and act as the current management console

#### Troubleshooted and resolved

* WAN peer ping failure caused by WAN-side ICMP filtering
* IPsec tunnel-up-but-no-traffic caused by reversed IPsec firewall rule direction
* remote pfSense-SBY GUI access enabled by adding an IPsec rule allowing management traffic from `172.16.50.0/24`

#### Intentionally left broad for now

* management access from the SBX-side `.50` network is currently wider than a hardened production design
* this is being kept temporarily for ease of administration during lab build-out

#### Not yet completed

* `sby-dc1` configuration
* cross-site DNS validation
* AD Sites and Services mapping
* AD replication validation
* later restriction of pfSense management access to a dedicated admin source

---

### Best opening line for the next chat

> Checkpoint: SBX↔SBY IPsec is working with Phase 1 established and Phase 2 installed between `192.168.0.252` and `192.168.0.253`, protecting `172.16.50.0/24` and `172.16.51.0/24`; WAN peer ping originally failed due to WAN ICMP filtering, and tunnel traffic originally failed because IPsec rule direction was reversed; pfSense-SBY GUI access from the SBX side now works after adding an IPsec management rule allowing access from `172.16.50.0/24`; this broad access is being kept temporarily for ease of administration from the big PC console; next task is continuing with `sby-dc1` build.

This is the right temporary choice for a lab. Later, tighten it after `sby-dc1` is up and basic multi-site AD validation is stable.
