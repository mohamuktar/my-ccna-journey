# CCNA Day 25 — RIP & EIGRP

### RIP

```text
RIP = distance-vector IGP - Uses routing by Rumour logic to learn routes
Metric = hop count
Maximum = 15 hops
RIPv1 = classful (❌️VLSM,❌️CIDR - no subnet info) + broadcast
RIPv2 = classless(✅VLSM,✅CIDR - yes subnets info ie  /8 /16 /24) + multicast
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
passive-interface g0/0
alfu weka gateway of last resort by ip route 0.0.0.0 <internet add> in glob config 
default-information originate
show ip protocols
maximum-paths <1-32>
distance <85> - to change AD ie to make RIP preffered over EIGRP OF 90
```

And EIGRP: Which is an Advanced Distance Vector Routing Protocol

```cisco
router eigrp <AS ie 1>
no auto-summary
network
passive-interface

eigrp router-id
show ip protocols
show ip route
maximum-paths <1-32>
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