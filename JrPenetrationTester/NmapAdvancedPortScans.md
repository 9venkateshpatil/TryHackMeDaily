# Nmap Advanced Port Scans

## TASK 2 — Null Scan

```bash
sudo nmap -sN TARGET
```

| Port State | Response |
|------------|----------|
| Open | No response |
| Closed | RST/ACK |

A lack of response means the port is either open or filtered.

---

## FIN Scan

```bash
sudo nmap -sF TARGET
```

| Port State | Response |
|------------|----------|
| Open | No response |
| Closed | RST |

---

## XMAS Scan

Sets the FIN, PSH and URG flags simultaneously.

```bash
sudo nmap -sX TARGET
```

---

## TCP ACK Scan

```bash
sudo nmap -sA TARGET
```

Used mainly for firewall analysis. Most targets respond with RST.

---

## Window Scan

```bash
sudo nmap -sW TARGET
```

Similar to ACK scans but analyses the TCP Window field.

---

## Custom Scans

```bash
nmap --scanflags RSTSYNFIN TARGET
```

General syntax:

```bash
nmap --scanflags CUSTOM_FLAGS TARGET
```
