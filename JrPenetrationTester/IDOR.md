# IDOR

Web applications rely on identifiers to distinguish one object from another. A user profile, invoice, support ticket, or private document usually has some kind of reference, often a number or a string, that the application uses internally to locate it. When an application lets the user supply that reference and then retrieves the object without checking whether the user is allowed to access it, the result is an Insecure Direct Object Reference, or IDOR.

IDOR is an access control vulnerability. It sits in the Broken Access Control category in the OWASP Top 10, and the same issue appears in the OWASP API Security Top 10 as Broken Object Level Authorisation, or BOLA. The wording changes depending on the context, but the root cause is the same: the server does not verify that the authenticated user has permission to interact with the specific object they requested.

---

## The Simple Version

The simplest form of IDOR happens when an application places an object identifier directly in a URL parameter and then uses it to query the back end without any authorisation check.

Suppose you sign up for an online service and visit your profile page:

```text
http://online-service.thm/profile?user_id=1305
```

The `user_id` parameter tells the server which record to fetch. In this example, record `1305` belongs to your account, so the page shows your details as expected.

Now imagine changing the parameter to:

```text
http://online-service.thm/profile?user_id=1000
```

If the page shows someone else's profile, the application has an IDOR vulnerability. The server accepted the modified parameter, looked up record `1000`, and returned it. At no point did it verify whether your session was allowed to view that record.

The authentication layer is still working. The application knows who you are because you are logged in. The problem is the missing authorisation layer. There is no server-side logic asking questions like:

- Does this session belong to user `1000`?
- Is this user allowed to view this profile?

The server simply assumes that any authenticated user is allowed to access any object they reference.

---

## Hashed Identifiers

Some applications hash identifiers before including them in requests. A hashed value looks like a fixed-length string of hexadecimal characters, which can make it seem as though the reference is hidden.

That does not always help much.

If the input to the hash function is predictable, such as a sequential integer, an attacker can reproduce the hashing process and generate valid references to other objects.

For example, if an application uses MD5 to hash user IDs, the integer `123` produces:

```text
202cb962ac59075b964b07152d234b70
```

On its own, that output does not reveal the original value. But if the attacker suspects that the application is hashing sequential integers, they can calculate hashes for `1`, `2`, `3`, `4`, and so on, then compare them to the values seen in the application. A match confirms the scheme.

Unlike encoding, hashing is one-way. There is no mathematical operation that reverses an MD5 hash to its original input. But for short and predictable inputs, that protection is weak.

Services such as CrackStation keep lookup tables with billions of precomputed hash-to-value pairs, so looking up a hashed integer can be almost instant.

---
