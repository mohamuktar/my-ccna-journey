## CDP & LLDP

# L2 Discovery Protocols — CCNA Revision Notes

## 1. L2 Discovery Protocols

* **Layer 2 discovery protocols** allow network devices to learn information about directly connected devices.
* Main protocols:

  * **CDP** — Cisco Discovery Protocol
  * **LLDP** — Link Layer Discovery Protocol

---

## 2. CDP — Cisco Discovery Protocol

* **Cisco proprietary** Layer 2 discovery protocol.
* Used to discover **directly connected Cisco devices**.
* Can learn information such as:

  * Device ID / hostname
  * Local interface
  * Neighbor's interface
  * Device/platform
  * IOS/software version
  * IP address
  * Device capabilities
* CDP operates at **Layer 2**.
* Does **not** need an IP address to discover a neighbor.

### Key idea

> **CDP = Cisco → Cisco discovery**

---

## 3. CDP Verification

### Show CDP neighbors

```cisco
show cdp neighbors
```

Shows basic information such as:

* Neighbor device
* Local interface
* Neighbor's interface
* Platform
* Capabilities

### Detailed information

```cisco
show cdp neighbors detail
```

Shows additional information such as:

* IP address
* IOS version
* Platform
* Device ID
* Interfaces

### Check CDP status

```cisco
show cdp
```

---

## 4. CDP Configuration

### Enable CDP globally

```cisco
cdp run
```

### Disable CDP globally

```cisco
no cdp run
```

### Disable CDP on one interface

```cisco
interface g0/0
no cdp enable
```

### Enable CDP on an interface

```cisco
interface g0/0
cdp enable
```

**Remember:**

* `cdp run` → global
* `cdp enable` → interface

---

# 5. LLDP — Link Layer Discovery Protocol

* **IEEE 802.1AB** standard.
* Vendor-neutral Layer 2 discovery protocol.
* Allows devices from **different vendors** to discover each other.
* Similar purpose to CDP.

### Key idea

> **LLDP = multi-vendor Layer 2 discovery**

### CDP vs LLDP

|         | CDP                | LLDP               |
| ------- | ------------------ | ------------------ |
| Type    | Cisco proprietary  | Open standard      |
| Vendors | Cisco devices      | Multi-vendor       |
| Layer   | L2                 | L2                 |
| Purpose | Neighbor discovery | Neighbor discovery |

---

# 6. LLDP Configuration

### Enable LLDP globally

```cisco
lldp run
```

### Disable LLDP globally

```cisco
no lldp run
```

### Enable transmit

```cisco
interface g0/0
lldp transmit
```

### Enable receive

```cisco
interface g0/0
lldp receive
```

**Remember:**

* `lldp transmit` → send LLDP information
* `lldp receive` → receive LLDP information

---

# 7. LLDP Verification

### Show LLDP neighbors

```cisco
show lldp neighbors
```

### Detailed information

```cisco
show lldp neighbors detail
```

Can show information such as:

* Neighbor hostname
* Local interface
* Neighbor interface
* IP address
* Capabilities
* Device information

### Check LLDP status

```cisco
show lldp
```

---

# 8. CDP Wireshark Capture

CDP uses **Layer 2 frames**.

In Wireshark, you can inspect:

* Source/destination MAC addresses
* CDP information
* Device ID
* Interface information
* Platform/software information

Useful for understanding **what information discovery protocols actually send over the network**.

---

# 9. LLDP Wireshark Capture

LLDP also uses **Layer 2 Ethernet frames**.

Wireshark can show:

* LLDP neighbor information
* Chassis ID
* Port ID
* System/device information
* Capabilities

---

# 🔥 CCNA Must-Know

```text
CDP = Cisco proprietary
LLDP = IEEE standard / multi-vendor

CDP:
show cdp neighbors
show cdp neighbors detail

LLDP:
show lldp neighbors
show lldp neighbors detail
```

### Memorize this

> **CDP = Cisco**
> **LLDP = Everyone**

And both are **Layer 2 neighbor-discovery protocols**.
