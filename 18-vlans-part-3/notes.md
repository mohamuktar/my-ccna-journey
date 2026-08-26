# Day 18 — VLANs: Native VLAN, Layer 3 Switching & SVIs

## 1. Native VLAN on a Router

In a Router-on-a-Stick (ROAS) configuration, the native VLAN can be configured in two ways:

**Method 1 — Subinterface**

```cisco
interface g0/0.10
 encapsulation dot1q 10 native
```

**Method 2 — Physical interface**

```cisco
interface g0/0
 ip address <ip-address> <subnet-mask>
```

* Native VLAN traffic is **untagged**.
* Default native VLAN is **VLAN 1**.
* Best practice: use an **unused VLAN** as the native VLAN.
* Native VLAN configuration must match on both ends of the trunk.

---

## 2. Wireshark & 802.1Q

Wireshark can be used to inspect VLAN tags inside Ethernet frames.

A tagged frame contains:

* **TPID:** `0x8100`
* **PCP:** Priority Code Point
* **DEI:** Drop Eligible Indicator
* **VID:** VLAN ID

Example:

* VLAN 20 → frame contains an **802.1Q tag with VID 20**
* Native VLAN → frame is **untagged**

---

# 3. Layer 3 / Multilayer Switch

A **Layer 3 switch** can perform both:

* Layer 2 switching
* Layer 3 routing

Unlike a Layer 2 switch, it can:

* Have IP addresses on interfaces
* Perform routing
* Build a routing table
* Configure static/default routes
* Perform inter-VLAN routing

Layer 3 switching is preferred for larger networks because inter-VLAN traffic can be routed directly by the switch instead of traveling to a router and back.

---

# 4. SVI — Switch Virtual Interface

An **SVI** is a virtual Layer 3 interface associated with a VLAN.

Example:

```cisco
interface vlan 10
 ip address 192.168.1.62 255.255.255.192
 no shutdown
```

Each VLAN can have its own SVI.

The SVI becomes the **default gateway** for hosts in that VLAN.

### Example

```text
VLAN 10 → SVI → 192.168.1.62
VLAN 20 → SVI → 192.168.1.126
VLAN 30 → SVI → 192.168.1.190
```

The Layer 3 switch can then route traffic directly between these VLANs.

---

# 5. Routed Port

A physical switch interface can be converted from a Layer 2 switchport into a Layer 3 routed port:

```cisco
interface g0/1
 no switchport
 ip address 192.168.1.193 255.255.255.252
```

`no switchport` = makes the interface a **routed port**.

---

# 6. Enable Layer 3 Routing

On a multilayer switch:

```cisco
ip routing
```

This enables Layer 3 routing and allows the switch to build its own routing table.

Without `ip routing`, inter-VLAN routing on the multilayer switch will not work.

---

# 7. Default Route on Layer 3 Switch

To send traffic outside the local network:

```cisco
ip route 0.0.0.0 0.0.0.0 <next-hop>
```

Example:

```cisco
ip route 0.0.0.0 0.0.0.0 192.168.1.194
```

---

# 8. Requirements for SVI to be Up/Up

For an SVI to become **up/up**:

1. The VLAN must exist.
2. At least one access port in the VLAN must be **up/up**, **or**
3. An up/up trunk must allow that VLAN.
4. The VLAN itself must not be shutdown.
5. The SVI must not be shutdown.

SVIs are **shutdown by default**, so use:

```cisco
no shutdown
```

Important: Creating an SVI does **not** automatically create the VLAN.

---

# 9. Inter-VLAN Routing Methods

### Method 1 — Separate Router Interfaces

```text
VLAN 10 ── Router interface
VLAN 20 ── Router interface
VLAN 30 ── Router interface
```

Requires a separate physical router interface for each VLAN.

### Method 2 — Router on a Stick (ROAS)

```text
        Trunk
Switch ─────── Router
          |
       Subinterfaces
```

Uses one physical router interface with multiple subinterfaces.

### Method 3 — Layer 3 Switch + SVIs

```text
VLAN 10 → SVI
VLAN 20 → SVI
VLAN 30 → SVI
```

The multilayer switch performs the inter-VLAN routing directly.

---

# 10. Key Commands

```cisco
ip routing
```

Enable Layer 3 routing on a multilayer switch.

```cisco
no switchport
```

Convert a switchport into a routed port.

```cisco
interface vlan 10
```

Create/configure an SVI.

```cisco
ip address <ip> <mask>
```

Assign an IP address to an SVI or routed port.

```cisco
no shutdown
```

Enable the interface.

```cisco
show ip interface brief
```

Check interface status and IP addresses.

```cisco
show ip route
```

View the routing table.

```cisco
show interfaces status
```

Check switch interface status.

For a routed port, the VLAN column displays:

```text
ROUTED
```

---

# 11. Day 18 Exam Points

* **Native VLAN traffic is untagged.**
* Default native VLAN = **VLAN 1**.
* `encapsulation dot1q <vlan> native` configures a native VLAN on a ROAS subinterface.
* A physical router interface can also be used for the native VLAN.
* **Layer 3 switch = switching + routing.**
* **SVI = virtual Layer 3 interface for a VLAN.**
* `ip routing` enables routing on a multilayer switch.
* `no switchport` creates a **routed port**.
* SVI does **not** automatically create the VLAN.
* An SVI needs an active VLAN/access port or allowed active trunk to become **up/up**.
* Layer 3 switches can perform inter-VLAN routing without sending the traffic to an external router.