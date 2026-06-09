# Nmap Host Discovery

## Introduction

Nmap is a powerful open-source network scanner used for:

- Host discovery
- Port scanning
- Service enumeration
- Vulnerability assessment

---

## TASK 5 — Host Discovery

### Basic Host Discovery

```bash
nmap -sn <host_ip>
```

### ARP Scan

```bash
nmap -PR -sn TARGETS
```

Useful during internal network enumeration.

---

## TASK 6 — ICMP Discovery

### ICMP Echo Request

```bash
nmap -PE -sn TARGET
```

### ICMP Timestamp Request

```bash
nmap -PP -sn TARGET
```

### ICMP Address Mask Request

```bash
nmap -PM -sn TARGET
```

---

## TASK 7 — TCP SYN Ping

```bash
nmap -PS -sn TARGET
```

Open ports reply with SYN/ACK.

---

## TCP ACK Ping

```bash
sudo nmap -PA -sn TARGET
```

Server replies with RST.

---

## UDP Ping

```bash
sudo nmap -PU -sn TARGET
```

Closed UDP ports respond with ICMP Destination Unreachable.

---

## Masscan

```bash
masscan 10.200.6.0/24 -p443
masscan 10.200.6.0/24 -p80,443
masscan 10.200.6.0/24 -p22-25
```
