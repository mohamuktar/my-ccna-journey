## OSPF Part 1 — Revision Notes

## 1. OSPF Basics

**OSPF = Open Shortest Path First**

* **Link-state** dynamic routing protocol.
* Uses **Dijkstra’s Shortest Path First (SPF) algorithm**.
* **CCNA explicitly tests single-area OSPFv2**.
* OSPFv2 → mainly IPv4.
* OSPFv3 → IPv6 (can also support IPv4).
* Routers build a **complete map of the network**.

### Link-State vs Distance-Vector

| Distance Vector              | Link State                  |
| ---------------------------- | --------------------------- |
| "Routing by rumor"           | Builds complete network map |
| RIP, EIGRP                   | OSPF                        |
| Learns routes from neighbors | Floods link information     |
| Less resource-intensive      | More CPU/memory             |
| Generally slower to react    | Generally faster to react   |

---

# 2. LSA & LSDB

### LSA — Link-State Advertisement

Contains information about a router's links/networks.

### LSDB — Link-State Database

* Collection of LSAs.
* Routers **flood LSAs** throughout the OSPF area.
* All routers in the **same area should have the same LSDB**.

### Basic OSPF Process

**1. Become neighbors**
↓
**2. Exchange/flood LSAs**
↓
**3. Build identical LSDB**
↓
**4. Run Dijkstra/SPF**
↓
**5. Calculate best routes → routing table**

**Important:** Each router independently calculates its best routes using the same network map.

### LSA Aging

* Default aging timer: **30 minutes**
* LSA is flooded again after aging.

---

# 3. OSPF Areas

OSPF uses **areas to divide large networks into smaller sections**.

### Why use areas?

Large single-area OSPF networks can cause:

* More SPF calculation time
* More CPU processing
* Larger LSDB → more memory
* Small topology changes can cause LSAs to flood widely and trigger SPF calculations

### Area 0

**Area 0 = Backbone Area**

For multi-area OSPF:

> **All other areas must connect to Area 0.**

For CCNA, focus mainly on **single-area OSPF**, usually Area 0.

---

# 4. Important OSPF Area Terms

| Term                 | Meaning                                           |
| -------------------- | ------------------------------------------------- |
| **Area**             | Set of routers/links sharing the same LSDB        |
| **Area 0**           | Backbone area                                     |
| **Internal Router**  | All interfaces are in the same OSPF area          |
| **ABR**              | Area Border Router — interfaces in multiple areas |
| **Backbone Router**  | Router with an interface in Area 0                |
| **Intra-area route** | Destination is inside the same area               |
| **Interarea route**  | Destination is in a different area                |
| **ASBR**             | Connects OSPF to an external network              |

### ABR

**ABR = Area Border Router**

Example:

```text
Area 1 ---- ABR ---- Area 0
```

An ABR:

* Has interfaces in multiple OSPF areas.
* Maintains a **separate LSDB for each area** it connects to.

---

# 5. OSPF Area Rules

Memorize these:

### Rule 1 — Areas should be contiguous

An area should **not be split into disconnected sections**.

### Rule 2 — Areas connect to Area 0

Every OSPF area should have an **ABR connected to Area 0**.

```text
Area 1 ──┐
Area 2 ──┼── Area 0
Area 3 ──┘
```

### Rule 3 — Same subnet = same area

OSPF interfaces in the **same subnet must be in the same area** to become neighbors.

---

# 6. Basic OSPF Configuration

Enter OSPF configuration mode:

```cisco
router ospf 1
```

`1` = **OSPF process ID**

### Important: OSPF Process ID

Unlike EIGRP AS numbers:

> **OSPF process ID is locally significant.**

So this can work:

```text
R1: router ospf 1
R2: router ospf 2
```

They can still become OSPF neighbors.

The process ID is **NOT the area number**.

---

## Network Command

Syntax:

```cisco
network <network> <wildcard-mask> area <area-id>
```

Example:

```cisco
router ospf 1
network 10.0.12.0 0.0.0.15 area 0
```

### VERY IMPORTANT

The OSPF `network` command:

> **Tells OSPF which interfaces to activate OSPF on.**

It does **NOT** mean:

> "Advertise this exact network."

OSPF checks the interface IP against the network + wildcard mask.

If it matches → OSPF is activated on that interface.

---

# 7. Passive Interface

```cisco
passive-interface g2/0
```

Effects:

* Stops OSPF **Hello messages** on the interface.
* Prevents OSPF neighbor formation there.
* The connected subnet can still be advertised through OSPF.

Use passive interfaces where there are **no OSPF neighbors**.

---

# 8. Advertise Default Route

If the router has a default route:

```cisco
ip route 0.0.0.0 0.0.0.0 <next-hop>
```

Advertise it into OSPF:

```cisco
router ospf 1
default-information originate
```

This causes the router to generate/flood information about the default route.

---

# 9. OSPF Router ID

Router ID selection order:

**1. Manually configured router ID**
↓
**2. Highest IP address on a loopback interface**
↓
**3. Highest IP address on a physical interface**

### Configure Router ID

```cisco
router ospf 1
router-id 1.1.1.1
```

To make the new router ID take effect:

```cisco
clear ip ospf process
```

⚠️ This resets the OSPF process and temporarily removes OSPF routes, so avoid doing it casually in production.

---

# 10. OSPF Load Balancing

OSPF supports:

* ✅ **Equal-cost load balancing (ECMP)**
* ❌ **Unequal-cost load balancing**

Default:

```text
Maximum paths = 4
```

Change it:

```cisco
maximum-paths 8
```

---

# 11. OSPF Administrative Distance

Default OSPF AD:

```text
110
```

Change it:

```cisco
distance 85
```

Lower AD = preferred route.

---

# 12. `show ip protocols`

```cisco
show ip protocols
```

Useful for checking:

* OSPF process ID
* Router ID
* Number of areas
* Maximum paths
* Network statements
* Passive interfaces
* OSPF neighbors
* Administrative distance
* Other OSPF information

---

# 🔥 CCNA Must-Know

Memorize these:

```text
OSPF = Link State
OSPFv2 = IPv4
Dijkstra = SPF algorithm
LSA = Link-State Advertisement
LSDB = Link-State Database
Area 0 = Backbone
ABR = Area Border Router
ASBR = External network connection
OSPF AD = 110
Multicast = 224.0.0.5/224.0.0.6
Default maximum paths = 4
```

### Most important commands

```cisco
router ospf 1
network 10.0.12.0 0.0.0.15 area 0
passive-interface g2/0
default-information originate
router-id 1.1.1.1
maximum-paths 8
distance 85
show ip protocols
clear ip ospf process
```

### OSPF mental model

```text
Neighbors
   ↓
Exchange LSAs
   ↓
Build LSDB
   ↓
Complete network map
   ↓
Dijkstra / SPF
   ↓
Best routes
   ↓
Routing table
```

**For CCNA:** Know the basics here, but pay especially close attention to **OSPF neighbors, LSAs, network types, and router IDs** in the next lessons. Those are where the OSPF material gets much deeper.
