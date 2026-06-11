# Nmap More

## TASK 2 — Service Version Detection

### Basic Scan

```bash
sudo nmap -sV TARGET
```

### Maximum Intensity

```bash
sudo nmap -sV --version-all TARGET
```

### Notes

- Version intensity ranges from 0 to 9.
- `--version-light` uses intensity 2.
- `--version-all` uses intensity 9.
- Using `-sV` forces a full TCP connection.
- A stealth SYN scan is not possible with version detection enabled.

---

## Operating System Detection

```bash
sudo nmap -O TARGET
```

Nmap can identify the target operating system based on its network responses.

---

## Traceroute

```bash
sudo nmap -sS --traceroute TARGET
```

Nmap can identify routers between you and the target. Unlike the traditional traceroute utility, Nmap starts with a high TTL and decreases it.
