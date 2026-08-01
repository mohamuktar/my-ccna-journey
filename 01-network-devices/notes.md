# CCNA Day 1 Notes - Introduction to Networking & Network Devices


# 1. What is a Network?

## Definition

A **computer network** is: A digital communication network that allows nodes to share resources.

A network allows connected devices to:- Communicate & Exchange info & Share resources

---

# 2. Network Nodes

A **node** is any device connected to a network.

Examples of network nodes:

- Router
- Switch
- Firewall
- Server
- Client
- Printer
- Smartphone
- Computer

Clients and servers are also called:

- End hosts
- Endpoints

---

# 3. Building a Simple Network

Two computers connected together create a network.


---

# 4. Client

## Definition

A **client** is:- A device that accesses a service made available by a server.

A client usually requests information or resources.

Examples:

- Laptop browsing a website
- Phone watching YouTube
- Computer downloading a file


---

# 5. Server

## Definition

A **server** is:- A device that provides functions or services for clients.


---

# Client vs Server

| Client | Server |
|---|---|
| Requests services | Provides services |
| Receives data | Sends data |
| Usually user device | Usually provides resources |
| Example: Web browser | Example: Web server |

---

# Important Concept

A device can be both a client and a server depending on what it is doing.

Example:

## Laptop as Client

Watching YouTube:

```
Laptop → YouTube Server
```

The laptop requests data.

---

## Laptop as Server

Sharing a file:

```
Other PC → Laptop
```

The laptop provides the file.

---

# 6. Switches

## Purpose

A **switch** connects multiple devices inside the same **Local Area Network (LAN).**

Example:

```
        PC1
         |
PC2 --- Switch --- Printer
         |
       Server
```

---

# Switch Characteristics

Switches:

- Have many network ports/interfaces
- Connect end hosts
- Forward traffic inside a LAN
- Are used in homes, offices, and enterprises

Common Cisco switches:

- Catalyst 9200
- Catalyst 3650

---

# Switches Do NOT:

❌ Connect different LANs together  
❌ Send traffic across the Internet  

A router is needed for communication between networks.

---

# 7. Local Area Network (LAN)

## Definition

A **LAN (Local Area Network)** is a network covering a small area.

Examples:

- Home network
- Office network
- School network

Devices inside the same LAN can communicate directly.

---

# 8. Routers

## Purpose

A **router** connects different networks together.


---

# Router Characteristics

Routers:

- Connect multiple networks
- Forward traffic between LANs
- Provide Internet connectivity
- Usually have fewer interfaces than switches

---

# Switch vs Router

| Switch | Router |
|---|---|
| Connects devices in the same LAN | Connects different networks |
| Many ports | Fewer ports |
| Local communication | Network-to-network communication |
| Works inside LAN | Works between LANs |

---

# 9. Firewalls

## Purpose

A firewall:- Controls and filters network traffic based on configured security rules.

A firewall decides:

- Which traffic is allowed
- Which traffic is blocked

---

# Types of Firewalls

## 1. Network Firewall

A hardware device that protects an entire network.

Used in enterprise environments.

---

## 2. Host-Based Firewall

Software installed on individual devices.

Examples:

- Windows Firewall
- macOS Firewall

Protects a single computer.

---

# Next-Generation Firewall (NGFW)

A next-generation firewall provides:

- Traditional firewall filtering
- Advanced traffic inspection
- Intrusion Prevention System (IPS)
- Additional security features

Examples:

- Cisco Firepower
- Modern Cisco ASA

---





# Final Memory Cheat Sheet

```
CLIENT
↓
Requests services


SERVER
↓
Provides services


SWITCH
↓
Connects devices inside a LAN


ROUTER
↓
Connects different networks


FIREWALL
↓
Filters and protects network traffic
```


---


# Day 1 Key Takeaways

- A network is a group of connected nodes that share resources.
- Clients request services.
- Servers provide services.
- Switches connect devices inside the same LAN.
- Routers connect different networks.
- Firewalls protect networks by controlling traffic.
- The role of a device depends on what it is doing.