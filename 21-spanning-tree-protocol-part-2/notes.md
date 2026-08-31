# DAY 21 — WHAT YOU ACTUALLY NEED

##  STP States

```text
Blocking → Listening → Learning → Forwarding
```

| State          | Normal Traffic | Learn MACs |
| -------------- | -------------- | ---------- |
| **Blocking**   | ❌              | ❌          |
| **Listening**  | ❌              | ❌          |
| **Learning**   | ❌              | ✅          |
| **Forwarding** | ✅              | ✅          |

**Remember:**

* Blocking = prevents loops
* Listening = transitional
* Learning = builds MAC table
* Forwarding = normal operation

---

##  STP Timers

```text
Hello         = 2 seconds
Forward Delay = 15 seconds
Max Age       = 20 seconds
```

### Forward Delay

```text
Listening = 15 sec
Learning  = 15 sec
-------------------
Total     = 30 sec
```

### Worst-case transition

```text
Max Age       = 20 sec
Listening     = 15 sec
Learning      = 15 sec
-----------------------
Total         = 50 sec
```

**Blocking → Forwarding can take up to 50 seconds.**

---

##  PortFast

PortFast allows an access port connected to an **end host** to immediately enter Forwarding.

Without PortFast:

```text
Listening → Learning → Forwarding
       30 seconds
```

With PortFast:

```text
PortFast → Forwarding immediately
```

### Interface command

```cisco
spanning-tree portfast
```

### Enable globally

```cisco
spanning-tree portfast default
```

**Global command → all access ports.**

⚠️ PortFast should be used on **end-host ports**, not switch-to-switch connections.

---

##  BPDU Guard

If a BPDU is received on a BPDU Guard-enabled port:

```text
BPDU received
     ↓
BPDU Guard
     ↓
Port shuts down
```

### Interface

```cisco
spanning-tree bpduguard enable
```

### Global

```cisco
spanning-tree portfast bpduguard default
```

To recover the port:

```cisco
shutdown
no shutdown
```

---

##  Root Guard / Loop Guard

### Root Guard

Prevents another switch from becoming the root by sending a **superior BPDU**.

```text
Root Guard → protects the root topology
```

### Loop Guard

Helps prevent loops when a port unexpectedly **stops receiving BPDUs**.

```text
Loop Guard → protects against loops
```

---

#  Root Bridge Configuration

### Primary Root

```cisco
spanning-tree vlan 10 root primary
```

### Secondary Root

```cisco
spanning-tree vlan 10 root secondary
```

Think:

```text
Primary   = Main root
Secondary = Backup root
```

---

#  STP Priority

Lower bridge priority = better chance of becoming the root.

Priority is configured in increments of:

```text
4096
```

Typical values:

```text
Primary   = 24576
Secondary = 28672
```

Manual configuration:

```cisco
spanning-tree vlan 10 priority 24576
```

---

#  STP Load Balancing

With PVST+, different VLANs can have different root bridges.

Example:

```text
VLAN 10 → SW1 = Root
VLAN 20 → SW2 = Root
```

Therefore different links can be blocked for different VLANs.

```text
Different VLANs
      ↓
Different root bridges
      ↓
Different STP paths
      ↓
Load balancing
```

---

#  STP Port Cost

Used primarily to determine the **Root Port**.

Examples:

```text
FastEthernet = 19
GigabitEthernet = 4
```

Cost can also help determine Designated/Non-designated ports.

---

#  STP Port Priority

Port ID consists of:

```text
Port Priority + Port Number
```

Example:

```text
0x8002
```

Break it down:

```text
80 | 02
↑    ↑
│    └── Port number
└─────── Port priority
```

```text
0x80 = 128
```

Therefore:

> **Port priority = 128**

Default port priority:

```text
128
```

Configured in increments of:

```text
32
```

---

#  BPDU

BPDU = **Bridge Protocol Data Unit**

Important Cisco PVST+ destination MAC:

```text
0100.0ccc.cccd
```

Classic/non-Cisco STP:

```text
0180.c200.0000
```

You **do not need to memorize the entire BPDU structure** for this CCNA lesson.

Know that it carries STP information such as:

* Root Bridge ID
* Root Path Cost
* Sender Bridge ID
* Sender Port ID
* STP timers

---

#  THE 10 THINGS TO MEMORIZE

```text
1. Blocking → Listening → Learning → Forwarding

2. Blocking = no traffic + no MAC learning

3. Listening = no traffic + no MAC learning

4. Learning = no traffic + DOES learn MACs

5. Forwarding = normal traffic + MAC learning

6. Hello = 2 sec
   Forward Delay = 15 sec
   Max Age = 20 sec

7. PortFast = immediately Forwarding for end hosts

8. BPDU Guard = BPDU received → port shuts down

9. Lower STP priority = better chance of becoming root

10. 0x8002 → port priority = 128
```

###  One-line mental model

> **STP blocks redundant paths, slowly activates new paths, PortFast skips the waiting for end hosts, and BPDU Guard shuts down an end-host port if another switch appears.**
