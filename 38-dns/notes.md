# CCNA Revision Notes — DNS

- DNS (Domain Name System): Resolves domain names to IP addresses.
- DNS port: UDP/53 typically; TCP/53 is also used.
- A record: Name → IPv4 address.
- AAAA record: Name → IPv6 address.
- CNAME: Maps one name to another name.
- DNS cache: Stores previous responses to reduce queries.
- Hosts file: Manually maps hostnames to IP addresses.
- DHCP: Can provide clients with their DNS server address.

## Key Commands

Windows

```
ipconfig /all
nslookup youtube.com
ipconfig /displaydns
ipconfig /flushdns
```

Cisco IOS — DNS client

```
ip name-server 8.8.8.8
ip domain lookup
ip domain name example.com
```

Cisco IOS — DNS server

```
ip dns server
ip host PC1 192.168.1.10
show hosts
```

## Remember

- Routers normally forward DNS traffic; they don't need DNS configured to route it.
- `ip name-server` specifies the DNS server the router queries.
- `ip dns server` enables the router to answer DNS queries using its host table and learned records.
- `show hosts` displays configured and cached host entries.