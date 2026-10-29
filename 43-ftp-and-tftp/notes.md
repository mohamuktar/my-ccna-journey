# CCNA Revision Notes: FTP & TFTP

## 1. Purpose

Both are client-server protocols used to transfer files over a network, including Cisco IOS images.

## 2. FTP vs TFTP

| Feature        | FTP                                                 | TFTP                           |
| -------------- | --------------------------------------------------- | ------------------------------ |
| Full name      | File Transfer Protocol                              | Trivial File Transfer Protocol |
| Transport      | TCP                                                 | UDP                            |
| Ports          | TCP 20 (data), 21 (control)                         | UDP 69 (initial request)       |
| Authentication | Username and password                               | None                           |
| Encryption     | None by default                                     | None                           |
| Features       | Transfer, list, navigate, create/delete directories | Basic file upload/download     |
| Complexity     | More complex                                        | Simpler                        |

- FTP: TCP provides reliable transport. Credentials and data are plaintext unless a secure alternative is used.
- TFTP: Lightweight, simple transfers; uses acknowledgments, timers, and retransmissions for reliability.
- FTPS: FTP secured using SSL/TLS.
- SFTP: A separate file-transfer protocol that uses SSH.

## 3. TFTP Reliability

- Uses lock-step communication: data message → acknowledgment (ACK) → next data message.
- If an expected message or ACK is missing, a timer triggers retransmission.
- TFTP does not use TCP, but implements its own transfer reliability.

## 4. FTP Connections

- Control connection — TCP 21: FTP commands and server replies.
- Data connection — TCP 20: Traditional active-mode FTP data transfer.
- Active mode: Server initiates the data connection.
- Passive mode: Client initiates the data connection; useful when the client is behind a firewall.

## 5. Cisco IOS File Systems

| Storage | Purpose                                     |
| ------- | ------------------------------------------- |
| Flash   | Usually stores the IOS image                |
| RAM     | Running processes and active data; volatile |
| NVRAM   | Stores startup configuration; non-volatile  |
| Network | Remote file systems such as FTP/TFTP        |

Commands:

```
show file systems
show flash
show version
```

## 6. Copy Files Using TFTP

```
copy tftp: flash:
```

Enter the TFTP server IP, source filename, and destination filename when prompted.

## 7. Copy Files Using FTP

Configure FTP credentials first:

```
ip ftp username USERNAME
ip ftp password PASSWORD
copy ftp: flash:
```

Enter the FTP server IP and filenames when prompted. The credentials must match an account configured on the FTP server.

## 8. Boot From a New IOS Image

After copying the image to flash:

```
show flash
configure terminal
boot system flash:IOS-FILENAME.bin
end
copy running-config startup-config
reload
```

Replace `IOS-FILENAME.bin` with the actual image filename and use the correct path for your device.

After rebooting, verify the active IOS with `show version`. Delete an old image only after confirming the new image boots successfully and the old image is no longer needed.

## CCNA Must-Remember

- FTP = TCP 20/21; TFTP = UDP 69.
- FTP supports authentication; TFTP does not.
- Neither FTP nor TFTP encrypts traffic by default.
- FTP has separate control and data connections.
- TFTP uses ACKs and retransmissions despite using UDP.
- `copy tftp: flash:` copies a file from TFTP to flash.
- `boot system` specifies which IOS image to boot.
- `copy running-config startup-config` saves configuration changes.