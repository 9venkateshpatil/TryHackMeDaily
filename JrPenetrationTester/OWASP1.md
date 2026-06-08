# ***OWASP Top 10 2025: IAAA Failures***

**Room: OWASP Top 10 2025: IAAA Failures**

**In this room we will focus on failures in the IAAA model.**

---

## **TASK 2**

IAAA is a simple way to think about how users and their actions are verified in applications. Each item plays a crucial role, and it is not possible to skip a level. That means, if a previous item is not being performed, you cannot perform the later items. The four items are:

- **Identity** - the unique account (e.g., user ID/email) that represents a person or service.
- **Authentication** - proving that identity (passwords, OTPs, passkeys).
- **Authorisation** - what that identity is allowed to do.
- **Accountability** - recording and alerting on who did what, when, and from where.

---

## **TASK 3**

**A01: Broken Access Control**

Broken Access Control happens when the server does not properly enforce who can access what on every request. A common occurrence of this is IDOR (Insecure Direct Object Reference): if changing an ID (like `?id=7` → `?id=6`) lets you see or edit someone else’s data, access control is broken.

In practice, this shows up as horizontal privilege escalation (same role, other user’s stuff) or vertical privilege escalation (jumping to admin-only actions) because the application trusts the client too much.

---

## **TASK 4**

**A07: Authentication Failures**

Authentication Failures happen when an application cannot reliably verify or bind a user’s identity. Common issues include:

- username enumeration
- weak or guessable passwords (no lockout or rate limits)
- logic flaws in the login/registration flow
- insecure session or cookie handling

If any of these are present, an attacker can often log in as someone else or bind a session to the wrong account.

---

## **TASK 5**

**A09: Logging & Alerting Failures**

When applications do not record or alert on security-relevant events, defenders cannot detect or investigate attacks. Good logging underpins accountability (being able to prove who did what, when, and from where). In practice, failures look like missing authentication events, vague error logs, no alerting on brute-force or privilege changes, short retention, or logs stored where attackers can tamper with them.
