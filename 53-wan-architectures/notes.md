# CCNA Revision Notes — WAN Architectures & VPNs

Jeremy's IT Lab · WAN (1.2.d) and VPNs (5.5)

## 1. WAN Fundamentals

- WAN (Wide Area Network): Connects geographically distant LANs (different cities/countries).
- Typically connects enterprise offices, data centers, and other sites.
- The Internet is technically a WAN, but WAN usually refers to private enterprise connections.

## 2. Leased Lines

- Dedicated physical connection between two sites.
- Private, not shared with other customers.
- Traditionally uses serial connections with PPP or HDLC (not Ethernet).
- Modern WANs increasingly use Ethernet over fiber.

Hub-and-spoke topology:

- Hub = central site (e.g., data center).
- Spokes = branch offices.
- Advantage: Centralized traffic control through a firewall.

Disadvantages: Higher cost, longer installation time, and generally slower speeds than modern Ethernet connections.

Standards:

- North America: T1, T2, T3.
- Europe: E1, E2, E3.

## 3. MPLS (Multi-Protocol Label Switching)

- Allows enterprises to connect sites over a shared service provider network.
- Uses labels to separate customer traffic and forward packets.
- Forwards based on MPLS labels, not destination IP addresses.
- Called a Layer 2.5 protocol because its label sits between the Layer 2 and Layer 3 headers.

MPLS router types:

| Router | Meaning       |
| ------ | ------------- |
| CE     | Customer Edge |
| PE     | Provider Edge |
| P      | Provider Core |

- CE connects to PE.
- PE adds MPLS labels.
- P routers forward traffic using labels.
- CE routers do not need to run MPLS.

Two MPLS VPN types:

- Layer 3 MPLS VPN: CE and PE exchange routing information using OSPF or another routing protocol, or static routes.
- Layer 2 MPLS VPN: Provider network acts like a large switch. CE routers appear directly connected and can peer with each other.

## 4. Internet Connectivity

Common enterprise Internet connection methods:

- Fiber Ethernet
- DSL
- Cable Internet (CATV)
- Leased lines
- MPLS

DSL (Digital Subscriber Line)

- Provides Internet over existing telephone lines.
- Requires a modem (modulator-demodulator).

Cable Internet

- Provides Internet over existing CATV (cable television) lines.
- Requires a cable modem.

### Internet redundancy terminology

| Term            | Connections                     |
| --------------- | ------------------------------- |
| Single-homed    | 1 connection to 1 ISP           |
| Dual-homed      | 2 connections to 1 ISP          |
| Multihomed      | 1 connection to each of 2 ISPs  |
| Dual multihomed | 2 connections to each of 2 ISPs |

Most redundancy: Dual multihomed.

## 5. Internet VPNs

The Internet is public and shared, so VPNs provide secure communication over it.

Two important types:

- Site-to-site VPN → typically IPsec.
- Remote-access VPN → typically TLS.

### Site-to-Site VPN (IPsec)

- Connects two networks/sites over the Internet.
- VPN tunnel forms between two routers or firewalls.
- Uses encryption to protect the original packet.
- Adds VPN encapsulation and a new IP header.
- Receiving device decrypts the packet.

Important: One tunnel between two routers protects traffic for all hosts at those sites. Individual PCs do not need VPN clients.

IPsec limitations:

- Supports unicast, not broadcast or multicast.
- Routing protocols such as OSPF cannot operate directly over standard IPsec tunnels.

### GRE over IPsec

- GRE (Generic Routing Encapsulation): Supports multiple Layer 3 protocols, broadcast, and multicast.
- GRE alone does not encrypt traffic.
- GRE over IPsec combines GRE flexibility with IPsec encryption.

Encapsulation order:

1. Original IP packet.
2. GRE header and new IP header.
3. Encrypt the GRE packet.
4. Add IPsec header and new IP header.

### DMVPN (Dynamic Multipoint VPN)

- Cisco-developed solution.
- Dynamically creates a full mesh of IPsec tunnels.
- Avoids manually configuring every tunnel.

How it works:

1. Configure hub-and-spoke IPsec tunnels.
2. Hub provides information allowing spokes to establish direct tunnels with each other.

Benefit: Simple hub-and-spoke configuration with efficient direct spoke-to-spoke communication.

### Remote-Access VPN (TLS)

- Connects individual devices (PCs, laptops, phones) to a company's internal network.
- Typically uses TLS (Transport Layer Security).
- Uses VPN client software, such as Cisco AnyConnect.
- Client establishes a secure tunnel to a company router or firewall.

TLS was formerly known as SSL (Secure Sockets Layer).

## 6. Site-to-Site vs Remote-Access VPN

| Feature          | Site-to-Site                  | Remote-Access                      |
| ---------------- | ----------------------------- | ---------------------------------- |
| Typical protocol | IPsec                         | TLS                                |
| Connects         | Two sites                     | One device to a site               |
| Tunnel endpoints | Routers/firewalls             | End device and company VPN gateway |
| Software         | No individual client required | VPN client software                |
| Usage            | Usually permanent             | Usually on-demand                  |

## CCNA Must-Know Checklist

- WAN connects geographically distant LANs.
- Leased lines are dedicated physical connections.
- Hub-and-spoke: central hub and connected spokes.
- PPP and HDLC are traditional leased-line encapsulations.
- MPLS forwards using labels, not IP addresses.
- Know CE, PE, and P routers.
- L3 MPLS: CE-PE routing.
- L2 MPLS: provider network acts like a switch.
- Know single-homed, dual-homed, multihomed, dual multihomed.
- DSL uses telephone lines; cable uses CATV.
- Site-to-site VPN typically uses IPsec.
- Remote-access VPN typically uses TLS.
- GRE supports multicast/broadcast but does not encrypt.
- GRE over IPsec provides GRE flexibility and IPsec security.
- DMVPN dynamically establishes spoke-to-spoke tunnels.
