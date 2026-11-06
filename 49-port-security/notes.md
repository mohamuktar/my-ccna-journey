# CCNA Port Security – Revision Notes

## 1. What is Port Security?

A Cisco switch security feature that controls:

- Which source MAC addresses are allowed on a switchport.
- How many MAC addresses are allowed on that port.

Configured per interface.

Purpose: Prevent unauthorized devices from accessing the network and limit MAC address flooding attacks.

## 2. Default Settings

When port security is enabled:

- Maximum allowed MAC addresses: 1
- First source MAC address is dynamically learned.
- Violation mode: Shutdown
- Secure MAC addresses do not age out by default.
- Unauthorized MAC address causes the port to enter err-disabled state.

Port security supports access and trunk ports, but the port must be statically configured—not dynamic auto or dynamic desirable.

## 3. Basic Configuration

```
interface g0/1
 switchport mode access
 switchport port-security
```

Set the maximum number of MAC addresses:

```
switchport port-security maximum 2
```

Manually configure an allowed MAC address:

```
switchport port-security mac-address aaaa.bbbb.cccc
```

Enable sticky MAC learning:

```
switchport port-security mac-address sticky
```

Sticky MAC addresses are dynamically learned and automatically added to the running configuration.

Important: Save the configuration to startup-config to retain sticky MAC addresses after a reboot.

## 4. Violation Modes

A violation occurs when an unauthorized source MAC address enters the port.

| Mode               | Action                                    | Syslog/SNMP | Violation counter  |
| ------------------ | ----------------------------------------- | ----------- | ------------------ |
| Shutdown (default) | Port enters err-disabled state            | Yes         | Increments         |
| Restrict           | Drops unauthorized traffic; port stays up | Yes         | Increments         |
| Protect            | Drops unauthorized traffic; port stays up | No          | Does not increment |

Remember:

- Shutdown = disables the port.
- Restrict = drops traffic and reports violations.
- Protect = silently drops traffic.

## 5. Recovering a Disabled Port

First, disconnect the unauthorized device.

Manual recovery:

```
interface g0/1
 shutdown
 no shutdown
```

Automatic recovery:

```
errdisable recovery cause psecure-violation
errdisable recovery interval 180
```

- Default recovery timer: 300 seconds.
- Recovery is disabled by default.
- The unauthorized device must be disconnected before recovery.

## 6. Secure MAC Address Aging

Secure MAC addresses are MAC addresses permitted by port security.

- Default aging time: 0 minutes (never age out).
- Aging applies to dynamically learned addresses by default.
- Static secure MAC addresses do not age out unless static aging is enabled.

Configure aging:

```
switchport port-security aging time 30
switchport port-security aging type inactivity
switchport port-security aging static
```

| Aging type | Meaning                                                      |
| ---------- | ------------------------------------------------------------ |
| Absolute   | Address expires after the timer, even if traffic continues.  |
| Inactivity | Timer resets whenever traffic arrives from that MAC address. |

## 7. Verification Commands

| Command                              | Purpose                                             |
| ------------------------------------ | --------------------------------------------------- |
| `show port-security`                 | Overview of port-security interfaces and violations |
| `show port-security interface g0/1`  | Detailed port-security status                       |
| `show interfaces status`             | Check for err-disabled status                       |
| `show errdisable recovery`           | View recovery settings and timers                   |
| `show mac address-table secure`      | Display secure MAC addresses                        |
| `show running-config interface g0/1` | Check interface configuration                       |

## CCNA Quick Review

- Port security controls allowed source MAC addresses and their maximum number.
- Default maximum: 1 MAC address.
- Default violation mode: Shutdown.
- Port must be statically configured as access or trunk.
- Sticky automatically learns MAC addresses and adds them to running-config.
- Save the configuration to preserve sticky addresses after reboot.
- Shutdown: err-disabled.
- Restrict: drops and reports.
- Protect: silently drops.
- Default aging time: 0 minutes.
- Default err-disable recovery: disabled; timer is 300 seconds.