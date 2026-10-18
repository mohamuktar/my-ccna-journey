# CCNA Notes: SNMP (Simple Network Management Protocol)

## 1. Purpose and components

- SNMP: Monitors and manages network devices.
- Managed device: Router, switch, or firewall being monitored.
- NMS (Network Management Station/System): Central system that manages devices.
- SNMP Manager: Software on the NMS that communicates with agents.
- SNMP Agent: Software running on managed devices.
- MIB (Management Information Base): Stores managed variables/information.
- OID (Object Identifier): Uniquely identifies a variable in the MIB, e.g., hostname or interface status.

## 2. Three main SNMP operations

- Notify: Device alerts NMS about events (e.g., interface down).
- Read: NMS requests device information (e.g., CPU usage).
- Write: NMS requests configuration changes.

## 3. SNMP versions

| Version | Key fact                                           |
| ------- | -------------------------------------------------- |
| SNMPv1  | Original version                                   |
| SNMPv2c | Supports GetBulk; community strings; no encryption |
| SNMPv3  | Preferred; supports authentication and encryption  |

Community strings: Act like passwords in SNMPv1/v2c, but are transmitted in plaintext.

## 4. SNMP message types

| Message    | Purpose                                           |
| ---------- | ------------------------------------------------- |
| `Get`      | Read variable values                              |
| `GetNext`  | Retrieve the next OID                             |
| `GetBulk`  | Retrieve large amounts of information efficiently |
| `Set`      | Change variable/configuration values              |
| `Trap`     | Notification without acknowledgment               |
| `Inform`   | Notification requiring acknowledgment             |
| `Response` | Reply to a request or Inform                      |

Remember: Trap = no acknowledgment; Inform = acknowledgment.

## 5. Important UDP ports

- UDP 161: SNMP agent receives requests.
- UDP 162: NMS receives notifications.

## 6. Community-string permissions

- RO (Read Only): Can read information; cannot change it using Set.
- RW (Read/Write): Can read and modify permitted variables.
- Avoid default community strings such as `public` and `private`.

## Exam priority

Focus on SNMP's purpose, manager vs. agent, MIB vs. OID, SNMPv1/v2c/v3, Trap vs. Inform, and UDP ports 161 and 162.

RFC numbers and detailed SNMP configuration are not the priority here. The key CCNA objective is understanding SNMP's function in network operations.