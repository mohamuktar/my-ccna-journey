# CCNA Revision Notes: Wireless Fundamentals

### 1. Wireless basics

- IEEE 802.11: Wireless LAN standards.
- IEEE 802.3: Ethernet standards.
- Wi-Fi Alliance: Certifies 802.11 equipment.
- Wireless uses half-duplex communication.
- CSMA/CA: Avoids collisions in wireless networks by listening before transmitting.
- CSMA/CD: Detects collisions in traditional shared Ethernet.

### 2. RF signal behavior

| Term        | Meaning                                   |
| ----------- | ----------------------------------------- |
| Absorption  | Signal energy is absorbed; signal weakens |
| Reflection  | Signal bounces off surfaces               |
| Refraction  | Signal bends when entering another medium |
| Diffraction | Signal bends around obstacles             |
| Scattering  | Signal spreads in multiple directions     |

- Amplitude: Signal strength.
- Frequency: Cycles per second (Hz).
- Period: Time for one cycle.
- Formula: \\(T = 1/f\\)

### 3. Wi-Fi frequency bands

| 2.4 GHz                 | 5 GHz                   |
| ----------------------- | ----------------------- |
| Greater range           | Generally shorter range |
| Better wall penetration | Less interference       |
| More interference       | Easier channel planning |

\- Wi-Fi 6 (802.11ax) also supports 6 GHz.

### 4. Wi-Fi channels

- 2.4 GHz channels overlap.
- Recommended non-overlapping channels: 1, 6, 11.
- Nearby APs should use different, non-overlapping channels.
- 5 GHz channels are non-overlapping according to the lesson.

### 5. 802.11 standards

| Standard | Wi-Fi generation |
| -------- | ---------------- |
| 802.11n  | Wi-Fi 4          |
| 802.11ac | Wi-Fi 5          |
| 802.11ax | Wi-Fi 6          |

Know the standard names, frequencies and theoretical data rates. Actual speeds are usually lower.

### 6. Service sets and identifiers

| Term  | Meaning                                           |
| ----- | ------------------------------------------------- |
| SSID  | Human-readable wireless network name              |
| BSSID | AP radio's MAC address                            |
| BSS   | Group of wireless clients connected through an AP |
| BSA   | Physical coverage area around an AP               |
| ESS   | Multiple BSSs connected through a wired network   |
| IBSS  | Independent/ad hoc network without an AP          |
| MBSS  | Mesh Basic Service Set                            |

Important distinctions:

- IBSS: Devices communicate directly without an AP.
- BSS: Clients communicate through an AP.
- ESS: Multiple APs, same SSID, unique BSSIDs; supports roaming.
- Roaming: Moving between APs in an ESS without reconnecting.
- ESS APs should use different channels; the lesson recommends 10–15% BSA overlap.

### 7. Mesh networking

- MBSS: Mesh network used when Ethernet connections to every AP are difficult.
- RAP: Root Access Point, connected to the wired network.
- MAP: Mesh Access Point, connects through the mesh backhaul.
- Mesh APs use separate radios for client access and backhaul.

### 8. Distribution System (DS)

- DS: Upstream wired network connecting wireless and wired infrastructure.
- Wireless LANs map to VLANs.
- Multiple SSIDs can map to separate VLANs.
- AP connects to the switch using a trunk when carrying multiple VLANs.

### 9. AP operational modes

| Mode                   | Purpose                                                         |
| ---------------------- | --------------------------------------------------------------- |
| Repeater               | Extends wireless coverage by retransmitting signals             |
| Workgroup Bridge (WGB) | Connects wired clients to a wireless network                    |
| Outdoor Bridge         | Connects distant networks wirelessly using directional antennas |

- Single-radio repeater: same channel; lesson says effective throughput can drop by 50%.
- Dual-radio repeater: receives and retransmits on different channels.
- uWGB: Bridges one wired device.
- Cisco WGB: Proprietary; supports multiple wired clients.
- Outdoor bridge: point-to-point or point-to-multipoint.

Highest-priority exam facts: CSMA/CA, five RF signal behaviors, 2.4 GHz channels 1/6/11, 802.11n/ac/ax, SSID vs BSSID, IBSS vs BSS vs ESS, BSS vs BSA, RAP vs MAP, and AP operational modes.