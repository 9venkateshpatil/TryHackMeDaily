# Nmap Basic Port Scans

## TASK 2 — Understanding Port States

- Open → Service is listening.
- Closed → Port reachable but no service.
- Filtered → Firewall blocks packets.
- Unfiltered → Port accessible but state unknown.
- Open|Filtered → Nmap cannot determine state.
- Closed|Filtered → Nmap cannot determine state.

---

## TASK 3 — TCP Flags

### URG
Urgent data.

### ACK
Acknowledgement flag.

### PSH
Push data to application.

### RST
Reset connection.

### SYN
Initiate TCP handshake.

### FIN
No more data to send.

---

## TASK 4 — TCP Connect Scan

```bash
nmap -sT TARGET
```

Performs the full TCP 3-way handshake.

```text
SYN → SYN/ACK → ACK
```

More likely to appear in logs.

---

## TASK 5 — TCP SYN Scan

```bash
sudo nmap -sS TARGET
```

Does not complete the full handshake.

```text
SYN → SYN/ACK → RST
```

Stealthier and faster.

---

## TASK 6 — UDP Scan

```bash
sudo nmap -sU TARGET
```

Closed UDP ports usually respond with:

```text
ICMP Type 3 Code 3
```

---

## TASK 7 — Fine Tuning

### Scan Specific Ports

```bash
-p22,80,443
```

### Scan Port Range

```bash
-p1-1023
```

### Scan All Ports

```bash
-p-
```

### Fast Scan

```bash
-F
```

### Timing Templates

| Level | Mode |
|---|---|
| -T0 | paranoid |
| -T1 | sneaky |
| -T2 | polite |
| -T3 | normal |
| -T4 | aggressive |
| -T5 | insane |
