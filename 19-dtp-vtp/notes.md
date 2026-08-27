### 🧠 Day 19 — What you actually need

#### 1. DTP = negotiates trunking

Cisco proprietary protocol.

Its job:

> **“Should this link become a trunk?”**

The important modes:

| SW1               | SW2               | Result     |
| ----------------- | ----------------- | ---------- |
| Access            | Anything          | **Access** |
| Trunk             | Trunk             | **Trunk**  |
| Trunk             | Dynamic Auto      | **Trunk**  |
| Trunk             | Dynamic Desirable | **Trunk**  |
| Dynamic Desirable | Dynamic Auto      | **Trunk**  |
| Dynamic Desirable | Dynamic Desirable | **Trunk**  |
| Dynamic Auto      | Dynamic Auto      | **Access** |

🔥 **The big rule:**

> **Desirable actively tries. Auto waits.**

So:

**Desirable + Auto = TRUNK**

**Auto + Auto = ACCESS**

And:

> **Access + Trunk = MISCONFIGURATION**

---

### 2. Security

Don't rely on DTP.

Normally configure ports manually:

```cisco
switchport mode access
```

or

```cisco
switchport mode trunk
```

To stop DTP negotiation:

```cisco
switchport nonegotiate
```

**Remember:**

> **DTP = Dynamic Trunking Protocol → negotiates trunking.**

---

# VTP — even simpler

VTP's purpose:

> **Synchronize VLAN databases between Cisco switches.**

There are 3 modes:

### 🟢 Server

Can:

* Create VLANs
* Modify VLANs
* Delete VLANs
* Advertise VLAN database

### 🔵 Client

Cannot create/modify/delete VLANs.

It **receives and synchronizes** the VLAN database.

### 🟡 Transparent

Does its **own VLAN configuration**.

It **doesn't synchronize its VLAN database**, but can **forward VTP advertisements**.

---

### 🚨 The one VTP concept I REALLY want you to remember

**Revision number.**

Higher revision number = considered newer VLAN database.

That's why VTP can be dangerous.

Example:

```text
Network:
Revision 10
VLANs 10,20,30,40

Old switch:
Revision 50
VLANs 1,99,200
```

Plug that old switch into the VTP domain...

💀 **Revision 50 wins.**

The other switches can synchronize to its VLAN database.

---

# 🎯 Your Day 19 cheat sheet


```text
DTP
├── Cisco proprietary
├── Negotiates access/trunk
├── Dynamic Desirable = actively tries
├── Dynamic Auto = waits
├── Desirable + Auto = TRUNK
├── Auto + Auto = ACCESS
├── Access + Trunk = MISCONFIG
├── Disable with: switchport nonegotiate
└── Manually configure access/trunk for security

VTP
├── Cisco proprietary
├── Synchronizes VLAN databases
├── Server = create/modify/delete
├── Client = receives VLAN database
├── Transparent = independent VLAN database
└── Higher revision number = newer database
```
