# Protocol And Servers

## Protocols and Their Security

Protocols covered in this room:

- Telnet
- HTTP
- FTP
- SMTP
- POP3
- IMAP

These protocols were designed decades ago when security was not a primary concern. They transmit data, including credentials, in cleartext. Modern deployments usually use encrypted versions such as HTTPS, SFTP, FTPS, and SMTPS.

---

## TASK 1 — Common Attacks Against Protocols

Servers implementing these protocols are vulnerable to:

- Sniffing Attacks
- MITM Attacks
- Password Attacks
- Exploitation of Vulnerabilities

### Sniffing Attacks

Sniffing attacks are still effective against:

- Misconfigured services
- Internal networks
- Legacy systems
- IoT devices

### Useful tcpdump Commands

```bash
sudo tcpdump port 110 -A
sudo tcpdump host 10.20.30.148 -A
sudo tcpdump port 80 -A
sudo tcpdump port 21 -A
sudo tcpdump -w capture.pcap
tcpdump -r capture.pcap -A
```

---

## TASK 2 — SSL and TLS

- SSL is deprecated.
- TLS 1.2 remains widely used.
- TLS 1.3 is the modern standard.

### HTTPS Process

1. Establish TCP connection
2. Establish TLS connection
3. Send HTTP requests

---

## TASK 3 — HTTP

HTTP transfers web pages and web content.

### Common Web Servers

- Nginx
- Apache
- IIS

### Challenge

```bash
telnet <target_ip> 80
GET /flag.txt HTTP/1.1
host: telnet
```

---

## TASK 4 — FTP

FTP was designed for file transfers.

### Secure Alternatives

- SFTP
- FTPS
- SCP

### FTP Login

```bash
ftp <target_ip>
get flag.txt
```

Anonymous login may also be enabled.

---

## TASK 5 — Email Infrastructure

Components:

- MUA
- MSA
- MTA
- MDA

---

## TASK 6 — POP3

Ports:

- 110 → POP3
- 995 → POP3S

---

## TASK 7 — IMAP

Ports:

- 143 → IMAP
- 993 → IMAPS
