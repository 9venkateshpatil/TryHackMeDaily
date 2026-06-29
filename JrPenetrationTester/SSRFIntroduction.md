# SSRF Introduction

Server-Side Request Forgery (SSRF) is a vulnerability that allows an attacker to make the server-side application send HTTP requests to a destination of the attacker's choosing. In a typical SSRF attack, the attacker manipulates a parameter that the application uses to construct a server-side request, redirecting it to an internal service, a cloud metadata endpoint, or an external server under their control.

SSRF works because internal systems often trust the application server. Backend services, databases, and cloud infrastructure may accept requests from the server without much extra authentication because they assume anything coming from a trusted internal IP is legitimate. If an attacker can control where the server sends its requests, they effectively inherit that trust.

With a regular SSRF, if the attacker forces the server to fetch an internal admin page, the contents of that page appear directly in the HTTP response. That makes the vulnerability easy to confirm.

With a Blind SSRF, the application may show the same success message no matter what happened behind the scenes. Even then, blind SSRF can still be exploited. An attacker can point the request to a server they control, use something like Burp Collaborator, and watch for a callback. Response time differences or error messages can also reveal useful information about internal infrastructure.

---

## Full URL in a Parameter

This is the most direct form of SSRF. The application accepts a complete URL as input and uses it to make a server-side request. URL preview features, webhook configuration pages, and PDF generators often behave like this.

Example pattern:

| Input | Resulting Server-Side Request |
|---|---|
| `server=api` | `https://server.website.thm/api/item?id=2` |
| `server=server.website.thm/flag?id=9&x=` | `https://server.website.thm/flag?id=9&x=/api/item?id=2` |

In the second case, the `&x=` at the end causes whatever the application appends to the URL to be treated as an unused parameter, which neutralises it.

---

## Path Traversal in the URL

When an attacker only controls a path segment, directory traversal sequences can sometimes be used to reach endpoints outside the intended directory.

For example, if the application constructs requests like this:

```text
https://website.thm/stock?url=/item/123/details
```

An attacker may supply `/../admin`, causing the server to request:

```text
https://website.thm/admin
```

---

## Hidden Form Fields

Not every SSRF vector is visible in the address bar. Some are buried in the page's HTML source and are only visible through manual inspection or request interception.

A common example is a profile avatar feature where the image path is stored in a hidden form field:

```html
<input type="hidden" name="avatar" value="/images/avatars/default.png">
```

If the server fetches whatever path is inside that field, an attacker can change it using developer tools or a proxy such as Burp Suite and point it at an internal resource.

That is why thorough SSRF testing means checking form fields, API requests, and anything else that feeds a server-side request.

---

## Common Indicators

The following patterns are strong signs that an application may be vulnerable to SSRF:

- **Full URL in a parameter**  
  A complete URL appears as a query parameter in the address bar.

- **Hidden form fields**  
  These are not visible on the rendered page, but they may still control server-side requests.

- **Partial URL / hostname only**  
  The application accepts a hostname and builds the rest of the URL on the server.

- **Path only**  
  Only the path portion is user-controlled, while the scheme and hostname are added by the server.

Beyond those four patterns, other features can also expose SSRF vectors.
