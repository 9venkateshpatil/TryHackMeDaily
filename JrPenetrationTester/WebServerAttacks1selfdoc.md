# Web Server Attacks 1

## TASK 1 — Introduction

We will look for misconfigurations and default settings that have not been changed in the top four types of web servers that dominate today’s market.

The servers are:

- Nginx
- Apache
- Python HTTP
- Express by Node.js

---

## TASK 2 — Identifying Web Servers

### Header Check

```bash
curl -sI http://lol
```

### Common Defaults

| Port | Server | Default Server Header |
|---|---|---|
| 80 | Apache2 | `Apache/2.4.x (Ubuntu)` |
| 8000 | Python HTTP Server | `SimpleHTTP/0.6 Python/3.xx.x` |
| 3000 | Node.js / Express | `None` (set by application) |
| 8080 | Nginx | `nginx/1.xx.x` |

Express usually identifies itself through the `x-powered-by` header in the HTTP response. Apache and Nginx usually reveal themselves in the `Server` header.

If you only have a browser, you can still check the running server through the Network tab in browser developer tools.

Another way to identify the server is by sending random non-existent page requests with `curl -s`, since the response is distinctive for each server:

- Python returns a plain text response
- Nginx includes its version in the HTML footer
- Apache includes its name in the page body

---

## TASK 3 — Python HTTP Server

Python gives a built-in HTTP server with a single command:

```bash
python3 -m http.server 8000
```

Developers use this to test static websites or share files among hosts on the same network. But there is no authentication or real security, so anyone can access the exposed files.

It serves the entire working directory, including dotfiles like `.env`, with no blocklist and no config file protection. If a file exists, anyone can access it.

### Example Directory Listing

```bash
curl -s http://venk.lol:8000/
```

Example output:

```text
<li><a href=".env">.env</a></li>
<li><a href="backup.zip">backup.zip</a></li>
<li><a href="config.txt">config.txt</a></li>
<li><a href="notes.txt">notes.txt</a></li>
```

### Example Dotfile Exposure

```bash
curl -s http://venk.lol:8000/.env
```

Example output:

```text
SECRET_KEY=dev-secret-key-do-not-use
DATABASE_URL=postgresql://webapp:S3cur3DBPass!@localhost/production
DEBUG=True
```

### Example Archive Download and Inspection

```bash
curl -s http://venk.lol:8000/backup.zip -o backup.zip
unzip backup.zip -d lol
cat db_dump.sql
```

The archive contained a database dump with the flag:

```text
THM{py_server_exposed}
```

---

## TASK 4 — Apache2

Apache is one of the most widely used web servers, so you are likely to encounter it often.

It leaves a few default things behind, such as:

- directory listing on certain paths
- the `server-status` module
- backup files in the document root

Version detection is done with:

```bash
curl -sI
```

Apache’s `Options +Indexes` directive tells the server to display a file listing when a directory does not have an `index.html` or similar default file. This is sometimes enabled intentionally for internal file-share directories and accidentally left on paths that contain sensitive data.

### Example Flag

A directory named `files` contained `internal-notes.txt` with the flag:

```text
THM{apache_dir_listing}
```

Apache can also expose a status page powered by the `mod_status` module. If configured correctly, it is accessible from localhost only; if not configured correctly, it may be reachable at `/server-status`.

You can also enumerate further using `gobuster`.

Always check `.bak` and `.passwd` files, as they may contain credentials.

---

## TASK 5 — Node.js

Node.js provides dynamic web applications unlike traditional Apache and Python HTTP servers. Attackers look for Express applications because developers often deploy code that worked in development and leave it live as-is, which can reveal structure and even credentials.

Developers sometimes create a debug endpoint that lists every route for convenience during development and then forget to delete it.

A common misconfiguration that lists the routes is:

```bash
curl -s http://venk.lol:3000/api/routes
```

Example response:

```json
[{"method":"GET","path":"/"},{"method":"GET","path":"/api/users"},{"method":"GET","path":"/api/routes"},{"method":"GET","path":"/api/debug/env"}]
```

The `/api/debug/env` endpoint is another misconfigured and accessible environment file that may contain useful credentials:

```bash
curl -s http://venk.lol:3000/api/debug/env
```

Example response:

```json
{"NODE_ENV":"development","DB_PASSWORD":"NodeDBPass2024!","PORT":"3000","DB_HOST":"localhost:5432","APP_NAME":"company-portal"}
```

You can also check static files, which may expose a `config.js` file:

```bash
curl -s http://venk.lol:3000/static/config.js
```

Example output:

```javascript
// Client-side configuration
const API_BASE = 'http://internal-api.company.local:8080';
const DEBUG = true;
const VERSION = '1.2.0';
// flag: THM{node_debug_exposed}
```

---

## TASK 6 — Nginx

Nginx is commonly used as a reverse proxy, load balancer, or static file server because of its high performance.

This task was similar to the Apache and Express tasks.

Here too, directory listing is sometimes available under a `/files` directory, and `/nginx_status` gives the current status of the server.

---

## TASK 7 — Comparison

| Misconfiguration | Apache | Python HTTP | Node.js | Nginx |
|---|---|---|---|---|
| Version disclosure in headers | Yes | Yes | Partial | Yes |
| Directory listing | `/files/` | Root path | N/A | `/files/` |
| Exposed status or debug endpoint | `/server-status` | N/A | `/api/debug/env`, `/api/routes` | `/nginx_status` |
| Sensitive files accessible | `backup.bak`, `internal-notes.txt` | `.env`, `notes.txt`, `backup.zip` | `config.js` | `server-config.txt`, `deploy-notes.txt` |
| Missing security headers | All | All | All | All |
