# CCNA Revision Notes: IP Phones, PoE & QoS

## 1. IP Phones and Voice VLANs

- PSTN/POTS: Traditional telephone service.
- VoIP (Voice over IP): Carries voice calls as IP packets over an IP network.
- IP phones commonly have an internal 3-port switch:
  - Uplink: connects to the network switch.
  - Downlink: connects to a PC.
  - Internal port: connects to the phone itself.
- This allows the PC and phone to share one physical switchport.

### Voice VLAN configuration

Separates phone traffic from regular PC data traffic.

```
interface g0/0
 switchport mode access
 switchport access vlan 10
 switchport voice vlan 11
```

- VLAN 10 = data VLAN for the PC (untagged).
- VLAN 11 = voice VLAN for the IP phone (tagged).
- CDP tells the IP phone which voice VLAN to use.
- The switchport remains an access port, not a trunk.

Verification:

```
show interfaces g0/0 switchport
show interfaces trunk
```

## 2. Power over Ethernet (PoE)

PoE supplies electrical power and data over the same Ethernet cable.

- PSE (Power Sourcing Equipment): Usually the switch; supplies power.
- PD (Powered Device): Receives power, such as an IP phone, camera, or wireless access point.
- The switch converts AC power from the outlet into DC power for connected devices.
- The PSE detects the device and determines how much power it needs.

Power policing: Prevents a powered device from drawing too much power.

```
power inline police
```

\- Default action: `err-disable` the port and generate a Syslog message.

```
power inline police action log
```

\- Logs the event and restarts the interface instead of putting it into err-disabled state.

Useful PoE standards:

| Standard     | Type          | Maximum power |
| ------------ | ------------- | ------------- |
| IEEE 802.3af | PoE / Type 1  | 15.4 W        |
| IEEE 802.3at | PoE+ / Type 2 | 30 W          |
| IEEE 802.3bt | Type 3        | 60 W          |
| IEEE 802.3bt | Type 4        | 100 W         |

Know the purpose of PoE and PSE/PD terminology; the lesson says memorizing the full standards table is not essential.

## 3. Quality of Service (QoS)

QoS is a set of tools that gives different treatment and priority to different types of network traffic.

Used especially for delay-sensitive traffic such as voice and video.

QoS manages four characteristics:

| Characteristic  | Meaning                                                                                          |
| --------------- | ------------------------------------------------------------------------------------------------ |
| Bandwidth       | Capacity of a link, measured in bits per second. QoS can reserve bandwidth for specific traffic. |
| Delay (latency) | Time taken for packets to travel from source to destination.                                     |
| Jitter          | Variation in one-way delay between packets.                                                      |
| Loss            | Percentage of packets that do not reach their destination.                                       |

- One-way delay: Source to destination.
- Two-way delay: Source to destination and back.
- High jitter can disrupt audio; IP phones use jitter buffers to smooth packet delivery.
- Packet loss can result from faulty cables or congestion.

Recommended interactive audio targets:

- One-way delay: 150 ms or less
- Jitter: 30 ms or less
- Packet loss: 1% or less

## 4. Queuing and Tail Drop

- A queue stores packets when a device receives traffic faster than it can forward it.
- Default queuing behavior: FIFO (First In, First Out) — packets are forwarded in arrival order.
- When a queue becomes full, new packets are dropped. This is called tail drop.

### TCP Global Synchronization

1. Congestion fills the queue.
2. Tail drop causes packet loss.
3. TCP hosts reduce their transmission rates.
4. The network becomes underutilized.
5. Hosts increase their rates again.
6. Congestion returns, repeating the cycle.

This creates waves of overutilization and underutilization.

## 5. RED and WRED

RED (Random Early Detection) helps prevent tail drop and TCP global synchronization.

- Starts randomly dropping selected TCP packets when queue usage reaches a threshold.
- Causes only affected TCP flows to reduce their sending rates, rather than all flows slowing down together.

WRED (Weighted Random Early Detection):

- An improved version of RED.
- Uses traffic classes to determine which packets are dropped first.
- Lower-priority traffic can be dropped sooner than higher-priority traffic.

CCNA key points to remember: Voice VLANs separate phone and PC traffic; PoE supplies power over Ethernet; QoS prioritizes traffic; FIFO is the default queue behavior; tail drop occurs when a queue is full; RED and WRED help manage congestion.