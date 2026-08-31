# DAY 22 — RAPID SPANNING TREE (RSTP)

## 1. STP Versions — Know These

| Standard   | Cisco Version   | Key Point                            |
| ---------- | --------------- | ------------------------------------ |
| **802.1D** | **PVST+**       | Classic STP                          |
| **802.1w** | **Rapid PVST+** | Rapid STP                            |
| **802.1s** | **MSTP**        | Groups multiple VLANs into instances |

**CCNA focus:** **RSTP / Rapid PVST+**

* Classic STP → up to **50 sec** convergence
* RSTP → **a few seconds**
* Rapid PVST+ → separate STP instance **per VLAN**

---

## 2. RSTP — Big Idea

**RSTP = faster STP.**

Classic STP relies heavily on timers.

RSTP uses a **handshake/negotiation mechanism** to move appropriate ports to forwarding much faster.

> RSTP is an **evolution**, not a complete replacement of STP.

---

## 3. RSTP Port States

Classic STP:

**Blocking → Listening → Learning → Forwarding**

RSTP has only **3 states**:

1. **Discarding**
2. **Learning**
3. **Forwarding**

**Listening is removed.**

**Blocking + Disabled → Discarding**

---

## 4. RSTP Port Roles

RSTP has **4 port roles**:

### Root Port

* Best path toward the root bridge
* Lowest root cost
* Root bridge has **no root port**

### Designated Port

* Best port on a segment
* Forwards traffic

### Alternate Port

* Discarding
* Receives a superior BPDU **from another switch**
* Backup for the root port
* Can rapidly become the root port

### Backup Port

* Discarding
* Receives a superior BPDU **from another interface on the same switch**
* Usually only occurs with a **hub**

**Easy distinction:**

> **Alternate = backup from another switch**
> **Backup = backup from the same switch**

---

## 5. RSTP Port Costs

RSTP supports faster links with updated costs:

| Speed    |      Cost |
| -------- | --------: |
| 10 Mbps  | 2,000,000 |
| 100 Mbps |   200,000 |
| 1 Gbps   |    20,000 |
| 10 Gbps  |     2,000 |
| 100 Gbps |       200 |
| 1 Tbps   |        20 |

---

## 6. UplinkFast / BackboneFast / PortFast

These classic STP features are built into RSTP:

* **UplinkFast** → rapidly recover from a root-port failure
* **BackboneFast** → rapidly react to indirect link failures
* **PortFast** → edge ports can immediately enter forwarding

For CCNA:

> Know their **names + basic purpose**, not detailed configuration.

---

## 7. RSTP BPDU — EXAM FACTS ⭐

Remember:

* Classic STP BPDU → **Protocol Version 0**
* RSTP BPDU → **Protocol Version 2**
* RSTP BPDU Type → **2**
* Classic STP → mainly root bridge originates BPDUs
* **RSTP → ALL switches originate BPDUs**

RSTP considers a neighbor lost after missing **3 BPDUs = 6 seconds**.

---

## 8. RSTP Link Types

### Edge

Connected to an **end host**.

* Equivalent to PortFast
* Moves directly toward forwarding

```bash
spanning-tree portfast
```

### Point-to-Point

Direct connection between **two switches**.

* Full-duplex
* Usually detected automatically

```bash
spanning-tree link-type point-to-point
```

### Shared

Connection to a **hub**.

* Half-duplex
* Rare in modern networks

```bash
spanning-tree link-type shared
```

⚠️ **Don't confuse:**

**Port role ≠ Port state ≠ Link type**

---

# 🔥 DAY 22 — MUST REMEMBER

If you're short on time, memorize these:

1. **802.1w = RSTP**
2. **Cisco = Rapid PVST+**
3. **RSTP is faster because of handshake/negotiation**
4. **RSTP states:** Discarding, Learning, Forwarding
5. **RSTP roles:** Root, Designated, Alternate, Backup
6. **Alternate = superior BPDU from another switch**
7. **Backup = superior BPDU from same switch**
8. **RSTP BPDU version = 2**
9. **ALL RSTP switches send BPDUs**
10. **Edge = end host = PortFast**
11. **Point-to-point = switch-to-switch**
12. **Shared = hub**
13. **UplinkFast + BackboneFast + PortFast are built into RSTP**
14. **MSTP (802.1s) = groups VLANs into STP instances**

### 🧠 One-line mental model

**RSTP = STP, but faster + 3 states + 4 roles + faster recovery.**
