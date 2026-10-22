# CCNA Revision Notes: Syslog

## 1. Purpose of Syslog

- Industry-standard protocol for logging device events.
- Used for monitoring, troubleshooting, and incident analysis.
- Logs events such as interface status changes, routing neighbor changes, and system restarts.
- Logs can be displayed in the CLI, stored in RAM, or sent to an external Syslog server.

## 2. Syslog message format

Know how to identify these fields:

- Sequence number: Order of log messages (optional).
- Timestamp: When the event occurred (recommended).
- Facility: Process that generated the message (e.g., LINK, OSPF, SYS).
- Severity: How serious the event is (0–7).
- Mnemonic: Short code describing the event.
- Description: Details of what happened.

## 3. Severity levels — memorize!

| Level | Keyword               | Meaning                          |
| ----- | --------------------- | -------------------------------- |
| 0     | Emergency             | System unusable                  |
| 1     | Alert                 | Immediate action required        |
| 2     | Critical              | Critical conditions              |
| 3     | Error                 | Error conditions                 |
| 4     | Warning               | Warning conditions               |
| 5     | Notification / Notice | Normal but significant condition |
| 6     | Informational         | Informational messages           |
| 7     | Debugging             | Debug messages                   |

Remember: 0 = most severe; 7 = least severe.

Mnemonic: Every Awesome Cisco Engineer Will Need Ice cream Daily.

## 4. Logging locations and UDP port

- Console: Messages displayed through the console; all levels by default.
- VTY (SSH/Telnet): Logging disabled by default; use `terminal monitor` in the session to display messages.
- Buffer: Stores logs in RAM; view using `show logging`.
- External Syslog server: Centralized logging; UDP port 514.

## 5. Key configuration commands

```
logging console 6
logging monitor informational
logging buffered 4096 6
logging host 192.168.1.10
logging trap debugging
terminal monitor
```

- A configured severity threshold includes that level and all more severe levels (lower numbers).
- `logging synchronous` prevents log messages from disrupting command-line typing.
- `service timestamps log datetime` adds date/time to log messages.
- `service timestamps log uptime` shows time since device startup.
- `service sequence-numbers` enables sequence numbers.

## 6. Syslog vs. SNMP

- Syslog: Sends event logs from devices to a logging destination.
- SNMP: Retrieves device information using Get and can modify supported variables using Set.