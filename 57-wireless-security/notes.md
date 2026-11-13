# CCNA Wireless Security – Revision Notes

## 1. Wireless security fundamentals

Wireless security is especially important because any device within range can receive wireless traffic.

Three main concepts:

| Concept        | Meaning                                                  |
| -------------- | -------------------------------------------------------- |
| Authentication | Verifies the identity of a user or device                |
| Encryption     | Scrambles data so only authorized recipients can read it |
| Integrity      | Helps detect whether messages were modified in transit   |

- Ideally, clients authenticate the AP too, preventing connections to malicious APs.
- Each wireless client uses a unique encryption/decryption key.
- A group key is shared among clients so they can decrypt traffic sent by the AP to all clients.

### Message Integrity Check (MIC)

1. Sender calculates a MIC and attaches it to the message.
2. Sender encrypts the message and MIC.
3. Receiver decrypts the message and independently calculates its MIC.
4. Receiver compares both MICs.
5. If they differ, the message is discarded.

MIC helps detect tampering.

## 2. Wireless authentication methods

### Open authentication

- No credentials required; AP accepts authentication requests.
- Not secure by itself.
- Can be combined with another authentication method, such as a public Wi-Fi login page.

### WEP – Wired Equivalent Privacy

- Original 802.11 security option.

- Uses RC4 for encryption.

- Shared-key protocol.

- Key lengths: 40 or 104 bits, combined with a 24-bit initialization vector (IV).

- Can provide encryption only or both encryption and authentication.

- WEP authentication uses a challenge phrase encrypted with the shared key.

- Insecure – do not use



### EAP – Extensible Authentication Protocol

- An authentication framework, not one specific authentication protocol.
- Provides a framework for different methods, including LEAP, EAP-FAST, PEAP and EAP-TLS.
- Integrated with 802.1X.

## 3. 802.1X authentication architecture

802.1X provides port-based network access control, restricting access until a client authenticates.

Know these three entities:

| Entity                | Function                                     | Wireless example |
| --------------------- | -------------------------------------------- | ---------------- |
| Supplicant            | Device requesting network access             | Wireless client  |
| Authenticator         | Device controlling network access            | AP or WLC        |
| Authentication server | Checks credentials and permits/denies access | RADIUS server    |

Authentication sequence:

1. Client uses open authentication to associate with the AP.
2. Client still does not have full network access.
3. Only traffic required for EAP authentication is allowed.
4. WLC acts as the middleman in split-MAC architecture.
5. Authentication server checks credentials and grants or denies access.

## 4. EAP authentication methods

| Method   | Key feature                                   | Important detail                               |
| -------- | --------------------------------------------- | ---------------------------------------------- |
| LEAP     | Username/password + mutual challenge-response | Uses dynamic WEP keys; vulnerable              |
| EAP-FAST | PAC (Protected Access Credential)             | PAC establishes a secure TLS tunnel            |
| PEAP     | Server digital certificate                    | Client authenticates inside the TLS tunnel     |
| EAP-TLS  | Certificates on server and client             | Mutual certificate authentication; most secure |

Important differences:

- LEAP: Developed by Cisco; considered vulnerable.
- EAP-FAST: Uses a PAC to establish the TLS tunnel.
- PEAP: Server has a certificate; client authenticates inside the tunnel (e.g. MS-CHAP).
- EAP-TLS: Both client and authentication server need certificates.



Exam focus

&#x20;EAP-TLS is the most secure, but PEAP can be easier to implement because client certificates are not required.



## 5. Encryption and integrity methods

| Protocol | Used in         | Encryption       | Integrity |
| -------- | --------------- | ---------------- | --------- |
| WEP      | Original 802.11 | RC4              | —         |
| TKIP     | WPA             | Based on WEP     | MIC       |
| CCMP     | WPA2            | AES Counter Mode | CBC-MAC   |
| GCMP     | WPA3            | AES Counter Mode | GMAC      |

### TKIP – Temporal Key Integrity Protocol

- Temporary improvement over WEP.
- Used in WPA.
- Adds security features to WEP:
  - MIC for message integrity.
  - Key mixing creates a unique WEP key for each frame.
  - IV increased from 24 to 48 bits.
  - Timestamp and sequence number help prevent replay attacks.
- Not necessary to memorize every feature; know its purpose and WPA association.

### CCMP – Counter/CBC-MAC Protocol

- Used in WPA2.
- More secure than TKIP.
- Uses AES Counter Mode for encryption.
- Uses CBC-MAC for message integrity.
- Requires compatible hardware.

### GCMP – Galois Counter Mode Protocol

- Used in WPA3.
- More secure and efficient than CCMP.
- Uses AES Counter Mode for encryption.
- Uses GMAC for message integrity.
- Supports higher data throughput than CCMP.


## 6. WPA, WPA2 and WPA3

The Wi-Fi Alliance developed these certifications to standardize wireless security protocols.

All three support two authentication modes:

| Mode       | Authentication                               |
| ---------- | -------------------------------------------- |
| Personal   | Pre-shared key (PSK)                         |
| Enterprise | 802.1X with an authentication server and EAP |

Personal mode:

- Common in home and small-office networks.
- User enters a Wi-Fi password (PSK).
- PSK is not sent over the air.
- A four-way handshake uses the PSK to generate encryption keys.

Enterprise mode:

- Uses 802.1X and an authentication server.
- Supports EAP methods such as PEAP and EAP-TLS.

### WPA comparison

| Certification | Encryption / Integrity | Authentication    |
| ------------- | ---------------------- | ----------------- |
| WPA           | TKIP                   | PSK or 802.1X/EAP |
| WPA2          | CCMP                   | PSK or 802.1X/EAP |
| WPA3          | GCMP                   | PSK or 802.1X/EAP |

### Additional WPA3 security features

- PMF (Protected Management Frames): Protects 802.11 management frames from eavesdropping and forging. Optional in WPA2, mandatory in WPA3.
- SAE (Simultaneous Authentication of Equals): Protects the four-way handshake in personal mode.
- Forward secrecy: Protects previously transmitted data against later decryption if wireless frames were captured.

## 7. CCNA exam facts to remember

Memorize these distinctions

| Question                                                          | Answer                                           |
| ----------------------------------------------------------------- | ------------------------------------------------ |
| What does GMAC provide?                                           | MIC                                              |
| Three 802.1X entities?                                            | Supplicant, authenticator, authentication server |
| Which method is most secure?                                      | GCMP                                             |
| Which EAP method requires certificates on both client and server? | EAP-TLS                                          |
| Which WPA3 feature protects the personal-mode handshake?          | SAE                                              |
| Which protocol uses RC4?                                          | WEP                                              |
| Which protocol uses TKIP?                                         | WPA                                              |
| Which protocol uses CCMP?                                         | WPA2                                             |
| Which protocol uses GCMP?                                         | WPA3                                             |
| Which EAP method uses a PAC?                                      | EAP-FAST                                         |
| Which EAP method uses a server certificate?                       | PEAP                                             |

