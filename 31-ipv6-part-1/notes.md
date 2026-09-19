# IPv6 Part 1 — CCNA Revision Notes

## 🔥 CCNA Exam Focus

IPv6 appears in:

* **1.8** — Configure and verify IPv6 addressing and prefixes
* **1.9** — Compare IPv6 address types
* IPv6 static routing is also required later.

This lesson focuses mainly on **IPv6 addresses, prefixes, abbreviation, and basic configuration**.

---

# 1. Hexadecimal Review

IPv6 uses **hexadecimal (base 16)**.

| Number system | Base | Digits     |
| ------------- | ---: | ---------- |
| Binary        |    2 | `0–1`      |
| Decimal       |   10 | `0–9`      |
| Hexadecimal   |   16 | `0–9, A–F` |

### Hex values to memorize 🔥

| Decimal | Hex |
| ------: | --: |
|      10 |   A |
|      11 |   B |
|      12 |   C |
|      13 |   D |
|      14 |   E |
|      15 |   F |

### Important relationship

**1 hexadecimal digit = 4 bits**

```text
Binary: 1111
Decimal: 15
Hex: F
```

Therefore:

```text
8 bits = 2 hex digits
128 bits = 32 hex digits
```

---

# 2. Binary ↔ Hex Conversion

### Binary → Hex

Split into groups of **4 bits**.

Example:

```text
1101 1011
 ↓     ↓
 D     B
```

Therefore:

```text
11011011 = DB
```

### Hex → Binary

Convert each hex digit into **4 bits**.

```text
EC
↓ ↓
1110 1100
```

Therefore:

```text
EC = 11101100
```

🔥 **Key trick:**
**1 hex digit ↔ 4 binary bits**

---

# 3. Why IPv6?

The main reason:

> **IPv4 address space is not large enough.**

IPv4:

```text
32 bits
≈ 4.29 billion addresses
```

IPv6:

```text
128 bits
≈ 340 undecillion addresses
```

IPv4 conservation techniques include:

* VLSM
* Private IPv4 addresses
* NAT

These help preserve IPv4 space, but **IPv6 is the long-term solution**.

---

# 4. IPv6 Address Structure

IPv6 addresses are:

* **128 bits**
* Written in **hexadecimal**
* Divided into **8 groups**
* Each group contains **4 hex digits**
* Groups are separated by `:`

Example:

```text
2001:0db8:0000:0000:0000:0000:0000:0001
```

That's:

```text
8 groups × 16 bits = 128 bits
```

IPv6 uses **prefix length notation**:

```text
2001:db8::1/64
```

No dotted-decimal subnet mask is used.

---

# 5. IPv6 Address Abbreviation 🔥

There are **two ways** to shorten an IPv6 address.

## Method 1 — Remove Leading Zeros

You can remove zeros at the **beginning of each quartet**.

Example:

```text
2001:0db8:0001:0002:0000:0000:0000:0001
```

Becomes:

```text
2001:db8:1:2:0:0:0:1
```

🔥 You can remove **leading** zeros only.

---

## Method 2 — Replace Consecutive `0000` Quartets with `::`

Example:

```text
2001:db8:0:0:0:0:0:1
```

Becomes:

```text
2001:db8::1
```

### ⚠️ Important rule

You can use `::` **only once** in an IPv6 address.

Why?

Because `::` represents an unknown number of all-zero quartets. Using it twice would make the address ambiguous.

---

# 6. Expanding IPv6 Addresses

To expand a shortened address:

### Step 1

Add leading zeros so every quartet has **4 hex digits**.

### Step 2

Replace `::` with enough `0000` quartets to make **8 total quartets**.

Example:

```text
2001:db8::1
```

Expanded:

```text
2001:0db8:0000:0000:0000:0000:0000:0001
```

🔥 **Remember:**

```text
IPv6 = 8 quartets
Each quartet = 4 hex digits
```

---

# 7. Finding the IPv6 Prefix

Same basic concept as IPv4:

> **Set all host bits to 0.**

### Common `/64`

For a `/64`, the first 64 bits are the network portion.

```text
2001:db8:1234:5678:abcd:ef01:2345:6789/64
```

Prefix:

```text
2001:db8:1234:5678::/64
```

---

## Prefix Length Is a Multiple of 4

This is easy because:

```text
1 hex digit = 4 bits
```

Example `/56`:

```text
16 + 16 + 16 + 8 = 56
```

So the first **14 hex characters** are the network portion.

Everything after that becomes `0`.

---

# 8. Prefix Length NOT a Multiple of 4

This is where **binary** becomes important.

Example:

```text
/93
```

`93` isn't divisible by 4.

You find the last affected hexadecimal digit, convert it to binary, and set the **host bits to 0**.

Example from the lesson:

```text
B = 1011
```

If only the first bit belongs to the network:

```text
1011
↑
network bit
```

Set remaining host bits to `0`:

```text
1000
```

Which equals:

```text
8
```

So the `B` becomes `8`.

🔥 **For non-multiple-of-4 prefixes, use binary.**

---

# 9. IPv6 Global Routing Prefix

An enterprise will typically receive a **/48** block from its ISP.

Typical IPv6 subnet:

```text
/64
```

Therefore:

```text
/48 → /64
```

The enterprise has:

```text
64 - 48 = 16 bits
```

available for the **Subnet ID**.

### Structure

```text
|------ 48 bits ------|-- 16 bits --|-------- 64 bits --------|
| Global Routing Prefix| Subnet ID   |       Host Interface   |
```

So:

* `/48` = Global Routing Prefix
* Next `16 bits` = Subnet Identifier
* Last `64 bits` = Host portion

🔥 **Common pattern:**

```text
ISP gives /48
       ↓
Enterprise creates /64 subnets
       ↓
16 bits available for subnetting
```

---

# 10. IPv6 Address Configuration

First enable IPv6 routing:

```cisco
ipv6 unicast-routing
```

Without this, the router won't forward IPv6 packets.

### Configure an interface

```cisco
interface g0/0
ipv6 address 2001:db8:0:0::1/64
no shutdown
```

Another example:

```cisco
interface g0/1
ipv6 address 2001:db8:0:1::1/64
no shutdown
```

---

# 11. Verify IPv6

Use:

```cisco
show ipv6 interface brief
```

This shows IPv6 addresses assigned to interfaces.

You may notice each interface has **two IPv6 addresses** even though you configured only one.

The additional address is the **link-local address**, which is automatically configured when IPv6 is enabled on the interface.

⚠️ Address types, including link-local addresses, are covered in a later IPv6 lesson.

---

# 🧠 Day 31 — Must Memorize

### IPv6 basics

```text
IPv6 = 128 bits
8 quartets
4 hex digits per quartet
1 hex digit = 4 bits
```

### Abbreviation

```text
Remove leading 0s
        +
Replace consecutive 0000 quartets with ::
```

🔥 **`::` can only appear ONCE.**

### Prefix

```text
Network bits → keep
Host bits → change to 0
```

### Common enterprise allocation

```text
/48 = Global Routing Prefix
/64 = Typical subnet
16 bits = Subnet ID
64 bits = Host portion
```

### Router configuration

```cisco
ipv6 unicast-routing

interface g0/0
ipv6 address 2001:db8::1/64
no shutdown

show ipv6 interface brief
```

### 🔥 Highest-priority CCNA concepts

1. **128-bit IPv6 address**
2. **Hexadecimal**
3. **1 hex digit = 4 bits**
4. **IPv6 abbreviation/expansion**
5. **`::` only once**
6. **Finding IPv6 prefixes**
7. **/48 → /64**
8. **`ipv6 unicast-routing`**
9. **`ipv6 address .../prefix-length`**
10. **`show ipv6 interface brief`**
