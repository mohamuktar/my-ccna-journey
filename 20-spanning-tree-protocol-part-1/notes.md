### 🟢 Day 20 — STP: What You Actually Need

## 1. Why do we need STP?

Redundant links are good because they provide **backup paths**.

But redundant Layer 2 links create **Layer 2 loops**.

Example:

```text
        SW1
       /   \
     SW2---SW3
```

A broadcast frame can keep circulating:

```text
SW1 → SW2 → SW3 → SW1 → SW2 → ...
```

Ethernet frames have **no TTL**, so the loop can continue indefinitely.

This causes:

### Broadcast storm

Too many looping frames → network becomes congested.

### MAC address flapping

The same source MAC keeps appearing on different switchports → switch's MAC table keeps changing.

---

# 2. What does STP do?

**STP = Spanning Tree Protocol**

Its job:

> **Prevent Layer 2 loops while keeping redundant links available as backups.**

It does this by putting some ports into:

* 🟢 **Forwarding** → sends/receives normal traffic
* 🔴 **Blocking** → doesn't forward normal traffic; acts as a backup

If the active link fails, STP can adjust the topology and allow the backup path to forward.

### The BIG idea:

> **STP intentionally blocks redundant paths to prevent loops.**

---

# 3. BPDUs

STP switches communicate using:

**BPDU = Bridge Protocol Data Unit**

STP sends Hello BPDUs.

Default Hello timer:

**2 seconds**

A switch receiving BPDUs knows it is connected to another STP-enabled switch.

---

# 4. Root Bridge 👑

This is probably the **most important concept from today's section.**

STP elects one switch as the:

> **Root Bridge**

The switch with the **lowest Bridge ID wins.**

```text
Lowest Bridge ID
       ↓
   ROOT BRIDGE
```

The Bridge ID is based on:

```text
Bridge ID
├── Bridge Priority
└── MAC Address
```

If priority is tied:

> **Lowest MAC address wins.**

### Default

All switches start with the same default priority.

So if you don't change anything:

> **Lowest MAC address becomes the root bridge.**

---

# 5. Root Bridge ports

Once the root bridge is elected:

> **All ports on the root bridge are Designated Ports and are Forwarding.**

Other switches need a path toward the root bridge.

---

# 6. PVST

Cisco uses:

**PVST = Per-VLAN Spanning Tree**

Meaning:

> STP can have a separate instance for each VLAN.

So you could have:

```text
VLAN 10 → SW1 is root
VLAN 20 → SW2 is root
VLAN 30 → SW3 is root
```

That's why the VLAN ID is included in the extended system ID.

---

# 7. Extended System ID

This is the part Jeremy goes deep into that **you don't need to obsess over yet.**

Just remember:

```text
Bridge ID
= Priority + Extended System ID + MAC
```

The Extended System ID contains the:

> **VLAN ID**

And the configurable STP priority changes in increments of:

> **4096**


---

# 🧠 Day 20 Cheat Sheet

Put this in your notes:

```text
STP — Spanning Tree Protocol

Purpose:
Prevent Layer 2 loops in redundant networks.

Without STP:
→ Broadcast storms
→ MAC address flapping
→ Network congestion

STP:
→ Blocks redundant paths
→ Creates a loop-free Layer 2 topology
→ Keeps blocked links as backups

BPDUs:
→ STP control messages
→ Default Hello = 2 seconds

Root Bridge:
→ Switch with LOWEST Bridge ID
→ Bridge ID = Priority + MAC address
→ If priority ties → lowest MAC wins
→ All root bridge ports = Designated + Forwarding

PVST:
→ Per-VLAN Spanning Tree
→ Separate STP instance per VLAN

Extended System ID:
→ Contains VLAN ID
→ STP priority changes in increments of 4096
```

### Memorize these 5 lines :

> **STP prevents Layer 2 loops.**
> **It blocks redundant paths.**
> **BPDUs are used to communicate STP information.**
> **Lowest Bridge ID becomes Root Bridge.**
> **If priority ties, lowest MAC address wins.**

