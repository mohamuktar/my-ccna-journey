# VLANs

## 1. Configure the Router

Start with the router and check the interfaces:

```text
do sh ip int br
```

Individually enter each interface and configure the IP address, description, and enable the interface:

```text
int g0/0
ip add 192.3.... subnetmask
desc ## ... ##
no shut
```

Repeat for each interface.

---

## 2. Configure the Switch

```text
SW1>en
SW1#conf t
```

### VLAN 10

```text
SW1(config)#int range g0/1,f3/1,f4/1
SW1(config-if-range)#switchport mode access
SW1(config-if-range)#switchport access vlan 10
```

### VLAN 20

```text
SW1(config-if-range)#int range g1/1,f5/1,f6/1
SW1(config-if-range)#sw mode ac
SW1(config-if-range)#sw ac vlan 20
```

### VLAN 30

```text
SW1(config)#int range g2/1,f7/1,f8/1
SW1(config-if-range)#sw mode ac
SW1(config-if-range)#sw ac vlan 30
```

Check the VLAN table:

```text
SW1(config-if-range)#do sh vl br
```

---

## 3. Rename the VLANs

### VLAN 10

```text
SW1(config-if-range)#vlan 10
SW1(config-vlan)#name ENGINEERING
```

### VLAN 20

```text
SW1(config-vlan)#vlan 20
SW1(config-vlan)#name HR
```

### VLAN 30

```text
SW1(config-vlan)#vlan 30
SW1(config-vlan)#name SALES
```

Verify:

```text
SW1(config-vlan)#do sh vl br
```
