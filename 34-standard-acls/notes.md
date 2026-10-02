# CCNA Day 34 — Standard ACLs

## 1. What are ACLs?

**ACL = Access Control List**

ACLs act as **packet filters** on routers.

They can:

* **Permit** traffic
* **Deny** traffic
* Filter based on **source/destination IP addresses**
* Filter based on **Layer 4 port numbers** (extended ACLs)

For Day 34, focus on **IPv4 standard ACLs**.

---

## 2. How ACLs Work

ACLs consist of an ordered list of **ACEs (Access Control Entries)**.

Example:

```cisco
ACE 1: permit 192.168.1.0/24
ACE 2: deny 192.168.2.0/24
ACE 3: permit any
```

### Processing order

The router checks entries:

**Top → Bottom**

Once a packet **matches an entry**:

1. Take the specified action.
2. **Stop checking the ACL.**

So **order matters**.

Example:

```text
permit 192.168.1.0/24
deny   192.168.0.0/16
```

`192.168.1.1` is permitted because it matches the first entry.

If reversed:

```text
deny    192.168.0.0/16
permit  192.168.1.0/24
```

`192.168.1.1` is denied because the first matching entry wins.

---

# 3. Implicit Deny

Every ACL has an invisible:

```text
deny any
```

at the end.

Therefore:

```text
permit 1.1.1.1
```

means:

```text
permit 1.1.1.1
deny everything else
```

If you want everything else permitted, add:

```cisco
permit any
```

### 🔥 Important

**Implicit deny exists at the end of every ACL.**

---

# 4. Applying an ACL

Creating an ACL does **not** activate it.

You must apply it to an interface.

```cisco
interface g0/0
ip access-group 1 in
```

or:

```cisco
ip access-group 1 out
```

### Direction

| Direction | Meaning                                  |
| --------- | ---------------------------------------- |
| `in`      | Check packets **entering** the interface |
| `out`     | Check packets **leaving** the interface  |

Think:

```text
IN  → packet enters interface
OUT → packet exits interface
```

### Maximum per interface

You can have:

* **1 inbound ACL**
* **1 outbound ACL**

Maximum = **2 ACLs per interface**.

Applying another ACL in the same direction **replaces** the previous one.

---

# 5. Where to Apply Standard ACLs

### Rule of thumb:

> **Apply standard ACLs as close to the destination as possible.**

Why?

Standard ACLs only examine the **source IP**.

If you place one too close to the source, you might accidentally block that source from accessing **other destinations**.

Example:

```text
PCs → R1 → R2 → SRV1
                  ↑
              destination
```

If you're controlling access to SRV1, place the standard ACL close to SRV1.

---

# 6. ACL Types

Two main types:

| Type         | Matches                            |
| ------------ | ---------------------------------- |
| **Standard** | Source IP only                     |
| **Extended** | Source/destination IP, ports, etc. |

Each can be:

* **Numbered**
* **Named**

### Day 34 focus

**Standard IPv4 ACLs**

---

# 7. Standard Numbered ACLs

Standard ACLs examine **only the source IP address**.

### Number ranges

```text
1–99
1300–1999
```

These are the standard IP ACL ranges you need to remember.

---

## Basic syntax

```cisco
R1(config)#access-list <number> {deny | permit} IP wildcard-mask
```

Example:

```cisco
access-list 1 deny 1.1.1.1 0.0.0.0
```

This denies:

```text
1.1.1.1/32
```

---

# 8. Matching a Single Host

These are equivalent:

```cisco
access-list 1 deny 1.1.1.1 0.0.0.0
```

```cisco
access-list 1 deny 1.1.1.1
```

```cisco
access-list 1 deny host 1.1.1.1
```

All mean:

```text
deny only 1.1.1.1
```

The latter two methods are only for **single hosts (/32)**.

---

# 9. Matching an Entire Network

For:

```text
192.168.1.0/24
```

use the wildcard mask:

```text
0.0.0.255
```

Example:

```cisco
access-list 1 deny 192.168.1.0 0.0.0.255
```

### Remember

ACLs use **wildcard masks**, not subnet masks.

---

# 10. Permit Everything

You can use:

```cisco
access-list 1 permit any
```

Equivalent:

```cisco
access-list 1 permit 0.0.0.0 255.255.255.255
```

So:

```text
0.0.0.0 255.255.255.255
```

matches **all IPv4 addresses**.

---

# 11. ACL Remarks

Remarks are descriptions/comments.

```cisco
access-list 1 remark BLOCK_UNWANTED_HOST
```

They **do not affect traffic**.

Useful for remembering what an ACL is for.

---

# 12. Useful Verification Commands

### Show all ACLs

```cisco
show access-lists
```

### Show only IP ACLs

```cisco
show ip access-lists
```

### Show ACL-related lines in running config

```cisco
show running-config | include access-list
```

### Show an entire ACL section

```cisco
show running-config | section access-list
```

---

# 13. Sequence Numbers

Example:

```text
10 deny 1.1.1.1
20 permit any
```

The numbers indicate the processing order.

The router checks:

```text
10 → 20 → ...
```

If `permit any` comes first:

```text
10 permit any
20 deny 1.1.1.1
```

then `1.1.1.1` is already permitted by entry 10, so entry 20 is never reached.

---

# 14. Standard Named ACLs

Named ACLs still work exactly like **standard ACLs**:

> **Source IP only**

The difference is that they use a **name instead of a number**.

Example:

```cisco
ip access-list standard BLOCK_BOB
```

Now you're in **standard named ACL configuration mode**.

Then:

```cisco
deny host 1.1.1.1
permit any
```

Apply it:

```cisco
interface g0/0
ip access-group BLOCK_BOB out
```

---

# 15. Named ACL Sequence Numbers

You can manually specify sequence numbers:

```cisco
ip access-list standard BLOCK_BOB
5 deny host 1.1.1.1
10 permit any
```

If you don't specify them, they are automatically assigned:

```text
10
20
30
...
```

Manual sequence numbers let you control entry order.

---

# 16. Example Configuration

Requirement:

> PC1 (`192.168.1.1`) can access `192.168.2.0/24`, but other PCs in `192.168.1.0/24` cannot.

### ACL

```cisco
access-list 1 permit 192.168.1.1
access-list 1 deny 192.168.1.0 0.0.0.255
access-list 1 permit any
```

Apply near the destination:

```cisco
interface g0/2
ip access-group 1 out
```

### Why this order?

```text
PC1
 ↓
permit 192.168.1.1
 ↓
Other 192.168.1.x
 ↓
deny 192.168.1.0/24
 ↓
Everything else
 ↓
permit any
```

If the deny came first, PC1 would also be denied.

---

# 17. Standard ACL Processing — Mental Model

When a packet arrives:

```text
Packet
  ↓
Does it enter/exit an ACL-controlled interface?
  ↓
Check ACE #1
  ↓
Match?
 ┌───────┴───────┐
YES              NO
 ↓                ↓
Action          Next ACE
 ↓                ↓
STOP             ...
```

If nothing matches:

```text
→ implicit deny
→ DROP
```

---

# 🔥 CCNA Must-Know

### Standard ACL

```text
SOURCE IP ONLY
```

### Standard ACL number ranges

```text
1–99
1300–1999
```

### Wildcard mask

```text
/24 → 0.0.0.255
```

### Apply ACL

```cisco
ip access-group <ACL> in
ip access-group <ACL> out
```

### Processing

```text
Top → Bottom
First Match → Action → STOP
```

### Implicit deny

```text
deny any
```

### Standard ACL placement

> **As close to the destination as possible.**

### Named standard ACL

```cisco
ip access-list standard NAME
```

### Numbered standard ACL

```cisco
access-list 1 permit ...
```

### Verify

```cisco
show access-lists
show ip access-lists
```

**The biggest Day 34 concepts to lock in:** **source IP only → wildcard masks → first match wins → implicit deny → apply in/out → place standard ACL close to destination.**
