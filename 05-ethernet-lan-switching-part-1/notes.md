# Day 05 — Ethernet LAN Switching

## Key Concepts

### OSI Layers
- **Layer 1 — Physical:** Sends bits as electrical/radio signals.
- **Layer 2 — Data Link:** Provides node-to-node communication using **MAC addresses**.
- **Switches operate primarily at Layer 2.**
- **IP addresses = Layer 3**
- **MAC addresses = Layer 2**

### LAN
- **LAN (Local Area Network):** Network within a relatively small area.
- **Switches expand a LAN.**
- **Routers separate different LANs.**

## Ethernet Frame

| Field | Size | Purpose |
|---|---:|---|
| Preamble | 7 bytes | Receiver clock synchronization |
| SFD | 1 byte | Marks end of preamble/start of frame |
| Destination MAC | 6 bytes | Destination device |
| Source MAC | 6 bytes | Sending device |
| Type/Length | 2 bytes | Identifies protocol or packet length |
| FCS | 4 bytes | Detects transmission errors |

### Important
- **Preamble:** 7 bytes of alternating `10101010`
- **SFD:** 1 byte, `10101011`
- **FCS:** Uses **CRC (Cyclic Redundancy Check)** to detect corrupted data.
- IPv4 EtherType = `0x0800`
- IPv6 EtherType = `0x86DD`

## MAC Addresses

- MAC = **Media Access Control**
- **48 bits / 6 bytes**
- Written as **12 hexadecimal digits**
- Physical Layer 2 address.
- Also called a **Burned-In Address (BIA)**.
- Normally globally unique.
- First **24 bits (3 bytes)** = **OUI (Organizationally Unique Identifier)**
  - Identifies the manufacturer.
- Last **24 bits (3 bytes)** = identifies the individual device.

### Hexadecimal
- Uses **16 values:** `0–9` and `A–F`
- `A = 10`
- `B = 11`
- `C = 12`
- `D = 13`
- `E = 14`
- `F = 15`

## How a Switch Learns MAC Addresses

Switches maintain a **MAC address table**.

When a frame arrives:

1. Switch looks at the **SOURCE MAC address**.
2. It records the source MAC.
3. It associates that MAC with the **interface the frame arrived on**.
4. This is called **dynamic MAC learning**.

> **MEMORIZE: Switches learn SOURCE, forward using DESTINATION.**

## Unknown vs Known Unicast

### Unknown Unicast
- Destination MAC is **NOT** in the MAC address table.
- Switch **floods** the frame.
- Sends it out **all interfaces except the interface it arrived on**.

### Known Unicast
- Destination MAC **IS** in the MAC address table.
- Switch forwards the frame **only through the correct interface**.
- No flooding.

### Example

```text
PC1 → Switch → PC2

First frame:
PC1 → Switch
        ↓
   Destination unknown
        ↓
      FLOOD
     ↙     ↘
   PC2     PC3

After PC2 replies:
Switch learns PC2's MAC.

Next frame:
PC1 → Switch → PC2
              ↑
        Known unicast