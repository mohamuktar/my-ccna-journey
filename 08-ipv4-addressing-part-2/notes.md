# Day 8 — IPv4 & Cisco CLI Revision

## IPv4 Classes

| Class | Prefix | Host Bits | Usable Hosts |
| ----- | ------ | --------: | -----------: |
| A     | `/8`   |        24 |   16,777,214 |
| B     | `/16`  |        16 |       65,534 |
| C     | `/24`  |         8 |          254 |

**Formula:**

```text
2^host bits - 2
```

## Address Types

For `192.168.1.0/24`:

```text
Network:        192.168.1.0
First usable:   192.168.1.1
Last usable:    192.168.1.254
Broadcast:      192.168.1.255
```

Remember:

```text
Network = host bits all 0
Broadcast = host bits all 1
```

## Common Subnet Masks

```text
/8  → 255.0.0.0
/16 → 255.255.0.0
/24 → 255.255.255.0
```

# Cisco IPv4 Configuration

```text

en
conf t
do sh ip int br (doshipintbr)
int g0/0
ip add _____ subnetmask
no shut
do sh ip int br (doshipintbr)
```

Example:

```text
int g0/0
ip address 10.255.255.254 255.0.0.0
no shut
```

Router interfaces are **shutdown by default**, so use:

```text
no shut
```

## Verify

From config mode:

```text
do sh ip int br
```

**Status = Layer 1**
**Protocol = Layer 2**

```text
UP / UP = working
administratively down = shutdown
```

## Other Important Show Commands

```text
show interfaces g0/0
```

Detailed interface information.

```text
show interfaces description
```

Shows interface descriptions.

## Interface Descriptions
Below is for adding description
int g0/0 --- first go to interface mode from global configmode
desc/description ## to SW1 ##
int g0/1
desc/description ## to SW2 ##
int g0/2
desc/description ## to SW3 ##
do sho int desc ( do show interfaces description )


