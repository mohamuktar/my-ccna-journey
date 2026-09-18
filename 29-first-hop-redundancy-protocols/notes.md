# FHRP — Short Revision Notes

## 1. What is FHRP?

**FHRP = First Hop Redundancy Protocol**

Purpose: provide a **redundant default gateway** for a subnet.

Problem:

* PCs normally use one router as their default gateway.
* If that router fails, PCs lose access to other networks.
* FHRP allows **2+ routers to share a Virtual IP (VIP)**.

### Basic operation

* Routers share a **Virtual IP** and **Virtual MAC**.
* Hosts use the **VIP as their default gateway**.
* One router handles traffic as the active/master router.
* If it fails, another router takes over.
* New active router sends **gratuitous ARP** so switches update their MAC tables.

**Important:** FHRP provides redundancy for the **first hop/default gateway**.

---

# 2. FHRP Protocols

| Protocol | Type              | Router Roles     | IPv4 Multicast                       | Virtual MAC                                  |
| -------- | ----------------- | ---------------- | ------------------------------------ | -------------------------------------------- |
| **HSRP** | Cisco proprietary | Active / Standby | v1: `224.0.0.2`<br>v2: `224.0.0.102` | v1: `0000.0c07.acXX`<br>v2: `0000.0c9f.fXXX` |
| **VRRP** | Open standard     | Master / Backup  | `224.0.0.18`                         | `0000.5e00.01XX`                             |
| **GLBP** | Cisco proprietary | AVG / AVF        | `224.0.0.102`                        | `0007.b400.XXXX`                             |

### 🔥 Main differences

**HSRP**

* Cisco proprietary
* Active + Standby
* Cannot load-balance within one subnet
* Can use different active routers for different VLANs/subnets

**VRRP**

* Open standard
* Master + Backup
* Similar functionality to HSRP
* Can use different master routers for different VLANs/subnets

**GLBP**

* Cisco proprietary
* **Load balances within a single subnet**
* Multiple routers can actively forward traffic
* **AVG** = Active Virtual Gateway
* **AVF** = Active Virtual Forwarder

---

# 3. HSRP

### Versions

* **HSRP v1**

  * Multicast: `224.0.0.2`
  * Groups: `0–255`
  * MAC: `0000.0c07.acXX`

* **HSRP v2**

  * IPv6 support
  * Multicast: `224.0.0.102`
  * Groups: `0–4095`
  * MAC: `0000.0c9f.fXXX`

**HSRP v1 and v2 are not compatible.**

### HSRP Active Router Selection

1. Highest **HSRP priority**
2. If tied → highest **interface IP address**

Default priority = **100**

Priority range = **0–255**

---

# 4. HSRP Preemption

By default, FHRPs are **non-preemptive**.

Example:

```text
R1 = Active
R2 = Standby

R1 fails
↓
R2 = Active

R1 returns
↓
R1 = Standby
R2 = Active
```

With **preemption**, R1 can take back the active role if it has the higher priority.

```cisco
standby 1 preempt
```

---

# 5. Basic HSRP Configuration

Configured directly on the interface:

```cisco
interface g0/0
standby version 2
standby 1 ip 172.16.0.254
standby 1 priority 200
standby 1 preempt
```

* `standby version 2` → HSRP v2
* `standby 1 ip` → virtual IP
* `standby 1 priority` → influences active router
* `standby 1 preempt` → allows router to reclaim active role

**HSRP group number must match between routers.**

Check:

```cisco
show standby
```

---

# 6. Virtual IP + Virtual MAC

Hosts use:

```text
Default Gateway → Virtual IP
```

The active router responds to ARP with the **virtual MAC**.

If active router fails:

```text
Standby → Active
       ↓
Gratuitous ARP
       ↓
Switch MAC tables update
       ↓
Traffic goes through new active router
```

Hosts don't need to change their default gateway.

---

# 7. GLBP

**Big exam point:**

> **GLBP allows multiple active routers to load-balance traffic within ONE subnet.**

* **AVG** = Active Virtual Gateway
* Up to **4 AVFs**
* AVFs act as gateways for different hosts.

Remember:

**HSRP/VRRP → redundancy, one active gateway per subnet**

**GLBP → redundancy + load balancing within the subnet**

---

# 🔥 CCNA Must-Know

* **FHRP = redundant default gateway**
* **VIP = virtual default gateway**
* **Virtual MAC** is used by the FHRP
* Active router failure → standby/backup takes over
* **Non-preemptive by default**
* **HSRP = Cisco proprietary**
* **VRRP = open standard**
* **GLBP = Cisco proprietary + load balancing within one subnet**
* HSRP: **Active / Standby**
* VRRP: **Master / Backup**
* GLBP: **AVG / AVF**
* HSRP v1 → `224.0.0.2`
* HSRP v2 → `224.0.0.102`
* VRRP → `224.0.0.18`
* GLBP → `224.0.0.102`
* HSRP active selection → **priority, then IP**
* HSRP default priority → **100**
* `standby 1 preempt` → reclaim active role after recovery
* `show standby` → verify HSRP
