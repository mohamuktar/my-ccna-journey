# CCNA Day 2 Notes - Interfaces and Cables

# 1. OBJECTIVES:

- How network devices connect
- Interfaces (ports)
- Ethernet cables
- Copper UTP cables
- Fiber-optic cables
- Straight-through vs crossover cables

---

# 2. Network Interfaces

An **interface** is a connection point on a networking device. Another word for interface is **Port**


---

## Switch Interfaces

Switches usually have many interfaces.

Example:

Cisco Catalyst switch:
- 24 ports
- 48 ports


---

# 3. RJ-45
**RJ-45 (RJ stands for:- Registered Jack)** is the standard connector used for copper Ethernet cables.


An RJ-45 connector:
- Has 8 pins
- Connects Ethernet cables to network devices

---

# 4. Ethernet

Ethernet is:- A collection of network protocols and standards used for wired networks.

Ethernet is not one single protocol.

Ethernet defines:
- Cable types
- Speeds
- Connectors
- Communication methods


---


---

# 5. UTP Cable

UTP:- Unshielded Twisted Pair

Used for copper Ethernet connections.

---

# UTP Characteristics

UTP contains:

- 4 twisted pairs
- 8 wires total

---

# Why Are Wires Twisted?

Twisting reduces Electromagnetic interference (EMI)

---

# UTP Limitations

Advantages:

✅ Cheap  
✅ Easy to install  
✅ Common for LAN connections

Disadvantages:

❌ Limited to about 100 meters  
❌ Vulnerable to interference  
❌ Can leak signals

---

# 6. RJ-45 Pins

RJ-45 has: 8 pins

However, not every Ethernet standard uses all pins.

---

# 7. 10BASE-T and 100BASE-T

Uses: 2 pairs = 4 wires

Pins used:

Transmit: 1 and 2

Receive: 3 and 6

---

# 8. Device Pin Behavior

## PC / Router / Firewall

Transmit: Pins 1 and 2
Receive: Pins 3 and 6


---

## Switch

Transmit: Pins 3 and 6
Receive: Pins 1 and 2

---

# 9. Full-Duplex Communication

Full-duplex means: Both devices can send and receive data at the same time.

Because:
- Separate transmit wires
- Separate receive wires

No collisions occur.

---

# 10. Straight-Through Cable

A straight-through cable connects:

Pin 1 → Pin 1
Pin 2 → Pin 2
Pin 3 → Pin 3

The pins are identical on both ends.

---

# Used For:

Different device types:

Examples:
PC -------- Switch
Router -------- Switch
Why?

Because transmit and receive pins are already opposite.

---

# 11. Crossover Cable

A crossover cable reverses transmit and receive pairs.

Example:
Pin 1 → Pin 3
Pin 2 → Pin 6

---

# Used For:

Same device types:

Examples:
Router -------- Router
Switch -------- Switch
PC -------- PC


---

# Why?

Both devices transmit on the same pins.

The cable crosses the signals so:

Transmit → Receive

---

# 12. Auto MDI-X

Modern networking devices use:- Auto MDI-X

Function:
Automatically detects cable type and adjusts communication.

Benefits:
- Straight-through cables can work between similar devices
- Crossover cables often unnecessary

---

# 13. Gigabit Ethernet

## 1000BASE-T

Uses: All 8 wires
Uses all:- 4 pairs

---

## 10GBASE-T

Uses:- All 8 wires

---

Difference:

10BASE-T and 100BASE-T:
- Dedicated transmit/receive pairs

1000BASE-T and 10GBASE-T:
- Each pair can transmit and receive

This allows higher speeds.

---

# 14. Fiber-Optic Cabling

Copper is limited to:- 100 meters


For longer distances:
Use: ✅ Fiber optic cables

---

# Fiber Characteristics

Fiber sends:
- Light signals

through:
- Glass fibers

Instead of:
- Electrical signals through copper

---

# SFP Transceiver

SFP:- Small Form-factor Pluggable

Used to connect fiber cables to networking devices.

Examples:
- Switches
- Routers

---

# Fiber Cable Structure

From inside to outside:

## 1. Core
- Glass center
- Carries light

## 2. Cladding
- Reflects light back into the core

## 3. Buffer
- Protects glass fiber

## 4. Outer Jacket
- Physical protection

---

# 15. Fiber Types

Two major types:
1. Multimode Fiber
2. Single-mode Fiber

---

# Multimode Fiber (MMF)

Characteristics:
- Larger core
- Multiple light paths
- Cheaper
- LED transmitters

Advantages:
✅ Less expensive

Disadvantages:
❌ Shorter distance

---

# Single-Mode Fiber (SMF)

Characteristics:
- Smaller core
- One light path
- Laser transmitter

Advantages:
✅ Long distances

Disadvantages:
❌ More expensive

---

# 16. Fiber Standards

| Standard | Speed | Fiber Type | Distance |
|---|---|---|---|
| 1000BASE-LX | 1 Gbps | MMF/SMF | 550m MMF / 5km SMF |
| 10GBASE-SR | 10 Gbps | MMF | 400m |
| 10GBASE-LR | 10 Gbps | SMF | 10km |
| 10GBASE-ER | 10 Gbps | SMF | 30km |

---

# 17. UTP vs Fiber

| UTP | Fiber |
|---|---|
| Copper cable | Glass fiber |
| Cheaper | More expensive |
| Max ~100 meters | Much longer distances |
| Uses RJ-45 | Uses SFP/transceivers |
| Electrical signals | Light signals |
| Can suffer EMI | Resistant to EMI |
| Can leak signals | Does not leak signals |

---

# Final Day 2 Cheat Sheet

```
RJ-45
↓
Copper Ethernet connector


UTP
↓
Cheap copper cable
100m limit


Switch
↓
Many RJ-45 ports


Straight-through
↓
Different devices


Crossover
↓
Same devices


Auto MDI-X
↓
Automatically fixes cable mismatch


Fiber
↓
Long distance communication


Multimode Fiber
↓
Cheaper, shorter distance


Single-mode Fiber
↓
Expensive, longest distance
```

---

# Day 2 Key Takeaways

- Interfaces are connection points on network devices.
- Ethernet defines networking standards and cable requirements.
- Network speeds are measured in bits per second.
- UTP cables use RJ-45 connectors.
- Straight-through cables connect different device types.
- Crossover cables connect similar device types.
- Auto MDI-X automatically detects cable requirements.
- Fiber optic cables use light instead of electrical signals.
- Multimode fiber is cheaper but shorter distance.
- Single-mode fiber supports the longest distances.