# Day 16 — VLANs

## 1. LAN & Broadcast Domain

* **LAN = a single broadcast domain.**
* **Broadcast domain:** devices that receive a broadcast frame (`destination MAC = FF:FF:FF:FF:FF:FF`).
* A **switch floods** broadcasts.
* A **router does NOT forward** broadcasts.

## 2. VLAN

**VLAN = Virtual Local Area Network**

* VLANs logically separate devices at **Layer 2**.
* Each VLAN is a **separate broadcast domain**.
* VLANs help with:

  * 🔒 **Security** — isolate departments/devices.
  * ⚡ **Performance** — reduce unnecessary broadcast traffic.
* A switch **does not forward traffic directly between VLANs**.
* **Inter-VLAN traffic must be routed** (router or other Layer 3 device).

### Example

| Department  |    VLAN | Subnet           |
| ----------- | ------: | ---------------- |
| Engineering | VLAN 10 | 192.168.1.0/26   |
| HR          | VLAN 20 | 192.168.1.64/26  |
| Sales       | VLAN 30 | 192.168.1.128/26 |

---

## 3. Access Ports vs Trunk Ports

### Access Port

* Belongs to **one VLAN**.
* Usually connects to **end hosts** (PCs, printers, etc.).
* Commands:

```text
switchport mode access
switchport access vlan 10
```

### Trunk Port

* Carries **multiple VLANs**.
* Covered more in Day 17.

---

## 4. VLAN Configuration

### Check VLANs

```text
show vlan brief
```

By default:

* **VLAN 1** exists.
* **VLANs 1002–1005** also exist.
* These default VLANs **cannot be deleted**.
* Switch interfaces are in **VLAN 1 by default**.

### Create/Name a VLAN

```text
vlan 10
name ENGINEERING
```

```text
vlan 20
name HR
```

```text
vlan 30
name SALES
```

### Assign Multiple Interfaces

```text
int range g1/0 - 3
switchport mode access
switchport access vlan 10
```

Then verify:

```text
show vlan brief
```

---

## ⭐ Remember

**VLAN = Layer 2 segmentation**

**Different VLANs = different broadcast domains**

**Switch = does NOT route between VLANs**

**Router/Layer 3 device = routes between VLANs**

**Access port = 1 VLAN**

**Trunk port = multiple VLANs**

**`show vlan brief` = see VLANs + assigned ports**
