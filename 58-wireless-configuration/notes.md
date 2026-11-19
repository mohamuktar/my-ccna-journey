# CCNA Wireless LAN Configuration – Revision Notes

Main focus: Configure WLANs using a WLC GUI, including WPA2-PSK authentication.

## 1. Network topology and VLAN design

The lab uses:

- 1 switch (SW1)
- 1 wireless LAN controller (WLC)
- 2 lightweight APs
- Wireless clients

PoE (Power over Ethernet): Supplies both power and data over one Ethernet cable.

LAG (Link Aggregation Group):

- Another term for EtherChannel.
- WLCs support static LAG only.
- No PAgP or LACP.
- Switch configuration uses `channel-group 1 mode on`.
- Provides additional throughput and redundancy.

### VLAN design

| VLAN | Name       | Subnet         | Purpose                       |
| ---- | ---------- | -------------- | ----------------------------- |
| 10   | Management | 192.168.1.0/24 | WLC, AP and switch management |
| 100  | Internal   | 10.0.0.0/24    | Internal WLAN                 |
| 200  | Guest      | 10.1.0.0/24    | Guest WLAN                    |

- VLAN 10 is not advertised as a wireless network.
- VLAN 100 maps to SSID `Internal`.
- VLAN 200 maps to SSID `Guest`.
- Switch SVIs use `.1` as the default gateway.
- WLC interfaces use `.100`.
- APs receive management IP addresses through DHCP.

### Split-MAC traffic flow

- AP switch ports = access ports.
- WLC switch connection = trunk port carrying required VLANs.
- APs form CAPWAP tunnels to the WLC.
- Wireless client traffic travels through the CAPWAP tunnel to the WLC.
- WLC forwards traffic into the VLAN mapped to the WLAN.

Example: An Internal client communicating with a Guest client sends traffic through the WLC to SW1. SW1 routes it into VLAN 200, and the WLC delivers it to the Guest client.

## 2. Switch configuration concepts

| Configuration                    | Purpose                                         |
| -------------------------------- | ----------------------------------------------- |
| Create VLANs 10, 100, 200        | Separate management, internal and guest traffic |
| AP ports as access VLAN 10       | Connect APs to management network               |
| WLC ports as static EtherChannel | Form LAG                                        |
| Port-channel as trunk            | Carry VLANs 10, 100, 200                        |
| SVI for each VLAN                | Default gateway                                 |
| DHCP pools                       | Assign IP addresses                             |
| `ntp master`                     | Make SW1 an NTP server                          |

### DHCP Option 43

Important exam fact

- DHCP Option 43 can tell lightweight APs the IP address of their WLC.
- Example: `option 43 ip 192.168.1.100`
- APs use this information to discover and contact the WLC.
- Not always necessary: APs and WLC on the same subnet can discover each other using local broadcast CAPWAP discovery messages.

## 3. Initial WLC setup

The initial setup is performed through the console CLI configuration wizard.

Important settings:

- System name, username and password.
- Enable LAG if required.
- Configure management interface IP, subnet mask, default gateway, VLAN ID and DHCP server.
- Configure SSID.
- Select country code.
- Enable wireless network standards.
- Configure Auto-RF.
- Configure NTP.
- Save settings and allow the WLC to restart.

Auto-RF: Allows the WLC to automatically select wireless channels and transmit power.

Country/regulatory domain: The AP's regulatory domain must match the WLC's configured country domain, or the AP might fail to join.

### Accessing the WLC GUI

1. Connect a PC to the wired management VLAN.
2. Open a web browser.
3. Enter the WLC management IP address.
4. Access the GUI through HTTP or HTTPS.
5. Log in using the configured credentials.

The GUI cannot be accessed through the console port.

## 4. WLC physical ports

Remember: Port = physical; interface = logical.

| Physical port            | Purpose                                 |
| ------------------------ | --------------------------------------- |
| Service port             | Out-of-band management                  |
| Distribution system port | Regular wireless client data traffic    |
| Console port             | CLI access                              |
| Redundancy port          | Connects WLCs for high availability     |
| USB port                 | File transfer, such as software updates |

Important details:

- Service port connects to a switch access port because it supports only one VLAN.
- Distribution system ports normally connect to switch trunk ports.
- Multiple distribution system ports can form a LAG.
- Redundancy port connects two WLCs into an HA pair.

High availability (HA): One WLC is active; the other is standby and can take over if the active controller fails.

## 5. WLC logical interfaces

| Interface             | Purpose                                                          |
| --------------------- | ---------------------------------------------------------------- |
| Management            | Device management, GUI, SSH, RADIUS, NTP, Syslog and CAPWAP      |
| Redundancy management | Management access to standby WLC                                 |
| Virtual               | Used for client communication, DHCP relay and web authentication |
| Service port          | Out-of-band management                                           |
| Dynamic               | Maps WLANs to VLANs                                              |



Exam focus

&#x20;Dynamic interfaces map WLANs to VLANs.



### Dynamic interface configuration

For each WLAN, create a dynamic interface and configure:

- Interface name.
- VLAN ID.
- IP address.
- Subnet mask.
- Default gateway.
- DHCP server address.

Example:

| Dynamic interface | VLAN |
| ----------------- | ---- |
| Internal          | 100  |
| Guest             | 200  |

## 6. WLAN configuration through the GUI

Navigate to the WLAN tab in the WLC GUI.

### Internal WLAN

1. Select the existing WLAN.
2. Check that the profile name and SSID are correct.
3. Enable the WLAN.
4. Map it to the Internal dynamic interface, not the management interface.
5. Open the Security tab.
6. Select WPA+WPA2.
7. Under Authentication Key Management, disable 802.1X and select PSK.
8. Choose ASCII or HEX.
9. Enter the pre-shared key.
10. Apply the configuration.

PSK requirements in the demonstrated WLC: ASCII password must be at least 8 characters.

### Guest WLAN

1. Create a new WLAN.
2. Specify profile name, SSID and unique WLAN ID.
3. Enable the WLAN.
4. Map it to the Guest dynamic interface.
5. Configure WPA2-PSK security.
6. Apply the configuration.

Remember: Profile name and SSID do not have to be identical.

### WPA2-PSK

- PSK = Pre-shared key.
- Also called WPA Personal mode.
- Common in home and small-office networks.
- No RADIUS server is required for PSK-only authentication.

## 7. Additional WLC settings

### Layer 3 security

| Option             | Function                                      |
| ------------------ | --------------------------------------------- |
| Web authentication | Requires username and password                |
| Web passthrough    | Displays a notice; user agrees to gain access |

Both can be used with open Layer 2 authentication for public Wi-Fi.

### QoS settings

Memorize all four

| QoS setting | Purpose                              |
| ----------- | ------------------------------------ |
| Platinum    | Voice                                |
| Gold        | Video                                |
| Silver      | Best effort (default)                |
| Bronze      | Background traffic (lowest priority) |

### Other settings

- Maximum number of clients: `0` means no maximum in the demonstrated WLC.
- FlexConnect can be enabled under advanced WLAN settings.
- WLC dashboard displays joined APs and connected clients.
- Client information includes IP address, associated AP and SSID.
- AP operational modes include Local, FlexConnect, Monitor and Rogue Detector.

## 8. WLC management and CPU ACLs

Management settings shown in the lab:

- SNMPv1 disabled.
- SNMPv2 and SNMPv3 enabled.
- Syslog disabled.
- HTTP and HTTPS enabled.
- Telnet disabled.
- SSH enabled.
- Management via wireless initially disabled.

CPU ACL:

- Controls which devices can access the WLC itself.
- Can restrict SSH, HTTP/HTTPS, Telnet and SNMP management traffic.
- Does not filter traffic passing through the WLC.
- Configure an ACL, add rules, then enable it under CPU Access Control Lists.

## 9. CCNA exam facts

| Question                                              | Answer                      |
| ----------------------------------------------------- | --------------------------- |
| Which WLC port forms an HA pair?                      | Redundancy port             |
| Which WLC interface maps WLAN to VLAN?                | Dynamic interface           |
| Which is a Layer 3 authentication method?             | Web authentication          |
| Which QoS setting is for video?                       | Gold                        |
| Which WLC port supports LAG for data traffic?         | Distribution system port    |
| Which DHCP option provides WLC discovery information? | Option 43                   |
| What LAG mode does a WLC use?                         | Static (`mode on`)          |
| Which switch port connects to a lightweight AP?       | Access port                 |
| Which switch port connects to a WLC?                  | Trunk port                  |
| Can you access WLC GUI through console?               | No                          |
| What does Auto-RF configure?                          | Channels and transmit power |

