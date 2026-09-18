# TCP & UDP — Day 30

## 🔥 CCNA Exam Focus

**Exam Topic 1.5:** Compare **TCP vs UDP**.

Both are **Layer 4 (Transport Layer)** protocols.

---

## 1. Layer 4 Basics

Layer 4 provides **end-to-end data transfer** between hosts.

Main functions:

* **Port numbers** → Layer 4 addressing
* **Reliable data transfer** → TCP
* **Error recovery** → TCP
* **Data sequencing** → TCP
* **Flow control** → TCP
* UDP does **not** provide these TCP services.

### Port numbers

Port numbers:

* Identify the **Application Layer protocol**
* Allow **session multiplexing** → a host can maintain multiple communication sessions simultaneously.

Example:

```text
PC1 → Server
Source Port:      50000
Destination Port: 80
```

Port `80` identifies **HTTP**.

The server replies:

```text
Source Port:      80
Destination Port: 50000
```

---

## 2. Port Number Ranges

| Range         | Name                | Purpose                 |
| ------------- | ------------------- | ----------------------- |
| `0–1023`      | Well-known          | Major protocols         |
| `1024–49151`  | Registered          | Registered applications |
| `49152–65535` | Ephemeral / Dynamic | Temporary source ports  |

🔥 **Remember:** Client devices commonly use **ephemeral ports** as random source ports.

---

# 3. TCP

**TCP = Transmission Control Protocol**

TCP is:

* **Layer 4**
* **Connection-oriented**
* **Reliable**
* Provides **sequencing**
* Provides **error recovery/retransmission**
* Provides **flow control**
* Uses acknowledgments
* Has more overhead than UDP

### TCP services

| Feature             | TCP |
| ------------------- | --- |
| Connection-oriented | ✅   |
| Reliable delivery   | ✅   |
| Acknowledgments     | ✅   |
| Retransmission      | ✅   |
| Sequencing          | ✅   |
| Flow control        | ✅   |
| Larger header       | ✅   |

---

# 4. TCP Header — CCNA Must Know

You **do not need to memorize the entire header**.

Know these:

* **Source Port**
* **Destination Port**
* **Sequence Number**
* **Acknowledgment Number**
* **Flags**

  * `SYN`
  * `ACK`
  * `FIN`
* **Window Size** → flow control

Both port fields are **16 bits**:

```text
2^16 = 65,536 possible port numbers
```

---

# 5. TCP 3-Way Handshake

Used to **establish a TCP connection**.

🔥 Memorize:

```text
SYN → SYN-ACK → ACK
```

### Flow

```text
PC1                    Server
 |                       |
 |------ SYN ----------->|
 |                       |
 |<---- SYN-ACK ---------|
 |                       |
 |------ ACK ----------->|
 |                       |
 |   Connection ready    |
```

**SYN** = Synchronize
**ACK** = Acknowledgment

After the handshake, actual data transfer begins.

---

# 6. TCP 4-Way Termination

Used to terminate a TCP connection.

🔥 Memorize:

```text
FIN → ACK → FIN → ACK
```

```text
PC1                    Server
 |                       |
 |-------- FIN --------->|
 |<------- ACK ----------|
 |<------- FIN ----------|
 |-------- ACK --------->|
```

**FIN** = Finish/terminate connection.

---

# 7. TCP Sequencing & Acknowledgments

TCP uses:

* **Sequence numbers**
* **Acknowledgment numbers**

This allows TCP to:

1. Track data
2. Detect missing segments
3. Put segments back into the correct order
4. Retransmit unacknowledged data

### Forward acknowledgment

The ACK indicates the **next sequence number expected**.

Example:

```text
PC1 → Server
SEQ 10

Server → PC1
ACK 11
```

`ACK 11` means:

> "I received up through 10; I expect 11 next."

🔥 **Remember:** ACK = **next expected sequence number**.

---

## TCP Retransmission

If a segment isn't acknowledged:

```text
Send segment
     ↓
No ACK
     ↓
Retransmit
```

This provides **reliable communication**.

---

# 8. TCP Flow Control

TCP uses the **Window Size** field and a **sliding window**.

Purpose:

> Prevent the sender from sending data faster than the receiver can handle.

Conceptually:

```text
Send multiple segments
        ↓
Receive ACK
        ↓
Adjust window size
```

🔥 **Window Size → Flow Control**

---

# 9. UDP

**UDP = User Datagram Protocol**

UDP is much simpler than TCP.

UDP is:

* **Layer 4**
* **Connectionless**
* **Best-effort**
* No acknowledgments
* No retransmissions
* No sequencing
* No flow control
* Smaller header
* Less overhead

### UDP features

| Feature             | UDP |
| ------------------- | --- |
| Connection-oriented | ❌   |
| Reliable delivery   | ❌   |
| Acknowledgments     | ❌   |
| Retransmission      | ❌   |
| Sequencing          | ❌   |
| Flow control        | ❌   |
| Smaller header      | ✅   |

🔥 **UDP = Send it and move on.**

---

# 10. UDP Header

Only **4 fields**:

1. Source Port
2. Destination Port
3. Length
4. Checksum

Compared with TCP, UDP has a much smaller header.

---

# 11. TCP vs UDP

🔥 **Most important section for Day 30**

| TCP                           | UDP                             |
| ----------------------------- | ------------------------------- |
| Connection-oriented           | Connectionless                  |
| Reliable                      | Best-effort                     |
| ACKs                          | No ACKs                         |
| Retransmission                | No retransmission               |
| Sequencing                    | No sequencing                   |
| Flow control                  | No flow control                 |
| Larger header                 | Smaller header                  |
| More overhead                 | Less overhead                   |
| Generally more delay/overhead | Generally faster/lower overhead |

### When to use TCP?

When **reliable delivery** is important.

Examples:

* File downloads
* Web applications using TCP
* SSH
* Email protocols listed in this lesson

You don't want missing/corrupted pieces of an important file.

### When to use UDP?

When **low overhead and delay sensitivity** are important.

Examples:

* Voice
* Real-time video
* VoIP
* Video conferencing

A brief loss of audio/video may be preferable to waiting for retransmission.

---

# 12. Important Exception

Some applications can provide reliability **at the application layer** even when using UDP.

Example:

**TFTP → UDP 69**

Also:

**DNS → usually UDP, sometimes TCP**

🔥 Don't memorize "UDP = never reliable."
Instead:

> **UDP itself does not provide reliability.**

---

# 13. Important Port Numbers 🔥

### TCP

| Protocol |     Port | Transport |
| -------- | -------: | --------- |
| FTP      | `20, 21` | TCP       |
| SSH      |     `22` | TCP       |
| Telnet   |     `23` | TCP       |
| SMTP     |     `25` | TCP       |
| HTTP     |     `80` | TCP       |
| POP3     |    `110` | TCP       |
| HTTPS    |    `443` | TCP       |

### UDP

| Protocol |       Port | Transport |
| -------- | ---------: | --------- |
| DHCP     |   `67, 68` | UDP       |
| TFTP     |       `69` | UDP       |
| SNMP     | `161, 162` | UDP       |
| Syslog   |      `514` | UDP       |

### Both TCP & UDP

| Protocol | Transport     |
| -------- | ------------- |
| **DNS**  | **UDP + TCP** |

🔥 **DNS usually uses UDP, but can use TCP.**

---

# 🧠 Day 30 Memorization

### TCP

```text
TCP
↓
Connection-oriented
↓
Reliable
↓
ACK
↓
Retransmission
↓
Sequencing
↓
Flow control
```

### UDP

```text
UDP
↓
Connectionless
↓
Best-effort
↓
No ACK
↓
No retransmission
↓
No sequencing
↓
No flow control
```

### Handshakes

```text
TCP Establish:
SYN → SYN-ACK → ACK

TCP Terminate:
FIN → ACK → FIN → ACK
```

### Key fields

```text
TCP:
Sequence + ACK + Window

UDP:
Source Port + Destination Port + Length + Checksum
```

### 🔥 Highest-priority port numbers

```text
FTP     20/21
SSH     22
Telnet  23
SMTP    25
HTTP    80
POP3    110
HTTPS   443

DHCP    67/68
TFTP    69
SNMP    161/162
Syslog  514

DNS     UDP + TCP
```

**CCNA takeaway:** The biggest thing to know from Day 30 is the **TCP vs UDP comparison** and the **important port numbers**.
