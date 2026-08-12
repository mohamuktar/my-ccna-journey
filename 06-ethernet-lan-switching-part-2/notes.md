
# CCNA Day 6 — Ethernet LAN Switching (Part 2)

## Ethernet Frame

- Minimum Ethernet frame size: **64 bytes**
- Minimum Ethernet payload: **46 bytes**
- Ethernet header + trailer: **18 bytes**
- If payload < 46 bytes → **padding is added**
- Padding bytes are `00`

### Ethernet Type values

- `0x0800` → IPv4
- `0x0806` → ARP
- `0x86DD` → IPv6

---

## ARP — Address Resolution Protocol

ARP is used to discover a device's **MAC address when its IP address is already known**.

Example:

```text
PC1 knows: PC3 = 192.168.1.3
PC1 doesn't know: PC3's MAC
             ↓
            ARP
             ↓
PC1 learns PC3's MAC
````

### ARP Request

* Sent when the destination MAC is unknown
* **Broadcast**
* Destination MAC:

```text
FFFF.FFFF.FFFF
```

* Essentially asks: **"Who has this IP address?"**

### ARP Reply

* Sent by the device that owns the IP
* **Unicast**
* Sent directly back to the device that made the request
* Contains the device's MAC address

### Memorize

```text
ARP Request → Broadcast
ARP Reply   → Unicast
```

---

## ARP Table

Stores the relationship:

```text
IP Address → MAC Address
```

### Windows / macOS / Linux

```bash
arp -a
```

### Cisco IOS

```cisco
show arp
```

* **Dynamic** = learned through ARP
* **Static** = manually/default configured

---

## Ping / ICMP

`ping` is a network utility used to test reachability.

Ping uses:

```text
ICMP Echo Request
ICMP Echo Reply
```

Example:

```bash
ping 192.168.1.3
```

### Important

ARP happens first if the destination MAC isn't already known.

Therefore, the **first ping can fail** while ARP resolution takes place. Later pings can succeed because the MAC address is now in the ARP table.

---

## Switch MAC Address Table

A switch uses its MAC address table to decide where to send Ethernet frames.

View it with:

```cisco
show mac address-table
```

The table contains:

* VLAN
* MAC Address
* Type
* Port

Dynamic MAC addresses are learned automatically from incoming frames.

### MAC Aging

Dynamic MAC addresses are removed after **5 minutes** if the switch doesn't receive traffic from that MAC.

---

## Switch Frame Behavior

### Known Unicast

Destination MAC is in the MAC table.

→ Forward only through the correct port.

### Unknown Unicast

Destination MAC is NOT in the MAC table.

→ Flood out all ports **except the port where the frame arrived**.

### Broadcast

Destination MAC:

```text
FFFF.FFFF.FFFF
```

→ Flood out all ports **except the port where the frame arrived**.

### Remember

```text
Known Unicast    → One port
Unknown Unicast → Flood
Broadcast       → Flood
```

---

## Clearing Dynamic MAC Addresses

Clear all dynamic MAC addresses:

```cisco
clear mac address-table dynamic
```

Clear a specific MAC:

```cisco
clear mac address-table dynamic address <MAC>
```

Clear dynamic MACs learned on an interface:

```cisco
clear mac address-table dynamic interface gi0/0
```

---

## Wireshark

Wireshark can capture and inspect network traffic.

A first-time ping can look like:

```text
ARP Request
     ↓
ARP Reply
     ↓
ICMP Echo Request
     ↓
ICMP Echo Reply
```

This lets you see exactly what is happening on the network.

---

#  Must-Know Concepts

| Concept                  | Remember                 |
| ------------------------ | ------------------------ |
| ARP                      | IP → MAC                 |
| ARP Request              | Broadcast                |
| ARP Reply                | Unicast                  |
| Ping                     | ICMP Echo Request/Reply  |
| Minimum Ethernet frame   | 64 bytes                 |
| Minimum Ethernet payload | 46 bytes                 |
| IPv4 EtherType           | `0x0800`                 |
| ARP EtherType            | `0x0806`                 |
| IPv6 EtherType           | `0x86DD`                 |
| Broadcast MAC            | `FFFF.FFFF.FFFF`         |
| Known unicast            | Forward to one port      |
| Unknown unicast          | Flood                    |
| Dynamic MAC aging        | 5 minutes                |
| MAC table command        | `show mac address-table` |
| ARP table command        | `show arp`               |


```
