# 🖥️ Cisco CLI — IPv4 Addressing

```text
en
conf t
do sh ip int br (doshipintbr)
int g0/0
ip add _____ subnetmask
no shut
do sh ip int br (doshipintbr)
```

---

## 🔎 OTHER show commands

```text
show interfaces g0/0
show interfaces description
```

### 📝 Below is for adding description

```text
int g0/0 --- first go to interface mode from global configmode
desc/description ## to SW1 ##

int g0/1
desc/description ## to SW2 ##

int g0/2
desc/description ## to SW3 ##

do sh int desc ( do show interfaces description )
```

---

# ⚙️ Configuring Interface SPEED & DUPLEX

```text
En
Conf t
do sh ip int br (doshipintbr)
do sh int stat (Do show interfaces status)

int f0/1
speed 100
duplex full
desc ## to R1 ##

do sh int stat (Do show interfaces status)
```

---

# 🔴 TO shutdown other interfaces not in use

```text
int range f0/5 - 12    (interface range {int range f0/5 - 7, f0/10 -12 })
desc ## not in use ##
shutdown
```

### 🟢 To open a combination of above shutdow interfaces

```text
int range f0/5 - 7, f0/10 -12
no shut
```

---

# ❌ TO show errors

```text
Show interface f0/1 (f0/2 then f0/3)
```

---

# 🛜 ROUTER

```text
en
conf t
do sh ip int br (doshipintbr)
int g0/0
ip add _____ subnetmask
desc ##    to .... #
speed ...
duplex (full)
no shut
do sh ip int br (doshipintbr)

R1(config)#int g0/0
R1(config-if)#int range g0/1 - 2
R1(config-if-range)#desc ## not in use ##
```

---

# 🔀 SWITCH

```text
do sh int stat (Do show interfaces status)

SW1(config)#int g0/1
SW1(config-if)#speed 1000
SW1(config-if)#duplex full
SW1(config-if)#desc ## to R1 ##

SW1(config-if)#int g0/2
SW1(config-if)#speed 1000
SW1(config-if)#duplex full
SW1(config-if)#desc ## to SW2 ##

SW1(config-if)#int range f0/1 - 2 (this connected to endhost i dont need to setup speed and duplex)
SW1(config-if-range)#desc ## to end hosts ##

SW1(config-if-range)#int range f0/3 - 24  (shutting down all other int not in use)
SW1(config-if-range)#desc ## not in use ##
SW1(config-if-range)#shutdown (after hiii command alot of stuff get printed to on screeen)

SW1(config-if-range)#do sh int stat (same for this one)
```
