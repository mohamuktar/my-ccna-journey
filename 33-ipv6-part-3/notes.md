### IPv6 Part 3 | Day 33 Notes

## 1. IPv6 Address Representation — RFC 5952

RFC 5952 recommends a standard way to write IPv6 addresses.

### Rules

1. **Remove leading zeros** from each quartet.
2. Replace the **longest consecutive sequence of all-zero quartets** with `::`.
3. If there are equal-length zero sequences, use `::` for the **leftmost** one.
4. Don't use `::` for only **one** zero quartet.
5. Hexadecimal letters should be **lowercase** (`a–f`).

Example:

```text
2001:0DB8:0000:0000:0000:0000:0000:0001
```

becomes:

```text
2001:db8::1
```

🔥 **Exam idea:** `::` can represent multiple zero quartets, but only **once** in an address.

---

# 2. IPv6 Header

IPv6 has a **fixed 40-byte header**.

| Field               |     Size | Purpose                         |
| ------------------- | -------: | ------------------------------- |
| Version             |   4 bits | Always `6`                      |
| Traffic Class       |   8 bits | QoS / priority                  |
| Flow Label          |  20 bits | Identifies traffic flows        |
| Payload Length      |  16 bits | Length of Layer 4 payload       |
| Next Header         |   8 bits | Identifies next protocol/header |
| Hop Limit           |   8 bits | Same basic function as IPv4 TTL |
| Source Address      | 128 bits | Sender                          |
| Destination Address | 128 bits | Receiver                        |

### Important comparisons

```text
IPv4 TTL       → IPv6 Hop Limit
IPv4 Protocol  → IPv6 Next Header
IPv4 header    → Variable length
IPv6 header    → Fixed 40 bytes
```

**Hop Limit:** decremented by each router. When it reaches `0`, the packet is discarded.

---

# 3. Solicited-Node Multicast

A **solicited-node multicast address** is generated from an IPv6 unicast address.

Format:

```text
FF02::1:FF + last 6 hex digits
```

Example:

```text
IPv6 address:
2001:db8::1234:5678

Solicited-node multicast:
FF02::1:FF34:5678
```

🔥 **Memorize:**

> `FF02::1:FF` + **last 6 hex digits**

These addresses are heavily used by **NDP**.

---

# 4. Neighbor Discovery Protocol (NDP)

**NDP replaces ARP in IPv6.**

NDP uses **ICMPv6** and multicast instead of IPv4 ARP broadcasts.

### ARP vs NDP

| IPv4                  | IPv6                            |
| --------------------- | ------------------------------- |
| ARP                   | NDP                             |
| Broadcast ARP request | Multicast Neighbor Solicitation |
| ARP reply             | Neighbor Advertisement          |
| ARP table             | IPv6 Neighbor Table             |

---

## Neighbor Solicitation (NS)

Equivalent to an **ARP request**.

```text
ICMPv6 Type 135
```

Used to ask:

> "What is your MAC address?"

The request is sent to the destination's **solicited-node multicast address**.

---

## Neighbor Advertisement (NA)

Equivalent to an **ARP reply**.

```text
ICMPv6 Type 136
```

The destination responds with information including its MAC address.

---

## IPv6 Neighbor Table

Cisco command:

```cisco
show ipv6 neighbor
```

Shows information such as:

* IPv6 address
* MAC address
* Interface
* Age
* Neighbor state

---

# 5. Router Solicitation / Router Advertisement

NDP also allows hosts to discover routers.

### Router Solicitation (RS)

```text
ICMPv6 Type 133
Destination: FF02::2
```

**FF02::2 = All routers**

A host sends an RS asking:

> "Are there any routers on this network?"

---

### Router Advertisement (RA)

```text
ICMPv6 Type 134
Destination: FF02::1
```

**FF02::1 = All nodes**

The router announces its presence and provides network information.

RA can provide information such as:

* IPv6 prefix
* Default gateway information

Routers can send RAs:

* In response to RS
* Periodically

---

# 6. SLAAC

**SLAAC = Stateless Address Autoconfiguration**

Allows a host to automatically configure an IPv6 address.

### Basic process

```text
Host
 ↓
Router Solicitation
 ↓
Router Advertisement
 ↓
Learns IPv6 prefix
 ↓
Generates its own Interface ID
 ↓
IPv6 address configured
```

Example:

```cisco
interface g0/0
ipv6 address autoconfig
```

Unlike EUI-64 configuration, you **don't manually specify the prefix**.

The device learns the prefix through **NDP/RA**.

The Interface ID can be generated using EUI-64 or randomly, depending on the device.

---

# 7. Duplicate Address Detection (DAD)

**DAD checks whether an IPv6 address is already being used.**

It occurs when an IPv6 interface/address initializes.

Examples:

* Interface comes up
* IPv6 address is configured
* SLAAC generates an address

### Basic process

```text
Device
 ↓
Sends Neighbor Solicitation
to its own solicited-node multicast address
 ↓
No response → address is unique
Response → duplicate address detected
```

DAD uses:

* Neighbor Solicitation
* Neighbor Advertisement

🔥 **Remember:** DAD is another function of **NDP**.

---

# 8. IPv6 Routing

IPv6 routing works similarly to IPv4:

```text
Packet arrives
      ↓
Check routing table
      ↓
Find most specific match
      ↓
Forward packet
```

But IPv4 and IPv6 have **separate routing processes and routing tables**.

View IPv6 routes:

```cisco
show ipv6 route
```

### Enable IPv6 routing

```cisco
ipv6 unicast-routing
```

⚠️ Without this command, the router can send/receive IPv6 traffic but **won't forward IPv6 traffic between networks**.

---

# 9. IPv6 Connected & Local Routes

When you configure an IPv6 address, the router automatically creates:

### Connected route

For the network:

```text
/64
```

### Local route

For the specific interface address:

```text
/128
```

Example:

```text
2001:db8:1::/64    → Connected network
2001:db8:1::1/128  → Local host route
```

### Important

```text
IPv4 host route → /32
IPv6 host route → /128
```

Link-local addresses do **not** appear as normal routes in the routing table.

---

# 10. IPv6 Static Routes

Basic format:

```cisco
ipv6 route <destination>/<prefix-length> <next-hop>
```

Example:

```cisco
ipv6 route 2001:db8:0:3::/64 2001:db8:0:12::2
```

This means:

> To reach `2001:db8:0:3::/64`, send traffic to `2001:db8:0:12::2`.

---

# 11. Three Types of Static Routes

## Directly Attached

Only the exit interface is specified:

```cisco
ipv6 route 2001:db8:0:3::/64 g0/0
```

⚠️ **Important IPv6 limitation:**

According to the lesson, directly attached static routes **do not work on Ethernet interfaces** in IPv6.

They can work on interfaces such as serial.

For IPv6 Ethernet, use:

* Recursive static route
* Fully specified static route

---

## Recursive Static Route

Only the **next-hop address** is specified:

```cisco
ipv6 route 2001:db8:0:3::/64 2001:db8:0:12::2
```

The router performs a recursive lookup:

```text
1. Look up destination
        ↓
2. Find next-hop address
        ↓
3. Look up next-hop
        ↓
4. Determine exit interface
```

---

## Fully Specified Static Route

Specify **both**:

* Next-hop
* Exit interface

```cisco
ipv6 route 2001:db8:0:3::/64 2001:db8:0:12::2 g0/0
```

---

# 12. Network vs Host vs Default Route

### Network route

Routes to a subnet:

```cisco
ipv6 route 2001:db8:0:3::/64 ...
```

### Host route

Routes to one specific IPv6 address:

```cisco
ipv6 route 2001:db8:0:3::10/128 ...
```

### Default route

Matches everything:

```cisco
ipv6 route ::/0 ...
```

IPv4 equivalent:

```text
0.0.0.0/0
```

🔥 **Memorize:**

```text
IPv6 default route → ::/0
IPv6 host route    → /128
IPv6 network route → commonly /64
```

---

# 13. Floating Static Route

A **floating static route** is a backup route.

You increase its **Administrative Distance (AD)** so the normal route is preferred.

Example:

```cisco
ipv6 route 2001:db8:0:3::/64 2001:db8:0:12::2 120
```

The `120` is the AD.

### Example

If the primary route is learned through OSPF:

```text
OSPF AD = 110
```

The floating static route must have:

```text
AD > 110
```

If the primary route is EIGRP:

```text
EIGRP AD = 90
```

Then:

```text
AD > 90
```

🔥 **Rule:**

> Floating static route = static route with a **higher AD** than the primary route.

---

# 14. Link-Local Next Hop

You **can use a link-local IPv6 address as a static-route next hop**, but you must specify the exit interface too.

❌ Not enough:

```cisco
ipv6 route 2001:db8:0:3::/64 FE80::2
```

✅ Fully specified:

```cisco
ipv6 route 2001:db8:0:3::/64 FE80::2 g0/0
```

Why?

> The router cannot determine which interface the link-local next hop belongs to by the address alone.

Therefore:

```text
Link-local next hop
        ↓
Next hop + Exit interface
        ↓
Fully specified static route
```

---

# 🔥 Day 33 — CCNA Must-Know

### IPv6 representation

```text
Remove leading zeros
Use :: for longest zero sequence
If tied → use leftmost
Use lowercase a-f
```

### IPv6 header

```text
40 bytes fixed

Version       → 6
Traffic Class → QoS
Flow Label    → Traffic flow
Payload Len   → Layer 4 payload
Next Header   → TCP/UDP/etc.
Hop Limit     → IPv4 TTL equivalent
Source        → 128 bits
Destination   → 128 bits
```

### NDP

```text
NDP → replaces ARP

NS → ICMPv6 135 → ARP request equivalent
NA → ICMPv6 136 → ARP reply equivalent

RS → ICMPv6 133 → asks for routers
RA → ICMPv6 134 → router information
```

### Important multicast

```text
FF02::1 → All nodes
FF02::2 → All routers
```

### Solicited-node multicast

```text
FF02::1:FF + last 6 hex digits
```

### SLAAC

```text
RS → RA → learn prefix → generate IPv6 address
```

### Static routing

```text
Directly attached → exit interface only
Recursive          → next hop only
Fully specified    → next hop + exit interface
Floating           → higher AD
```

### Special routes

```text
IPv6 host route    → /128
IPv6 default route → ::/0
```

### Critical command

```cisco
ipv6 unicast-routing
```

**Big picture:** Day 31 = IPv6 addressing → Day 32 = IPv6 address types → **Day 33 = NDP, SLAAC, and IPv6 static routing.**

