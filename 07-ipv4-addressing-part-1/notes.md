# CCNA Day 7 — IPv4 Addresses & Introduction to Routing

## 1. Layer 3 / Network Layer

* **Layer 2:** MAC addresses + switching → communication within a LAN
* **Layer 3:** IP addresses + routing → communication between different networks
* Routers operate at **Layer 3**
* Layer 3 provides:

  * Logical addressing (**IP addresses**)
  * Connectivity between different networks
  * Path selection

### Key idea

**Switch = connects devices within networks**
**Router = connects different networks**

---

## 2. What Changes When We Add a Router?

Without a router:

```text
PC1 ─ SW1 ─ SW2 ─ PC3
192.168.1.0/24 = ONE network
```

With a router:

```text
PC1 ─ SW1 ─ R1 ─ SW2 ─ PC3
      192.168.1.0/24   192.168.2.0/24
```

Router interfaces need an IP address for **each network** they connect to:

```text
R1 G0/0 = 192.168.1.254/24
R1 G0/1 = 192.168.2.254/24
```

### Important

A Layer 2 broadcast does **not cross a router**.

---

# 3. IPv4 Addresses

An IPv4 address is:

* **32 bits**
* **4 bytes**
* Divided into **4 octets**
* Each octet = **8 bits**
* Written in **dotted decimal**

Example:

```text
192.168.1.254
```

```text
192    .168    .1      .254
8 bits  8 bits  8 bits  8 bits
```

Total:

```text
8 + 8 + 8 + 8 = 32 bits
```

---

# 4. Binary

Binary is **base 2**.

An 8-bit octet has these values:

```text
128 64 32 16 8 4 2 1
```

Example:

```text
192 = 11000000
```

So every IPv4 octet ranges from:

**0–255**

---


# 5. Network Portion vs Host Portion

The `/24` tells you how many bits belong to the **network portion**.

Example:

```text
192.168.1.10/24
```

`/24` = first **24 bits** are network bits.

```text
192.168.1 | 10
 NETWORK  | HOST
```

So:

* Network = `192.168.1`
* Host = `10`

### Common examples

```text
/8  → first 8 bits = network
/16 → first 16 bits = network
/24 → first 24 bits = network
```

---

# 6. Same Network vs Different Network

Example:

```text
PC1 = 192.168.1.1/24
PC2 = 192.168.1.2/24
```

Same network:

```text
192.168.1.0/24
```

Because their network portions match.

But:

```text
PC1 = 192.168.1.1/24
PC3 = 192.168.2.1/24
```

Different networks.

A **router** is needed to communicate between them.

---

# 7. IPv4 Address Classes

| Class | First Octet | Default Prefix |
| ----- | ----------: | -------------: |
| A     |       0–127 |             /8 |
| B     |     128–191 |            /16 |
| C     |     192–223 |            /24 |
| D     |     224–239 |      Multicast |
| E     |     240–255 |   Experimental |

Focus mainly on **A, B, C** for this lesson.

