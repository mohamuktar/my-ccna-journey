# CCNA Revision Notes: Dynamic NAT and PAT

## 1. Static NAT review

- Static NAT: Permanently maps one inside local IP address to one inside global IP address.
- Translates the source IP for outgoing traffic and the destination IP for return traffic.
- Allows external hosts to reach the internal host using its mapped inside global address.

## 2. Dynamic NAT

Dynamic NAT automatically assigns an inside global IP address from a configured pool when needed.

Key points:

- Uses an ACL to identify which source IP addresses should be translated.
- Uses a NAT pool to define available inside global addresses.
- Creates temporary one-to-one mappings.
- ACL-denied traffic is not translated, but is not automatically dropped by this NAT ACL.
- If all pool addresses are in use, NAT pool exhaustion occurs and new traffic requiring translation is dropped.
- Unused mappings time out, allowing addresses to be reused.

### Dynamic NAT configuration

```
interface g0/0
 ip nat outside

interface g0/1
 ip nat inside

access-list 1 permit 192.168.0.0 0.0.0.255

ip nat pool POOL1 100.0.0.1 100.0.0.10 netmask 255.255.255.0

ip nat inside source list 1 pool POOL1
```

Remember: the inside/outside interfaces, ACL, pool, and NAT mapping must be configured.

## 3. PAT — Port Address Translation

Also called NAT overload.

PAT translates IP addresses and uses port numbers to distinguish traffic flows, allowing many inside hosts to share one public IP address simultaneously.

- Tracks connections using unique source port numbers.
- If two hosts use the same source port, the router can assign a different translated port.
- If their ports are already unique, port translation may not be necessary.
- PAT is the most widely used NAT type in the lesson because it conserves public IPv4 addresses.

### PAT using a NAT pool

Add `overload` to the dynamic NAT command:

```
ip nat inside source list 1 pool POOL1 overload
```

This allows multiple inside local addresses to share addresses from the pool.

### PAT using the outside interface address

A common alternative is to use the router's outside interface IP address:

```
ip nat inside source list 1 interface g0/0 overload
```

This lets multiple internal hosts share the IP address assigned to `g0/0`.

## 4. Verification and troubleshooting commands

| Command                      | Purpose                                           |
| ---------------------------- | ------------------------------------------------- |
| `show ip nat translations`   | View active NAT translations                      |
| `show ip nat statistics`     | View NAT statistics and configuration information |
| `clear ip nat translation *` | Clear dynamic NAT translations                    |

The lesson notes that dynamic mappings can have a default timeout of 24 hours, while temporary UDP/ICMP entries may clear after about a minute.

## 5. Know the differences

| Feature          | Static NAT          | Dynamic NAT                      | PAT                         |
| ---------------- | ------------------- | -------------------------------- | --------------------------- |
| Mapping          | One-to-one          | One-to-one                       | Many-to-one possible        |
| Assignment       | Manually configured | Automatically assigned from pool | Automatically tracks flows  |
| Public IP usage  | One per mapped host | One per active mapping           | Many hosts can share one IP |
| Mapping duration | Permanent           | Temporary                        | Tracks active traffic flows |

CCNA memory tip: Static = fixed mapping; Dynamic NAT = temporary one-to-one mapping; PAT = many hosts share an IP using port numbers.