# ***OWASP Top 10 2025: Application Design Flaws***

**Room: OWASP Top 10 2025: Application Design Flaws**

---

## **TASK 2**

**AS02: Security Misconfigurations**

### **What It Is**
Security misconfigurations happen when systems, servers, or applications are deployed with unsafe defaults, incomplete settings, or exposed services. These are not code bugs but mistakes in how the environment, software, or network is set up. They create easy entry points for attackers.

### **Why It Matters**
Even small misconfigurations can expose sensitive data, enable privilege escalation, or give attackers a foothold into the system. Modern applications rely on complex stacks, cloud services, and third-party APIs. A single exposed admin panel, an open storage bucket, or misconfigured permissions can compromise the entire system.

### **Practical**
In the practical challenge, I had to access a target website which exposed very sensitive information about an API.

It literally showed the API for accessing using user ID:

<u><i>GET /api/user/&lt;user_id&gt;</i></u>

So I opened the terminal and executed:

```bash
curl http://&lt;target-ip&gt;/api/user/admin
```

And got the flag:

```text
THM{V3RB0S3_3RR0R_L34K}
```

---

## **TASK 3**

**AS03: Software Supply Chain Failures**

### **What It Is**
Software supply chain failures happen when applications rely on components, libraries, services, or models that are compromised, outdated, or improperly verified. These weaknesses are not inherent in your code, but rather in the software and tools you depend on. Attackers exploit these weak links to inject malicious code, bypass security, or steal sensitive data.

### **Why It Matters**
Modern applications are built from many third-party packages, APIs, and AI models. One compromised dependency can compromise your entire system, allowing attackers to gain access without ever touching your own code. Supply chain attacks can be automated and distributed, making them hard to detect and very damaging.

### **Practical**
I ran the following command:

```bash
curl -X POST -d '{"data":"debug","debug":true}' -H 'Content-Type:application/json' http://10.48.172.121:5003/api/process
```

The response returned:

```json
{
  "admin_token": "admin_token_12345",
  "flag": "THM{SUPPLY_CH41N_VULN3R4B1L1TY}",
  "internal_secret": "internal_secret_key_2024",
  "version": "1.2.3"
}
```

And I got the flag.

---

## **TASK 4**

**AS04: Cryptographic Failures**

### **What It Is**
Cryptographic failures happen when encryption is used incorrectly or not at all. This includes weak algorithms, hard-coded keys, poor key handling, or unencrypted sensitive data. These flaws let attackers access information that should be private.

### **Why It Matters**
Web applications rely on cryptography everywhere: protecting network traffic, securing stored data, verifying identities, and safeguarding secrets. When these controls fail, sensitive data such as passwords, tokens, or personal information can be exposed, leading to account takeovers or full-scale breaches.

Attackers can exploit these flaws through man-in-the-middle attacks, brute-force attacks on weak keys, or by simply discovering secrets that were never properly protected.

### **Challenge**
Firstly, we navigated to the website and saw an encrypted document on the main page.

We accessed the source code and saw something interesting:

```html
<script src="/static/js/decrypt.js"></script>
```

This was an accessible document in the page source.

We found the secret key, the encryption mode, and the key size:

```javascript
const SECRET_KEY = "my-secret-key-16";
const ENCRYPTION_MODE = "ECB";
const KEY_SIZE = 128;
```

Then we opened CyberChef, used Base64 and AES decryption, and got the flag:

```text
THM{CRYPTO_FAILURE_H4RDCOD3D_K3Y}
```

---

## **TASK 5**

**AS06: Insecure Design**

### **What It Is**
Insecure design happens when flawed logic or architecture is built into a system from the start. These flaws stem from skipped threat modelling, no design requirements or reviews, or accidental errors.

Moreover, with the introduction of AI assistants, AI systems can exacerbate insecure design. Developers often assume that models are safe, correct, or predictable, or that the code they produce is flaw-free. When an AI system can generate queries, write code, or classify users without limits, the risk is built into the design, leading to poor architectural patterns.

### **Challenge**
I tried a couple of API directories on the website and found that it was in:

<u><i>/api/messages/admin</i></u>

```text
THM{1NS3CUR3_D35IGN_4SSUMPT10N}
```
