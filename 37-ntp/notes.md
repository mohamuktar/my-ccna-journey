# CCNA Revision Notes — Time and NTP

## 1. Why Time Matters

Accurate time is important for:

* Log correlation: Match events across multiple devices.

* Troubleshooting: Determine the order of network events.

* Security: Investigate attacks and suspicious activity.

* Authentication: Some security protocols depend on synchronized time.

## 2. commands

NTP
show clock/show clock detail =>internal hardware clock
show logging --> to view system logs
Manual Time Configuration - done from priviledged exec mode R1#
R1# clock set HH:MM:SS DAY MONTH YEAR
R1# show clock
Hardware Clock (Calendar) Config
R1# calendar set HH:MM:SS DAY MONTH YEAR
R1#show calendar
R1#clock update-calendar = whne clock time is okay but for calendar not so - clock update
clock read-calendar -> when calendar time is okay but fo clock is not so - clock read - so you might want to start but show clock/show calendar

Config Time Zone done from global config mode R1(config)# - (Hii below inacover almost all)
R1(config)# do show clock
R1(config)# clock timezone PST -8
R1(config)# do clock set HH:MM:SS DAY MONTH YEAR
R1(config)# clock summer-time PDT recurring 
R1(config)# end
R1# show clock

NTP client Configuration (NTP uses only UTC timezon - you must config appropr TimeZ on each devic)
R1(config)# ntp server 192.168.1.1
R1(config)# ntp server 216.239.35.1 prefer -> to make the router prefer 1 over other add prefer 
R1(config)# do show ntp associations 
R1(config)# do show ntp status 
R1(config)# do show clock detail
R1(config)# do show calendar
R1(config)# clock timezone PST -8
R1(config)# ntp update-calendar
R1(config)# do show clock detail
R1(config)# do show calendar
R1(config)# int l0 ---> coinfig loopback
R1(config-if)# ip add 10.1.1.1 255.255.255.255
R1(config-if)#exit
R1(config)#ntp source loopback0
NEXt lets go to R2 - we want it to use R1 L0 as its ntp server
R2(config)# ntp server 10.1.1.1 255.255.255.255
R2(config)# do sh ntp associations
R2(config)# do sh ntp status - tunaview stratum number
NEXt lets go to R3 - we want it to use R1 L0 as its ntp server
R2(config)# ntp server 10.1.1.1 255.255.255.255

NTP server config - make cisco dev a server when there is no server to sync to (different&new from above) - hii ni fast compared to above
R1(config)# ntp master -> the default stratum No of ntp master command is 8
R1(config)# do sh ntp associations
next we go to R2 and make it use R1 as ntp server
R2(config)# ntp server 11.2.12.1 255.255.255.255 -> the ip is given in the topology
R2(config)# do sh ntp associations
next we go to R3 and make it use R1 as ntp server
R3(config)# ntp server 11.2.12.1 255.255.255.255
R3(config)# do sh ntp associations


NTP SYMMETRIC ACTIVE MODE (ie btwn R2 and R3) - configed R2 as R3 peer and vice versa
R2(config)# ntp peer 10.0.23.2 (this is ip add is for R3 directly connected int)
R2(config)# do show ntp associations
netx R3
R3(config)# ntp peer 10.0.23.2 (this is ip add is for R2 directly connected int)
R3(config)# do show ntp associations

NTP AUTHENTICATION(optional. It allows NTP Clients to sync to intended servers)
ntp authenticate
ntp authentication-key 1 md5 <put key/password here ie mohamed>
ntp trused-key 1



