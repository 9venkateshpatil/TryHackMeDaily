# John The Ripper

## TASK 4

```bash
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
```

The above is the download link for a tool for identifying hashes.  
After downloading, just run:

```bash
python3 hash-id.py
```

Then paste the hash to identify its type.

```bash
john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt
```

This is the syntax for cracking a hash when the hash type is known, like MD5 in this case.

---

## TASK 5

NT hash is the hash format modern Windows operating system machines use to store user and service passwords. It is also commonly referred to as NTLM, which references the previous version of the Windows hashing format known as LM, thus NT/LM.

In Windows, SAM (Security Account Manager) is used to store user account information, including usernames and hashed passwords. You can acquire NT hash / NTLM hashes by dumping the SAM database on a Windows machine, using a tool like Mimikatz, or using the Active Directory database: `NTDS.dit`.

You may not have to crack the hash to continue privilege escalation, as you can often conduct a “pass the hash” attack instead. But sometimes, hash cracking is a viable option if there is a weak password policy.

Used for cracking the NTLM hash:

```bash
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt ntlm.txt
```

---

## TASK 6

The `/etc/shadow` file is where password hashes are stored on Linux machines. It also stores other information, such as the date of the last password change and password expiration information. It contains one entry per line for each user or user account on the system.

This file is usually only accessible by the root user, so you must have sufficient privileges to access the hashes. However, if you do, there is a chance that you will be able to crack some of the hashes.

### Unshadowing

John can be very particular about the formats it needs data in to work with it. For this reason, to crack `/etc/shadow` passwords, you must combine it with the `/etc/passwd` file for John to understand the data it is being given. To do this, we use a tool built into the John suite called `unshadow`.

The basic syntax is:

```bash
unshadow [path to passwd] [path to shadow]
```

Example:

```bash
unshadow local_passwd local_shadow > unshadowed.txt
```

### File 1 — local_passwd

Contains the `/etc/passwd` line for the root user:

```text
root:x:0:0::/root:/bin/bash
```

### File 2 — local_shadow

Contains the `/etc/shadow` line for the root user:

```text
root:$6$2nwjN454g.dv4HN/$m9Z/r2xVfweYVkrr.v5Ft8Ws3/YYksfNwq96UL1FX0OJjY1L6l.DS3KEVsZ9rOVLB/ldTeEL/OIhJZ
```

We can then feed the output from `unshadow`, in this example called `unshadowed.txt`, directly into John. We should not need to specify a mode here, as we have already made the input specifically for John.

In some cases, you will still need to specify the format, as we have done previously using:

```bash
--format=sha512crypt
john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt
```

### Practical

There were two files, `local_passwd` and `local_shadow`, containing lines for root users from both `/etc/passwd` and `/etc/shadow`.

So I unshadowed them and combined both using this command:

```bash
unshadow local_passwd local_shadow > lol.txt
```

---

## TASK 7

### Single Crack Mode

In this mode, John uses only the information provided in the username to try to work out possible passwords heuristically by slightly changing the letters and numbers contained within the username.

The best way to explain Single Crack Mode and word mangling is to go through an example.

Consider the username `Markus`.

Some possible passwords could be:

- Markus1, Markus2, Markus3
- MArkus, MARkus, MARKus
- Markus!, Markus$, Markus*

This technique is called word mangling. John is building its dictionary based on the information it has been fed and uses a set of rules called mangling rules, which define how it can mutate the word it started with to generate a wordlist based on relevant factors for the target you are trying to crack.

This exploits how weak passwords can be when based on the username or service being targeted.

To use single crack mode, we use roughly the same syntax as before. For example, if we wanted to crack the password of the user named `Mike`, using single mode, we would use:

```bash
john --single --format=[format] [path to file]
```

### A Note on File Formats in Single Crack Mode

If you are cracking hashes in single crack mode, you need to change the file format so John can understand the data and create a wordlist from it. You do this by prepending the hash with the username that the hash belongs to.

So, according to the above example, we would change the file `hashes.txt`:

From:

```text
1efee03cdcb96d90ad48ccc7b8666033
```

To:

```text
mike:1efee03cdcb96d90ad48ccc7b8666033
```

### Practical

We were given a `hash07.txt` file.  
Firstly, we added the username before the hash, separating them with a colon. I did it using `nano hash07.txt`.

I checked the hash type and it was MD5, so I cracked it using:

```bash
john --single --format=raw-md5 hash07.txt
```

---

## TASK 8

This task went over my head, so I am skipping it.  
Let’s come back to it when I actually face a situation where I have to make a custom rule for John the Ripper.

---

## TASK 9

We can use John to crack the password of password-protected Zip files. Again, we use a separate part of the John suite to convert the Zip file into a format that John will understand, and then we crack it using familiar syntax.

### Zip2John

As with the `unshadow` tool, we use `zip2john` to convert the Zip file into a hash format that John can understand and hopefully crack.

Basic usage:

```bash
zip2john [options] [zip file] > [output file]
```

- `[options]` lets you pass checksum options to `zip2john`
- `[zip file]` is the Zip file you want to extract the hash from
- `>` redirects the output to another file
- `[output file]` is the file that stores the output

Example:

```bash
zip2john zipfile.zip > zip_hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt
```

### Practical

First, we were given a zip file.  
We had to convert it into a format that John could understand, so we used:

```bash
zip2john secure.zip > lol.txt
```

Then, to crack the password of that zip, we used:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt lol.txt
```

We got the password of the zip along with the internal structure:

```text
secure.zip/zippy/flag.txt
```

Then I unzipped the file using:

```bash
unzip secure.zip
```

I entered the password and got the flag:

```text
THM{w3ll_d0n3_h4sh_r0y4l}
```

---

## TASK 10

```bash
rar2john [rar file] > [output file]
/opt/john/rar2john rarfile.rar > rar_hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
```

### Practical

We were again given a secure RAR file.  
We converted it using:

```bash
rar2john secure.rar > lol.txt
```

Then cracked it using:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt lol.txt
```

Then I extracted the file using:

```bash
unrar x secure.rar
```

I entered the password and got the flag:

```text
THM{r4r_4rch1ve5_th15_t1m3}
```

---

## TASK 11

Let’s explore one more use of John that comes up semi-frequently in CTF challenges: using John to crack the password of SSH private key `id_rsa` files.

Unless configured otherwise, SSH authentication uses a password. However, you can configure key-based authentication, which lets you use your private key, `id_rsa`, as an authentication key to log in to a remote machine over SSH. Often, the private key itself is also protected by a password. In that case, we can use John to crack it.

### ssh2john

As the name suggests, `ssh2john` converts the `id_rsa` private key into a hash format that John can work with.

If you do not have `ssh2john` installed, you can use:

```bash
python3 /opt/john/ssh2john.py
```

or on Kali:

```bash
python /usr/share/john/ssh2john.py
```

Basic syntax:

```bash
ssh2john [id_rsa private key file] > [output file]
```

### Cracking

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```

### Practical

We got an `id_rsa` file with the private key of a user in it.

We converted it into a John-understandable format using:

```bash
/opt/john/ssh2john.py id_rsa > lol.txt
```

Then cracked the private key (`id_rsa`) password.

The password was:

```text
mango
```
