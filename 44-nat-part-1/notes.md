# CCNA Notes: NAT Part 1 — Static NAT

### 1. Private IPv4 addresses (RFC 1918)

| Range                           | CIDR  |
| ------------------------------- | ----- |
| `10.0.0.0 – 10.255.255.255`     | `/8`  |
| `172.16.0.0 – 172.31.255.255`   | `/12` |
| `192.168.0.0 – 192.168.255.255` | `/16` |

- Private IPs can be reused across different networks.
- They cannot be routed directly over the public Internet.
- CIDR allows flexible subnetting without relying on address classes.

### 2. NAT (Network Address Translation)

- Modifies packet source and/or destination IP addresses.
- Allows private-addressed devices to communicate over the Internet.
- Source NAT: translates the source IP address.
- Static NAT: creates a permanent, one-to-one IP address mapping.

### 3. NAT terminology

- Inside local: Inside host's original IP.
- Inside global: Translated IP representing the inside host externally.
- Outside local: Outside host's address from the inside network's perspective.
- Outside global: Outside host's address from the outside network's perspective.

Without destination NAT, outside local and outside global are usually the same.

### 4. Static NAT configuration

```
interface g0/1
 ip nat inside

interface g0/0
 ip nat outside

ip nat inside source static 192.168.0.167 100.0.0.1
```

Syntax:

```
ip nat inside source static <inside-local> <inside-global>
```

- `ip nat inside` — identifies the internal interface.
- `ip nat outside` — identifies the external interface.
- Static NAT requires one mapped address per inside host.

### 5. Verification commands

| Command                      | Purpose                                             |
| ---------------------------- | --------------------------------------------------- |
| `show ip nat translations`   | Displays NAT mappings and active translations       |
| `show ip nat statistics`     | Displays NAT statistics                             |
| `clear ip nat translation *` | Clears dynamic translations; static mappings remain |

Remember for the exam: RFC 1918 ranges, the four NAT terms, static NAT's one-to-one mapping, configuration syntax, and verification commands.