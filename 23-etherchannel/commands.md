Yes — **EtherChannel is configured on both switches**, but the important thing is that **the two sides must agree on the EtherChannel parameters**.

Think of it like this:

```text
        ASW1                         ASW2
   ┌─────────────┐              ┌─────────────┐
   │             │              │             │
   │   G0/0 ─────┼──────────────┼──── G0/0    │
   │   G0/1 ─────┼──────────────┼──── G0/1    │
   │   G0/2 ─────┼──────────────┼──── G0/2    │
   │   G0/3 ─────┼──────────────┼──── G0/3    │
   │             │              │             │
   └─────────────┘              └─────────────┘
        Po1  ═══════════════════  Po1
```

You configure **Po1 on ASW1** and **Po1 on ASW2**.

### For Layer 2 EtherChannel

For example, using **LACP**:

**ASW1:**

```cisco
conf t

port-channel load-balance src-dst-mac

interface range g0/0 - 3
 channel-group 1 mode active

interface port-channel 1
 switchport mode trunk
```

**ASW2:**

```cisco
conf t

port-channel load-balance src-dst-mac

interface range g0/0 - 3
 channel-group 1 mode active

interface port-channel 1
 switchport mode trunk
```

So yes, **you replicate the configuration on the other switch**, but the physical interfaces are the interfaces **on that particular switch**.

---

### LACP modes

Remember:

| ASW1      | ASW2      | Works?                     |
| --------- | --------- | -------------------------- |
| active    | active    | ✅                          |
| active    | passive   | ✅                          |
| passive   | passive   | ❌                          |
| desirable | desirable | ❌ — that's PAgP            |
| on        | on        | ✅ — static, no negotiation |

For your CCNA notes, **active/active** is the easiest one to remember.

```text
ASW1 active  ←→  ASW2 active
       LACP
```

---

## Your Layer 3 EtherChannel is slightly different

This part is very important:

```cisco
interface range g0/0 - 3
 no switchport
 channel-group 1 mode active

interface port-channel 1
 ip address 10.0.0.1 255.255.255.252
```

The IP address goes on **Port-Channel 1**, NOT on G0/0–G0/3 individually.

Then on the other switch:

```cisco
interface range g0/0 - 3
 no switchport
 channel-group 1 mode active

interface port-channel 1
 ip address 10.0.0.2 255.255.255.252
```

So:

```text
       ASW1                         ASW2
   Po1: 10.0.0.1              Po1: 10.0.0.2
        │                           │
        ╞═══════════════════════════╡
             EtherChannel
          G0/0 G0/1 G0/2 G0/3
```

### Why `R` instead of `S`?

Exactly what you noticed.

```text
show etherchannel summary
```

For Layer 2:

```text
Po1(SU)
```

**S = Layer 2 EtherChannel**

For Layer 3:

```text
Po1(RU)
```

**R = Layer 3 EtherChannel**

The `U` means the port-channel is **in use/up**.

And:

```cisco
show ip interface brief
```

will show:

```text
Port-channel1    10.0.0.1    YES manual    up    up
```

while the physical interfaces themselves are routed members and **don't each get the 10.0.0.1 address**.

### The big picture

Your mental model should be:

**Layer 2 EtherChannel:**

```text
Physical interfaces
      ↓
channel-group 1
      ↓
Port-channel 1
      ↓
switchport mode trunk
```

**Layer 3 EtherChannel:**

```text
Physical interfaces
      ↓
no switchport
      ↓
channel-group 1
      ↓
Port-channel 1
      ↓
IP address
```

So yes: **build the EtherChannel on both switches**, and for a Layer 3 EtherChannel, give each switch's Port-Channel interface an IP from the same subnet.
