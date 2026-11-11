# CCNA Revision Notes: Dynamic ARP Inspection (DAI)

## 1. What is DAI?

Dynamic ARP Inspection (DAI) is a Layer 2 switch security feature that inspects and filters ARP messages received on untrusted ports.

- Protects against ARP poisoning attacks.
- Only filters ARP messages; other traffic is unaffected.
- All ports are untrusted by default.
- Trusted ports usually connect to switches, routers, and other network infrastructure.
- End-device ports usually remain untrusted.

## 2. ARP Review

ARP discovers the MAC address associated with a known IP address.

- ARP Request: Broadcast to `FF:FF:FF:FF:FF:FF`.
- ARP Reply: Normally unicast to the device that sent the request.
- ARP messages contain sender and target IP and MAC address fields.
- ARP operates within the local network; it is not routed to external networks.

Gratuitous ARP (GARP):

- An ARP reply sent without receiving an ARP request.
- Sent to the broadcast MAC address.
- Allows other devices to learn the sender's IP-to-MAC mapping.

## 3. ARP Poisoning Attack

An attacker sends fake ARP messages, potentially using another device's IP address.

Example: The attacker sends a Gratuitous ARP claiming to have the default gateway's IP address.

- Other devices update their ARP tables with the attacker's MAC address.
- Traffic intended for the legitimate gateway goes to the attacker first.
- The attacker can inspect or modify the traffic.

This can create a Man-in-the-Middle (MitM) attack.

## 4. How DAI Works

DAI inspects ARP messages received on untrusted ports.

- Checks the ARP message's sender MAC and sender IP.
- Compares them against the DHCP snooping binding table.
- Matching entry → forward the ARP message.
- No matching entry → drop the ARP message.
- Trusted ports → forward without inspection.

DAI verification

Match

Sender MAC + sender IP match the binding table → Forward

No match

No matching binding-table entry → Drop

Important: DAI normally relies on DHCP snooping. Devices using static IP addresses won't automatically have DHCP snooping binding-table entries.

## 5. DAI Configuration
 
Know these commands

Enable DAI for a VLAN:

```
ip arp inspection vlan 1
```

Enable it on every VLAN that requires inspection.

Configure a trusted interface:

```
interface g0/0
 ip arp inspection trust
```

All other interfaces remain untrusted by default.

Remember: Unlike DHCP snooping, DAI does not require a separate global enable command in this configuration.

## 6. DAI Rate Limiting

Limits the number of ARP packets received on an interface.

```
interface g0/1
 ip arp inspection limit rate 25 burst interval 2
```

This allows 25 packets per 2 seconds.

- Default rate on untrusted ports: 15 packets per second.
- Rate limiting is disabled on trusted ports by default.
- If the configured rate is exceeded, the interface becomes err-disabled.
- Helps protect against attackers overwhelming the switch CPU with ARP messages.

The burst interval is optional. Default: 1 second.

Enable automatic recovery:

```
errdisable recovery cause arp-inspection
show errdisable recovery
```

Manual recovery:

```
interface g0/1
 shutdown
 no shutdown
```

## 7. Optional DAI Validation Checks

The standard DAI check compares sender MAC and sender IP against the DHCP snooping binding table.

Additional checks:

| Check     | What it validates                                  |
| --------- | -------------------------------------------------- |
| `dst-mac` | Ethernet destination MAC against ARP target MAC    |
| `ip`      | Invalid or unexpected IP addresses in ARP messages |
| `src-mac` | Ethernet source MAC against ARP sender MAC         |

Configure multiple checks in one command:

```
ip arp inspection validate ip src-mac dst-mac
```

Important: Entering the validation command again overwrites the previous validation settings. Include all checks you want enabled in the same command.

## 8. ARP ACLs

ARP ACLs can provide manual IP-to-MAC mappings for DAI.

Useful for devices with static IP addresses that don't appear in the DHCP snooping binding table.

The instructor identifies detailed ARP ACL configuration as beyond the CCNA exam scope.

## CCNA Quick Review

| Topic               | Remember                                  |
| ------------------- | ----------------------------------------- |
| DAI                 | Inspects ARP messages on untrusted ports  |
| Default port state  | Untrusted                                 |
| Trusted ports       | Usually infrastructure-facing ports       |
| ARP poisoning       | Manipulates ARP tables                    |
| GARP                | ARP reply sent without a request          |
| Standard DAI check  | Sender MAC + sender IP                    |
| Verification source | DHCP snooping binding table               |
| No binding entry    | ARP message dropped                       |
| Static IP devices   | May need ARP ACLs                         |
| Rate limiting       | Excess ARP traffic can cause err-disabled |
| Validation checks   | `ip`, `src-mac`, `dst-mac`                |

Main commands to memorize:

```
ip arp inspection vlan 1
ip arp inspection trust
ip arp inspection limit rate 25
ip arp inspection validate ip src-mac dst-mac

show ip arp inspection
show ip arp inspection interfaces
errdisable recovery cause arp-inspection
```