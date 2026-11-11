# CCNA Security Fundamentals – Revision Notes

## 1. CIA Triad

The three foundations of information security:

| Principle       | Meaning                                                          |
| --------------- | ---------------------------------------------------------------- |
| Confidentiality | Only authorized users can access data.                           |
| Integrity       | Data must not be modified by unauthorized users.                 |
| Availability    | Systems and networks must remain accessible to authorized users. |
 
## 2. Key Security Concepts

- Vulnerability: A potential weakness in a system.
- Exploit: Something that takes advantage of a vulnerability.
- Threat: The potential for a vulnerability to be exploited.
- Mitigation: A technique used to protect against threats.

No system is 100% secure.
 
## 3. Common Attacks
 
| Attack                   | Description                                                               | Main impact                   |
| ------------------------ | ------------------------------------------------------------------------- | ----------------------------- |
| DoS                      | Overwhelms a system so legitimate users cannot access it.                 | Availability                  |
| DDoS                     | Multiple infected devices attack one target.                              | Availability                  |
| Spoofing                 | Uses a fake source IP or MAC address.                                     | Depends on attack             |
| Reflection               | Uses a third-party server to send traffic to a victim.                    | Availability                  |
| Amplification            | Small attacker request triggers a much larger response toward the victim. | Availability                  |
| Man-in-the-Middle (MitM) | Attacker intercepts or modifies communication between two devices.        | Confidentiality and integrity |
| Reconnaissance           | Gathers information about a target before an attack.                      | Information gathering         |

### Important attack examples

TCP SYN flood

1. Attacker sends many SYN messages.
2. Target responds with SYN-ACK.
3. Attacker never sends the final ACK.
4. Incomplete connections fill the TCP connection table.

DHCP exhaustion

- Attacker sends DHCP Discover messages using spoofed MAC addresses.
- DHCP pool becomes full.
- Legitimate devices cannot obtain IP addresses.

ARP spoofing / ARP poisoning

- Attacker sends a fake ARP reply.
- Victim's ARP table is updated with the attacker's MAC address.
- Traffic intended for the legitimate device goes through the attacker.

## 4. Malware

| Type         | Description                                                                               |
| ------------ | ----------------------------------------------------------------------------------------- |
| Virus        | Infects another program or file and spreads when it is shared or downloaded.              |
| Worm         | Standalone malware that can spread independently without user interaction.                |
| Trojan horse | Malicious software disguised as legitimate software; typically requires user interaction. |

## 5. Social Engineering

Social engineering manipulates people into revealing confidential information or performing actions for an attacker.

| Attack         | Description                                                              |
| -------------- | ------------------------------------------------------------------------ |
| Phishing       | Fraudulent emails designed to steal information.                         |
| Spear phishing | Targeted phishing against specific people or organizations.              |
| Whaling        | Phishing targeting high-profile individuals, such as company executives. |
| Vishing        | Voice/telephone phishing.                                                |
| Smishing       | SMS/text-message phishing.                                               |
| Watering hole  | Compromises a website frequently visited by the target.                  |
| Tailgating     | Follows an authorized person into a restricted area.                     |

## 6. Password Attacks

- Password guessing: Trying passwords based on likely information.
- Dictionary attack: Trying passwords from a list of common words and passwords.
- Brute-force attack: Trying every possible character combination.

The transcript recommends strong passwords with:

- At least 8 characters; preferably longer.
- Uppercase and lowercase letters.
- Numbers.
- Special characters.
- Regular changes.

## 7. Multi-Factor Authentication (MFA)

MFA requires more than one authentication factor.

| Factor             | Meaning                  | Example                           |
| ------------------ | ------------------------ | --------------------------------- |
| Something you know | Information you know     | Password or PIN                   |
| Something you have | Something you possess    | Phone authenticator or badge      |
| Something you are  | Biometric characteristic | Fingerprint, face, or retina scan |

Two-factor authentication (2FA) uses two different factors.

## 8. Digital Certificates

- Used to verify the identity of a certificate holder, commonly a website.
- CSR: Certificate Signing Request, submitted by the entity requesting a certificate.
- CA: Certificate Authority, which generates and signs the certificate.
- A valid certificate helps a browser verify that a website is legitimate.

## 9. AAA Framework

AAA controls and monitors users accessing a network or computer system.

| Component      | Meaning              | Purpose                  |
| -------------- | -------------------- | ------------------------ |
| Authentication | Who are you?         | Verifies identity.       |
| Authorization  | What can you access? | Determines permissions.  |
| Accounting     | What did you do?     | Records user activities. |

### AAA protocols

| Protocol | Type              | Ports                                            |
| -------- | ----------------- | ------------------------------------------------ |
| RADIUS   | Open standard     | UDP 1812 (authentication), UDP 1813 (accounting) |
| TACACS+  | Cisco proprietary | TCP 49                                           |

\- Cisco ISE (Identity Services Engine) is Cisco's AAA server.

## 10. Security Program Elements

- User awareness programs: Teach employees about security threats, such as phishing.
- User training programs: Formal training on security policies, password practices, and threats.
- Physical access control: Restricts access to equipment and secure areas, such as network closets and data centers.

## CCNA Quick Review

| Remember        | Key point                                       |
| --------------- | ----------------------------------------------- |
| CIA             | Confidentiality, Integrity, Availability        |
| Vulnerability   | Potential weakness                              |
| Exploit         | Takes advantage of a vulnerability              |
| Threat          | Potential for exploitation                      |
| Mitigation      | Protects against threats                        |
| SYN flood       | Incomplete TCP handshakes fill connection table |
| DHCP exhaustion | DHCP address pool becomes full                  |
| ARP poisoning   | Fake ARP reply redirects traffic                |
| Worm            | Spreads independently                           |
| Trojan          | Disguised as legitimate software                |
| MFA             | Uses multiple authentication factors            |
| AAA             | Authentication, Authorization, Accounting       |
| RADIUS          | UDP 1812 and 1813                               |
| TACACS+         | TCP 49        

