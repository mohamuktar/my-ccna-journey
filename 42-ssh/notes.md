# CCNA Revision Notes: SSH, Telnet & Device Access

## 1. Console Line Security

- By default, console access may not require a password.
- `line console 0` — configure console access.
- `password` + `login` — require a console password.
- `login local` — require a configured local username and password.
- `exec-timeout` — log out after inactivity.

```
line console 0
 password ccna
 login
 exec-timeout 3 30
```

## 2. Layer 2 Switch Management IP

Layer 2 switches do not route packets, but an SVI (Switch Virtual Interface) allows remote management through SSH, Telnet, and ping.

```
interface vlan 1
 ip address 192.168.1.2 255.255.255.0
 no shutdown
ip default-gateway 192.168.1.1
```

- `interface vlan 1` — configure the management SVI.
- `ip default-gateway` — allows management traffic to reach devices outside the local subnet.
- The management VLAN must be active and operational.

## 3. Telnet vs SSH

| Feature    | Telnet            | SSH                      |
| ---------- | ----------------- | ------------------------ |
| Purpose    | Remote CLI access | Secure remote CLI access |
| Encryption | None; plaintext   | Encrypted                |
| Transport  | TCP               | TCP                      |
| Port       | 23                | 22                       |
| Security   | Insecure          | Preferred                |

Remember: Use SSH instead of Telnet because Telnet exposes usernames, passwords, and session data in plaintext.

## 4. VTY Line Configuration

VTY (Virtual Teletype) lines control remote CLI access.

- `line vty 0 15` — configure lines 0–15, up to 16 simultaneous sessions if supported.
- `login local` — authenticate using local usernames.
- `transport input ssh` — allow SSH only.
- `transport input telnet` — allow Telnet only.
- `transport input telnet ssh` — allow both.
- `exec-timeout` — disconnect inactive sessions.
- `access-class` — apply an ACL to restrict remote VTY access.

```
username admin secret StrongPassword123
enable secret EnablePassword123

line vty 0 15
 login local
 transport input ssh
 exec-timeout 5 0
```

ACL command distinction:

- `access-list` / `ip access-list` — create an ACL.
- `access-class` — apply an ACL to VTY lines.
- `ip access-group` — apply an ACL to an interface.

## 5. SSH Configuration — Exam Priority

SSH requires a hostname, domain name, RSA keys, authentication, and VTY configuration.

```
hostname R1
ip domain name example.com
crypto key generate rsa modulus 2048
ip ssh version 2

username admin secret StrongPassword123
enable secret EnablePassword123

line vty 0 15
 login local
 transport input ssh
```

- Hostname + domain name: configure before generating RSA keys.
- RSA keys: enable SSH cryptographic functionality.
- 2048-bit modulus: a suitable modern key size for practice.
- `ip ssh version 2`: explicitly enable SSHv2.
- `login local`: use local username/password authentication.
- `transport input ssh`: restrict remote access to SSH.

The IOS image must support cryptographic features for SSH to work.

## 6. Verify SSH

| Command                        | Purpose                                |
| ------------------------------ | -------------------------------------- |
| `show ip ssh`                  | Check SSH support, version, and status |
| `show ip interface brief`      | Check interface IPs and status         |
| `show running-config`          | Inspect configuration                  |
| `show crypto key mypubkey rsa` | Inspect RSA public keys                |

From a client, connect using:

```
ssh -l admin 192.168.1.1
```

Or:

```
ssh admin@192.168.1.1
```

# CCNA SSH — Essential Configs Only

## 1. Console Security

`login` — Line → Password → Login

```
line console 0
 password ccna
 login
```

`login local` — Username → Line → Login local

```
username moha secret ccna
line console 0
 login local
```

## 2. Layer 2 Switch Management IP

```
interface vlan 1
 ip address 192.168.1.253 255.255.255.0
 no shutdown
exit
ip default-gateway 192.168.1.254
```

## 3. Telnet Configuration

```
enable secret ccna
username moha secret ccna
line vty 0 15
 login local
 transport input telnet
```

## 4. SSH Configuration

```
hostname SW1
ip domain name moha.com
crypto key generate rsa modulus 2048
ip ssh version 2

enable secret ccna
username moha secret ccna

line vty 0 15
 login local
 transport input ssh
```

## 5. Verification

```
show version
show ip ssh
show ip interface brief
show running-config
```

Remember for the CCNA:

- SSH uses TCP port 22; Telnet uses TCP port 23.
- `login local` requires a local username and password.
- Configure the hostname and domain name before generating RSA keys.
- `transport input ssh` allows SSH only.
- `ip default-gateway` lets a Layer 2 switch reach management devices outside its local subnet.