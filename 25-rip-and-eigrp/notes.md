# CCNA Day 25 — RIP & EIGRP

### RIP

```text
RIP = distance-vector IGP
Metric = hop count
Maximum = 15 hops
RIPv1 = classful + broadcast
RIPv2 = classless + multicast
RIPv2 multicast = 224.0.0.9
RIP AD = 120
```

### EIGRP

```text
EIGRP = advanced/hybrid distance vector
Multicast = 224.0.0.10
Metric = bandwidth + delay
Internal AD = 90
External AD = 170
Can do unequal-cost load balancing
Routing table code = D
```

### Wildcard masks

```text
0 = must match
1 = doesn't have to match
```

And know:

```text
/24 → 0.0.0.255
/28 → 0.0.0.15
/32 → 0.0.0.0
```

### Commands worth recognizing

```cisco
router rip
version 2
no auto-summary
network
passive-interface
default-information originate
show ip protocols
maximum-paths
```

And EIGRP:

```cisco
router eigrp <AS>
no auto-summary
network
passive-interface
eigrp router-id
show ip protocols
show ip route
maximum-paths
```

---

## 🧠 The big reason Day 25 matters

Don't look at Day 25 as **"I need to become an expert in RIP and EIGRP."**

Look at it as:

```text
RIP
 │
 ├── Dynamic routing
 ├── Network command
 ├── Passive interfaces
 ├── Default route advertisement
 ├── Administrative distance
 ├── ECMP
 │
 ↓
EIGRP
 │
 ├── Network command
 ├── Passive interfaces
 ├── Wildcard masks
 ├── Router ID
 ├── Administrative distance
 ├── ECMP
 ├── Unequal-cost load balancing
 │
 ↓
OSPF ⭐
 │
 ├── All of the above concepts become useful
 └── THIS is the major CCNA focus
```