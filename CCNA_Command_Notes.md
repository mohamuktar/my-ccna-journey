# CCNA Command Notes

## Cisco CLI -- IPv4 Addressing

``` cisco
en
conf t
do sh ip int br (doshipintbr)
int g0/0
ip add _____ subnetmask
no shut
do sh ip int br (doshipintbr)
```

### Other show commands

``` cisco
show interfaces g0/0
show interfaces description
```

### Below is for adding description

``` cisco
int g0/0 --- first go to interface mode from global configmode
desc/description ## to SW1 ##
int g0/1
desc/description ## to SW2 ##
int g0/2
desc/description ## to SW3 ##
do sh int desc ( do show interfaces description )
```

## Configuring Interface SPEED & DUPLEX

``` cisco
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

### TO shutdown other interfaces not in use(continuation of above)

``` cisco
int range f0/5 - 12    (interface range {int range f0/5 - 7, f0/10 -12 })
desc ## not in use ##
shutdown
```

### To open a combination of above shutdow interfaces

``` cisco
int range f0/5 - 7, f0/10 -12
no shut
```

### TO show errors

``` cisco
Show interface f0/1 (f0/2 then f0/3)
```

## ROUTER

``` cisco
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

## SWITCH

``` cisco
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

## ROUTING TABLE

``` cisco
show ip route - after configuring an ip add on interface 2 routes are automatically added (local and connected) use show ip route after no shut command to view it or even after done  with one interface before going next
```

## STATIC ROUTE CONFIG (3 ways just use first)

``` cisco
ip route ip-add(of source/dest) netmask next-hop (R1 and R4 i just config 1 route the rest is 2routes)
ip route ip-add netmask exit-interface (static route which u only use exit-interface rely on Proxy ARP to function )
ip route ip-add netmask exit-interface next-hop
```

### ROUTING TABLE (again after done with above)

``` cisco
do show ip route - after configuring an ip add on interface 2 routes are automatically added (local and connected) use show ip route after no shut command to view it or even after done  with one interface before going next
```

## Config Default route

``` cisco
ip route 0.0.0.0 0.0.0.0 ip-add (network and netmask should all be 0)
do show ip route (to see what you have done)
```

## TROUBLESHOOTING

``` text
ipconfig /all or ipconfig (ON THE PCs)
```

``` cisco
do sh ip int br (after using this command and find misconfig where wrong ip add is assigned to a certain interface just enter int and ip add which will override ie int g0/0 then ip add {correct ip})
do sh run | include (ie ip route) --- the use the no ___ (ie check below  )
R1(config)#do sh run | include ip route
ip route 192.168.3.0 255.255.255.0 192.168.12.3 
R1(config)#no ip route 192.168.3.0 255.255.255.0 192.168.12.3 (To disable the wrong config)
R1(config)#ip route 192.168.3.0 255.255.255.0 192.168.12.2   (put the correct config)
R1(config)#do sh run | include ip route
R1(config)#do show ip route (to see the entire table)
```

### To view mac address of a router's interface ie g0/0

``` text
show int g0/0 - then scroll up to see 2mac addresses (1 is assigned anohter is bia)
to support above start by pinging from a pc - go to simulation mode - click it and see the in and out layers (yaani src and dest mac addresses)
```

## VLANS & ACCESS PORTS

start with the router. sort everything out do shipintbr

then individually enter each interface and add 192.3.... subnetmask ,
desc, no shut

after done - go to the switch and check below

``` cisco
SW1>en
SW1#conf t
SW1(config)#int range g0/1,f3/1,f4/1
SW1(config-if-range)#switchport mode access
SW1(config-if-range)#switchport access vlan 10
```

go via all int range check shortforms below

``` cisco
SW1(config-if-range)#int range g1/1,f5/1,f6/1
SW1(config-if-range)#sw mode ac
SW1(config-if-range)#sw ac vlan 20
SW1(config)#int range g2/1,f7/1,f8/1
SW1(config-if-range)#sw mode ac
SW1(config-if-range)#sw ac vlan 30
SW1(config-if-range)#do sh vl br      -- to see the vlan table
```

### NEXT WE WILL CHANGE THE NAME

``` cisco
SW1(config-if-range)#vlan 10
SW1(config-vlan)#name ENGINEERING
SW1(config-vlan)#vlan 20
SW1(config-vlan)#name HR
SW1(config-vlan)#vlan 30
SW1(config-vlan)#name SALES
SW1(config-vlan)#do sh vl br - this is used to view access port infor not trunk like below
```

## VLANS TRunk port (above is access ports)

we start with switch then router - ya switch ndo moto coz switch was 2 -
na for sw1 niliconfig int g0/0. for sw2 i config 2ints g0/0 na g0/1 - so
for switch 2 ilikuwa kitu nimefanya below but twice

``` cisco
SW1(config)#interface g0/0
switchport trunk encapsulation dot1q
switchport mode trunk - this command itakataa if used before above
switchport trunk allowed vlan ? - hii command with ? ni guiding sana (u will see word, add, all,except,none,remove)
switchport trunk allowed vlan 10,30 (switchport trunk allowed vlan add/remove 20)
do show interfaces trunk - This is for anytime you wanna view the trunk infor
switchport trunk native vlan 1001 - for sec purposes change native vlan from 1 to unused vlan
```

Next ni router on a stick - na pia tunaipiga config dont forget it has
one wire to one int ambayo lazima tudivide virtually

malizana na int of switch 2 kwanza which is connected to router 1st

``` cisco
SW2(config)# int g0/2
SW2(config-if)# sw mode trunk
SW2(config-if)# sw trunk allowed vlan 10,20,30
SW2(config-if)# sw trunk native vlan 1001
```

Next ndo tunaenda router sasa

``` cisco
interface g0/0
no shutdown

interface g0/0.10
encapsulation dot1q 10 - hapa ndo tunaipea vlan eg like this is vlan 10
ip address <IP> <subnet-mask> - ip add ni last usable (next subnet -1 = brodcast - 1 = answer)

interface g0/0.20
encapsulation dot1q 20
ip address <IP> <subnet-mask>

interface g0/0.30
encapsulation dot1q 30
ip address <IP> <subnet-mask>
```

### NEXT ni Native VLAN on a router (above ilikuwa on a switch)

2 methods: 1 is the command- encapsulation dot1q vlan-id native (on
router subint) 2. Just config ip add on routers int

So if you use option1 check below: (bolded underlined ndo added )

``` cisco
interface g0/0.10
encapsulation dot1q 10 - hapa ndo tunaipea vlan eg like this is vlan 10
encapsulation dot1q 10 native - just add native compared to above
ip address <IP> <subnet-mask> - ip add ni last usable (next subnet -1 = brodcast - 1 = answer)
```

For above to avoid repetition i can just

``` cisco
interface g0/0.10
encapsulation dot1q 10 native
```

## InterVLAN ROUTING Via SVI (switch virtual interface)

Config PCs to use SVIs (not router) as gateway add

``` cisco
R1(config)#no interface g0/0.10
R1(config)#no interface g0/0.20
R1(config)#no interface g0/0.30
R1(config)#default interface g0/0 - sometime this not used- just no 3 ints, then go int g0/0 then ip add
R1(config)#interface g0/0
R1(config-if)#ip address 192.168.1.194 255.255.255.252
R1(config-if)#do show ip interface brief
```

Ip add successfully configed on router next ni switch

``` cisco
SW2(config)#default interface g0/1
Interface GigabitEthernet0/1 set to default configuration
SW2(config)#ip routing  - this command enables layer 3 routing on switch. DONT FORGET
SW2(config)#interface g0/1
SW2(config-if)#no switchport - This configs the int as routing port
SW2(config-if)#ip address 192.168.1.193 255.255.255.252
SW2(config-if)#do show ip interface brief
SW2(config-if)#exit
SW2(config)#ip route 0.0.0.0 0.0.0.0 192.168.1.194 - default route pointing to R1
SW2(config)#do show ip route
SW2(config)#do show int status
```

Next is SVI config on SW2 😭

``` cisco
do sh vl br - remember svi wont have status of up up if vlan doesnt exit - so first check if they exist
SW2(config)#interface vlan 10 - creates svi for vlan10
SW2(config-if)#ip address 192.168.1.62 255.255.255.192
SW2(config-if)#no shutdown - svis are shutdown by default so no shut enables them
SW2(config-if)#interface vlan 20
SW2(config-if)#ip address 192.168.1.126 255.255.255.192
SW2(config-if)#no shutdown
SW2(config-if)#interface vlan 30
SW2(config-if)#ip address 192.168.1.190 255.255.255.192
SW2(config-if)#no shutdown
```

## DTP & VTP

### DTP (Dynamic Trunking Protocol)

``` cisco
switchport mode dynamic ? option itanipea ni desirable / auto

SW1(config-if)# switchport mode dynamic desirable
SW1(config-if)# do show interfaces g0/0 switchport   

switchportg nonegotiate - disables DTP negotiation
switchport mode access also disables dtp negotiation
its recommended you disable DTP on all switchports and manually conf them as access or trunk ports
show int g0/0 switchport
```

### VTP (VLAN Trunking Protocol)

``` cisco
SW1(config) do show vtp status
3 vtp modes - SW1(config)# vtp mode server/client/transparent
SW1(config) vtp domain cisco
SW1(config)#vlan 10
SW1(config-vlan)#name engineering
SW1(config-vlan)# do show vtp status - u will see the config Revision change to 1 - so it will advert to clients which will synchronize. so when you check other switches you will see name changed to cisco and updated the vlan database to match SW1 with revision number of 1. if a swithch receives a vtp advert ins the same vtp domain with a higher revision number it will update its vlan database to match

to change version - sw1(config)# vtp version 2 ama 3
after addings vlans ndo uone changes - do show vlan brief
a switch in vtp transparent mode will forward vtp adverts but it wont sync its vlan database to vtp server. it also wont advertise it own vlan database
```

## SPANNING TREE PROTOCOL(STP)

STP Enables redundunt layer 2 network

## ETHERCHANNEL Config

### Configure load-balancing

``` cisco
conf t -then - port-channel load-balance <method> ie src-dst-mac - if want 2 see more options use ? before method - this command niko 50/50 - start from below) - to view load balance- do sh etherchannel load-balance {doshethload - shortform}

int g0/0 - 3
channel-group 1 mode passive/active -- so after config etherchannel in whatever mode (above is lacp) next is config interface port-channel - (This command configs int to be part of an Etherchannel - ths command (channel-group 1 mode --) creates port channel interface automatically)
int po1 --- interface port-channel 1
switchport mode trunk
--- repeat above for switch 2 then 
do sh eth sum --- do show etherchannel summary (this is common)  
```

### Layer 3 etherchannel config (ip routing, int range then no switchport etc )

``` cisco
ASW1(config)#interface range g0/0 - 3
ASW1(config-if-range)#no switchport - hii command makes them layer 3 routed interfaces
ASW1(config-if-range)#channel-group 2 mode active -(tushaatumia group 1 above )
coz its layer 3 we need ip add next but where do we config - check below
ASW1(config-if-range)#int po2
ASW1(config-if-range)#ip add <10.0.0.1> <subnet>
do the above for the other layer 3 switch then below
ASW1(config-if-range)#do sh eth sum (do show etherchannel summary) - so when you check instead of the S (layer 2) you will see R which stands for (layer 3)
ASW1(config-if-range)#do sh ip int br - si umeconfig ip add so hii command ndo maana
after - btw- below ni kama extra
exit
ip routing - so that switch can build its routing table
ip route 172.16.2.0 <subnetmask> <nxthop>
do sh ip route
```

## FLOATING STATIC ROUTESd

this is same as static routing but you add Administrative Distance
(AD) - check below

``` cisco
ip route 172.16.2.0 <subnetmask> <nxthop> - this is static route sio but check the diff below
ip route 172.16.2.0 <subnetmask> <nxthop> AD - noma - SO put the AD No higher - if not it will be chosen of the OSPF (which is common and has AD of 110) check belwo
ip route 172.16.2.0 <subnetmask> <nxthop> 111 (higher than 110 of ospf)
so when I - do sh ip route - i WONT see what i configd coz of higher AD - so you'll see ospf selected
```

## OSPF

``` cisco
router ospf 1 or any process ID doesnt matter
network <network> <wildcard-mask> area <area-id ie 0> - network command u can repeat like 2 or 3 times most 3 (each R int)
network 0.0.0.0 255.255.255.255 area 0 - trick of above (instead of 3times)
passive-interface g2/0
ip route 0.0.0.0 0.0.0.0 <next-hop> --- next ni connecting to mneti 
default-route originate
show ip protocols
router-id 1.1.1.1 -- manually config router-id check 
clear ip ospf process
maximum-paths 8 - to change max path from 4
distance 85 - I changed it from 110 - so as to make ospf preffered over eigrp routes
```

### EXAMPLE OF ABOVE OSPF in PRactice

``` cisco
R1(config)#int l0 - this is L0 loopback int
R1(config-if)#ip add 1.1.1.1 255.255.255.255
R1(config-if)#router ospf 1
R1(config-router)#net 0.0.0.0 255.255.255.255 area 0 - shortcut to make all int ospf
R1(config-router)#pass
R1(config-router)#passive-interface l0 - (advertises the Loopback network through OSPF but prevents OSPF hello packets from being sent out of Loopback0) so assuming R1 is connected to service provider and R4 connected to endhosts - give it 2 passive ints. 1 is loopback L0 and the other is the int connected to ISProuter or endhost eg passive-int l0, g0/3. 🤣
R1(config-router)#default-information originate - after this command exit and set up the ip route ---(Advertise my default route (0.0.0.0/0) to my OSPF neighbors)
R1(config-router)#exit
R1(config)#ip route 0.0.0.0 0.0.0.0 203.0.113.2  - (creates that default route)
go to ie last router  - do sh ip route - to see what default route it has in its routing table
```

### ABIT OF SHOW COMMANDS OF OSPF (DNI = Database,Neighbor,Interface)

``` cisco
R1#show ip protocols
R1#show ip ospf database - to see the LSA's in LSDB
R1#show ip ospf neighbor
R1#show ip ospf interface / show ip ospf int g0/0 - to specify
```

### Change Reference Bandwidth

Change Reference Bandwidth coz default refbandwidth is not Ideal why coz
100mbs and above ie,1000mbs all get cost of 1. reference bandwith of 100
divided by 100mbps and above = 1

``` cisco
router ospf 1
auto-cost reference-bandwidth 100000 (3 ways to modify OSPF Cost this is 1. check below)
```

repeat above for all routers

### MANUALLY CONFIG OSPF COST

``` cisco
int g0/0 
ip ospf cost 10000 (This is 2nd method & 3rd not recommended is -- bandwidth ... {in Kilobitspersecond})
do show ip ospf int g0/0  || To see all ints - do show ip ospf int br
```

OSPF states - Demons in Texas Eat Eels like fries

### ACTIVATING OSPF without Using the Network command

so this is cofiguring ospf DIRECTLY on each int

``` cisco
R1(config)#int g0/0
R1(config-if)#ip ospf 1 area 0
```

OSPF can be enabled two ways:

-   Interface method: ip ospf 1 area 0 --- enables OSPF directly on a
    specific interface.
-   Network method: network `<network>`{=html} `<wildcard>`{=html} area
    0 --- matches interfaces whose IP addresses fall within the
    specified range. network 0.0.0.0 255.255.255.255 area 0 matches all
    IPv4 interfaces.(use network method as default)

### Changing ospf priority - influencing DR/BDR

``` cisco
int g0/0
ip ospf priority 255 (255 is the highest)
clear ip ospf process - all neighbors go down. When DR go down BDR takes over. election held 4 next BDR
```

### PPP network type - Serial Int

``` cisco
show controllers s2/0 - know which is DCE vs DTE
int s2/0
clock rate ? 64000 - DCE side needs to specify clockrate to DTE. Ethernet use speed command to config speed serial ints use clock rate
ip add <ip add> <submask>
no shut
show int s2/0
encapsulation ppp - if done then, do on both routers. Changes encapsulation from HDLC to PPP
sh ip ospf neighbor
```

### config ospf hello and DEAD TIMERS

``` cisco
ip ospf hello-interval 10 -(to return to default) - no ip ospf hello-interval (no need to specify 10secs)
ip ospf dead-interval 40- (to return to default) - no ip ospf dead-interval
```

``` cisco
no ip ospf network point-to-point - this command unaremove ppp
show run | section ospf (to show the entire OSPF configuration block) or show run | include ospf (to show only lines containing the word "ospf").
```

## IPV6

``` cisco
R1(config)#ipv6 unicast-routing
R1(config)#int g0/0
R1(config-if)#ipv6 address 2001:db8:0:0::1/64
R1(config-if)#no shutdown
R1(config-if)#
R1(config-if)#int g0/1
R1(config-if)#ipv6 address 2001:db8:0:1::1/64
R1(config-if)#no shutdown
R1(config-if)#
R1(config-if)#int g0/2
R1(config-if)#ipv6 address 2001:db8:0000:0002:0000:0000:0000:0001/64
R1(config-if)#no shutdown
R1# show ipv6 int br
```

### Configure EUI-64

``` cisco
interface g0/0
ipv6 address 2001:db8::/64 eui-64
no shut
R1# show ipv6 int br 
```

### ANYCAST address config

``` cisco
int g0/0
ipv6 address 2001:db8::1/128 anycast
```

### IPV6 Neighbor table

``` cisco
do sh ipv6 neighbor
```

### SLAAC - Stateless Address Auto-configuration

``` cisco
int g0/0
next instead of - ipv6 address 2001:db8::/64 eui-64 =- u write -= ipv6 address autoconfig
```

### STATIC ROUTES

``` cisco
ipv6 route <destination>/<prefix-length> <next-hop/exit-interface> - eg below
ipv6 route 2001:db8:0:3::/64 2001:db8:0:12::2 (this is recursive coz uses nxthop) ---/or-- ipv6 route 2001:db8:0:3::/64 g0/0 (this is directlyattached static route coz it uses exit int) -- {there is also fully specified static route which uses exit int and nexthop respectively} - dont use directly attached static or the one which uses exit int coz in ipv6 it doesnt work with ethernet int and g0/0 is gigEthe....
```

### Link-Local Next Hop

since u specify exit int and nxthop its a fully specified static route

``` cisco
R1(config)#ipv6 route 2001:db8:0:3::/64 (destination address of R3) g0/0 (exit-int of R1) FE80::EF8:22FF:FEE6:D300 (link-local add used as next hop of R2) --- : R1-->R2--->R3 -(when using link-local add as the next hop u have to use fully specified static route)
```

## Numbered ACL CONFIG

``` cisco
access-list 1 deny 192.168.1.0 0.0.0.255 <ip> <wildcard>  ---- (AL1- access-list 1)
access-list 1 permit any
interface g0/0
ip access-group 10 in
R1(config)# do sh access-lists /// do sh ip access-lists - use either
```

## Named ACL CONFIG

``` cisco
ip access-list standard LAN_USERS     ----- (IALS = ip access-list standard)
 deny 192.168.1.0 0.0.0.255
permit any
interface g0/0
ip access-group LAN_USERS in

R1(config)# do sh access-lists /// do sh ip access-lists - use either
```

## CONFIG numbered ACL with subcommands

``` cisco
ip access-list standard 1 --- (IALS = ip access-list standard) - i will use this
deny 192.168.1.0 0.0.0.255
permit any
do sh run | section access-list --> when you the running-config it will display as if you cofigured using the traditional numbered ACL from global confi mode
```

## Resequencing ACLs

``` cisco
ip access-list resequence 1 10 10 ->ip access-list resequence <acl-id> <starting-seq-num increment>
```

## Extended NUMBERED ACL Configuration

``` cisco
R1(config)# access-list <number> [permit | deny] <protocol> <src-ip> <dest-ip>
```

## Extended NAMED ACL Configuration

``` cisco
R1(config)# ip access-list extended {name | number}
R1(config-ext-nacl)# [permit | deny] <protocol> <src-ip> <dest-ip>
```

------------ipv4 protocol numbers - 1=ICMP, 6=TCP, 17=UDP, 88=EIGRP,
89=OSPF

### EXAMPLE

``` cisco
R1(config)# ip access-list extended EXAMPLE --- (IALE)
R1(config-ext-nacl)# deny tcp any 10.0.0.0 0.0.0.255 
R1(config-ext-nacl)# permit ip any any ----> allows all traffic
R1(config-ext-nacl)# deny udp 10.0.0.0 0.0.255.255 host 192.168.1.1--> this denies 10.0.0.0/16 from sending UDP  to 192.168.1.1/32
R1(config-ext-nacl)# deny icmp 172.16.1.1 192.168.0.0  0.0.0.255// deny icmp host 172.16.1.1 192.168.0.0 0.0.0.255--> this denies 172.16.1.1/32 from pinging hosts in 192.168.0.0/24 - look carefully you will learn somethign about the this and the one above
```

### Matching TCP/UDP port No-

``` cisco
R1(config-ext-nacl)# deny tcp <src-ip> <eq/gt/lt/neq/range> <src-port-num> <dest-ip> <eq/gt/lt/neq/range> <dest-port-num> --> eq=equal/ gt= greater than/ lt=less than/ neq=not equal
example: R1(config-ext-nacl)# deny tcp any host 1.1.1.1 eq 80 -> u can specify src and/or dest port details, here i only did for the destination
R1(config-ext-nacl)# permit ip any any ----> allows all traffic
```

## CDP (Cisco Discovery Protocol)

cdp & lldp commands = same replace cdp with lldp

``` cisco
show cdp// show cdp traffic// show cdp interface// show cdp neighbors// show cdp neighbors detail// show cdp entry <neighbor-hostname ie R2>
```

### CDP Config Commands (2know4CCNA)

``` cisco
cdp run --> Enable CDP globally::::::   no cdp run --> disable cdp
Enable CDP on an interface
interface g0/0
cdp enable
Config CDP Timer:--> cdp timer seconds
Config CDP Holdtime--> cdp holdtime seconds
```

### CDP Config Commands (2know4CCNA)

``` cisco
lldp run --> Enable LLDP globally
Enable LLDP on specific int (transmit direction): R1(config-if)# lldp transmit
Enable LLDP on specific int (receive direction): R1(config-if)# lldp receive
                     {enable both transmit &receive}
Config LLDP timer: R1(config)# lldp timer seconds
Config LLDP holdtime: R1(config)# lldp holdtime seconds
Config LLDP reinit timer: R1(config)# lldp reinit seconds
```

## NTP

### Manual Time Configuration - done from priviledged exec mode R1

``` cisco
R1# clock set HH:MM:SS DAY MONTH YEAR
R1# show clock/show clock detail
```

### Hardware Clock (Calendar) Config (skip this go to timezone - packet tracer haisupport)

``` cisco
R1# calendar set HH:MM:SS DAY MONTH YEAR
R1#show calendar
R1#clock update-calendar = whne clock time is okay but for calendar not so - clock update
clock read-calendar -> when calendar time is okay but fo clock is not so - clock read - so you might want to start but show clock/show calendar
```

### Config Time Zone done from global config mode R1(config)# - (Hii below inacover almost all)

``` cisco
R1(config)# clock timezone PST -8
R1(config)# do show clock detail
```

### NTP client Configuration (NTP uses only UTC timezon - you must config appropr TimeZ on each devic)

``` cisco
R1(config)# ntp server 1.1.1.1
R1(config)# do sh ntp associations
R1(config)# do sh ntp stat
R1(config)# int l0 ---> coinfig loopback - (this L0 section not supported by paket tracer so i skipped )
R1(config-if)# ip add 10.1.1.1 255.255.255.255
R1(config-if)#exit
R1(config)#ntp source loopback0
```

NEXt lets go to R2 - we want it to use R1 L0 as its ntp server

``` cisco
R2(config)# ntp server 10.1.1.1 255.255.255.255
R2(config)# do sh ntp associations
R2(config)# do sh ntp status - tunaview stratum number
```

NEXt lets go to R3 - we want it to use R1 L0 as its ntp server

``` cisco
R2(config)# ntp server 10.1.1.1 255.255.255.255
```

### NTP server config - make cisco dev a server when there is no server to sync to (different&new from above) - hii ni fast compared to above

``` cisco
R1(config)# ntp master -> the default stratum No of ntp master command is 8. if asked to do with authentication the below is important otherwise skip and go below - start from - R2(config)#ntp server 192.168.12.1)
R1(config)#ntp authenticate
R1(config)#ntp authentication-key 1 md5 moha
R1(config)#ntp trusted-key 1
```

next we go to R2 and make it use R1 as ntp server with authentication

``` cisco
R2(config)#ntp authenticate
R2(config)#ntp authentication-key 1 md5 moha
R2(config)#ntp trusted-key 1
R2(config)#ntp server 192.168.12.1 key 1
R2(config)# do sh ntp associations
R2(config)#ntp update-calendar ---> update calendar bro
```

next we go to R3 and make it use R1 as ntp server (if not asked for
authentication just skip to ntp server 192.168.13.1 without key 1 then
do sh ntp associations - same for above r2)

``` cisco
R3(config)#ntp authenticate
R3(config)#ntp authentication-key 1 md5 moha
R3(config)#ntp trusted-key 1
R3(config)#ntp server 192.168.13.1 key 1
R3(config)# do sh ntp associations
R3(config)#ntp update-calendar ---> update calendar bro
```

### NTP SYMMETRIC ACTIVE MODE (ie btwn R2 and R3) - configed R2 as R3 peer and vice versa

``` cisco
R2(config)# ntp peer 10.0.23.2 (this is ip add is for R3 directly connected int)
R2(config)# do show ntp associations
R3(config)#ntp update-calendar
```

netx R3

``` cisco
R3(config)# ntp peer 10.0.23.2 (this is ip add is for R2 directly connected int)
R3(config)# do show ntp associations
```

## DNS - (Hii commands zake sifai hadi kujua sana so ..)

-   **DNS:** Resolves domain names to IP addresses.
-   **DNS server:** Answers DNS queries.
-   **DNS client:** Queries a DNS server to resolve names.
-   `ip host PC1 192.168.1.10` --- Creates a local hostname-to-IP
    mapping.
-   `ip dns server` --- Configures the router to act as a DNS server.
-   `ip name-server 8.8.8.8` --- Specifies a DNS server for the router
    to query.
-   `ip domain lookup` --- Enables DNS lookups on the router.
-   `ip domain name example.com` --- Configures the router's domain
    name.
-   `show hosts` --- Displays local host mappings and hostname cache
    information.

**Exam priority:** Understand DNS operation, client vs. server roles,
and basic configuration purposes. Prioritize concepts over memorizing
every command.

## DHCP

### DHCP Server

``` cisco
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10 (not necessary but okay)
R1(config)# ip dhcp pool POOL1
R1(dhcp-config)# network 192.168.1.0 255.255.255.0
R1(dhcp-config)# default-router 192.168.1.1
R1(dhcp-config)# dns-server 8.8.8.8
R1(dhcp-config)# lease 0 5 30 (0days, 5hrs, 30mins)
R1# show ip dhcp binding
de
```

### DHCP Relay (client-facing interface)

``` cisco
interface g0/1
 ip helper-address 192.168.10.10
 show ip interface g0/1
```

### DHCP Client (interface configuration mode)

``` cisco
interface g0/1
 ip address dhcp
no shut 
```

## SSH

### Console Port security - login (Line → Password → Login)

``` cisco
line console 0
 password ccna
 login
end
```

### Console Port security - login local (Username → Line → Login local) - (U-LI-LOLO)

``` cisco
username moha secret ccna
line console 0
 login local
```

### Layer 2 switch - Management IP -assign ip add to SVI to allow remote connec to CLI of switch - same as multilayer switch

``` cisco
SW1(config)# interface vlan1
SW1(config-if)# ip add 192.168.1.253 255.255.255.0
SW1(config-if)# no shut
SW1(config-if)# exit
SW1(config)# ip default-gateway 192.168.1.254
```

### TELNET CONFIG

``` cisco
enable secret ccna
username moha secret ccna
line vty 0 15
 login local
 transport input telnet
```

### SSH (ssh username@ip-address)

``` cisco
hostname SW1
ip domain name moha.com
crypto key generate rsa modulus 2048

enable secret ccna
username moha secret ccna

line vty 0 15
 login local
 transport input ssh
```

### Verification

``` cisco
show version
show ip ssh
show ip interface brief
show running-config
```

## Static NAT configuration (ip nat inside source static `<inside-local>`{=html} `<inside-global>`{=html}

``` cisco
R1(config)# int g0/1
 ip nat inside
R1(config)# int g0/0
 ip nat outside
exit - exit coz the below command is for global confiig not global int config
R1(config)# ip nat inside source static 192.168.0.167 100.0.0.1
R1(config)# ip nat inside source static 192.168.0.168 100.0.0.2
show ip nat translations
show ip nat statistics
R1#clear ip nat translation *  -> clear dynamic entries or if u in config mode start with - do clear ip nat traslation *
```

## DYNAMIC NAT

``` cisco
R1(config)# int g0/1
 ip nat inside
R1(config)# int g0/0
 ip nat outside
exit - exit coz the below command is for global confiig not global int config

R1(config)# access-list 1 permit 192.168.0.0   0.0.0.255 - traffic permitted by this ACL willbe translat
R1(config)# R1(config)#ip nat pool POOL1 100.0.0.1 100.0.0.2 netmask 255.255.255.0
R1(config)# ip nat inside source list 1 pool POOL1 --- dynamic NAT and PAT have same configs from up till this command where after POOL1 you just add (overload). unlike the one-to-one mapping PAT uses many-to-one mapping. 

Or another and more common way of PAT config is via mapping the ACL to interface so start from top - 
R1(config)# int g0/1
 ip nat inside
R1(config)# int g0/0
 ip nat outside
exit 

R1(config)# access-list 1 permit 192.168.0.0   0.0.0.255 
R1(config)# ip nat inside source list 1 interface g0/0 overload

show ip nat translations
show ip nat statistics
R1#clear ip nat translation *  -> clear dynamic entries
```

## IP PHONES/VOICE VLAN

same as access ports/untagged port with +1 command

``` cisco
int g0/0
switchport mode access
switchport access vlan 10
switchport voice vlan 11
show int g0/0 switchport
```

## QOS

For QOS Content do flashcards - waah alot of content

## Switchport Sec

### Enabling Port sec

``` cisco
SW1(config)# int g0/1
switchport port-security - it will be rejected because of dynamic auto 
do sh int g0/1 switchport - this will show its dynamic auto so below will command will change it to access - u can skip this and above too
switchport mode access
switchport port-security - this enables port sec
do show port-sec int g0/1 - shows default settings of port-sec of int g0/1
```

### Manually Re-enabling int after err-disable

``` cisco
int g0/1
shut
no shut
```

### Re-enabling int (errdisable recovery)

step 1 disconnect the unauthorized device then ... manually reenable the
int or let errdisable recovery do it 4 u automatically

``` cisco
SW1(config)# errdisable recovery cause psecure-violation
SW1(config)# errdisable recovery interval 180 - 
```

### VIOLATION MODE: RESTRICT

``` cisco
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security mac-address 000a.000a.000a
SW1(config-if)# switchport port-security violation restrict 
sh port-security int g0/...
```

for restrict mode - port status is up, syslog/SNMP message generated,
violation count \^ncrease by 1 for each unauthorized frame

### violation mode: Protect

``` cisco
SW1(config-if)# switchport port-security
SW1(config-if)# switchport port-security mac-address 000a.000a.000a
SW1(config-if)# switchport port-security violation protect
```

for restrict mode - port status is up, no syslog/SNMP, no increment in
violation count

### Secure mac address aging

``` cisco
SW1(config-if)# switchport port-security aging time 30
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''aging type inctivity/absolute
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''aging static
sh port-security int g0/...
```

### sticky secure Mac-address

dont age out and saved to running config so after done use write memory
command lol - away of config static mac add without manually doing it
but they are dynamically learned = dynamic add

``` cisco
SW1(config-if)# switchport port-security - kama kawa enable port-sec first 
SW1(config-if)# switchport port-security mac-address sticky
write
```

## DHCP Snooping Configuration

``` cisco
ip dhcp snooping
ip dhcp snooping vlan 1
no ip dhcp snooping information option - remove option 82
Configure the interface connected to the legitimate DHCP server as trusted:
interface g0/0
 ip dhcp snooping trust
show ip dhcp snooping binding
```

### DHCP Snooping Rate Limiting

``` cisco
SW1(config)# int range g0/1 - 3
SW1(config-if-range)# ip dhcp snooping limit rate 1 - if it receives more than 1 dhcp message per second it will be errdisable. To enable errdisabled int check below
SW1(config)# errdisable recovery cause dhcp-rate-limit
show errdisable recovery
```

## DAI CONFIG

``` cisco
ip arp inspection vlan 1 - if have more than 1 vlan enable on them too
int range g0/0 - 1 = if have 2 ints trusted on switch (int connected to router and int connected to other switch)/ int g0/0 (next)
ip arp inspection trust
show ip arp inspection interfaces
```

### DAI RATE LIMITING

``` cisco
int range g0/0 - 1 
ip arp inspection limit rate 25 (burst interval 2 = optional- skip mzee)
To enable errdisabled int check below
 errdisable recovery cause arp-inspection
show errdisable recovery
```

### enabling all validation option checks

``` cisco
SW2(config)# ip arp inspection validate dst-mac ip src-mac (ukiguza tab after validate it will show u options so dont cram)
```

## DAY 56 - wireless architecture

I recently took the test and had more than 12 questions related to WLCs
specifically interactive questions where you have to manually set a
requirement for a network. DO NOT SKIP THIS SECTION, this is on par with
STP, OSPF and Subnetting for importance. Highly recommend using outside
sources for there section of the course.
