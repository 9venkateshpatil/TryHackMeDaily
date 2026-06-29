# Session Management

Thinking about your interactions with web applications, you should realise that you do not send a web application your username and password on every request. Instead, after authentication, you are given a session. That session is what the application uses to keep track of your state, remember your actions, and decide whether you are allowed to do what you are trying to do. Session management is all about making sure these steps work correctly. If they do not, a threat actor may be able to compromise your session and effectively hijack it.

---

## Session Creation

You might think the first step in the lifecycle happens only after you provide your credentials, such as a username and password. However, on many web applications, the initial session is already created when you visit the site. Some applications do this so they can track your actions even before authentication. That said, the main focus here is authenticated sessions.

Once you provide your username and password, you receive a session value that is sent with each new request. How that session value is generated, used, and stored is a very important part of secure session creation.

---

## Session Tracking

Once you receive your session value, it is submitted with each request. This allows the application to track your actions even though HTTP itself is stateless.

With every request, the application can recover the session value, perform a server-side lookup, and understand who the session belongs to and what permissions it has. If something goes wrong in this process, it may allow an attacker to hijack the session or impersonate a user.

---

## Session Expiry

Because HTTP is stateless, a user may stop using the application at any time, such as by closing the tab or the browser. The application does not automatically know that this happened. That is where session expiry comes in.

A session value should have a lifetime attached to it. If that lifetime expires and an old session value is sent back to the application, it should be rejected. At that point, the user should be redirected to the login page so a new session can begin.

---

## Session Termination

In some cases, the user may explicitly log out. When that happens, the application should terminate the user's session.

This is similar to expiry, but it is different because the session can still be within its lifetime while being invalidated manually. Problems in termination handling could let an attacker keep using a session that should already be dead.

---

## Cookie-Based Session Management

Cookie-based session management is often the old-school way of handling sessions. Once the application wants to start tracking the user, it sends a `Set-Cookie` header in the response. The browser stores that cookie automatically.

Example:

```text
Set-Cookie: session=12345;
```

The browser creates a cookie entry named `session` with the value `12345`, valid for the domain that sent it.

Several attributes can also be added to the header. Those attributes control how the cookie behaves, how long it lives, and when it is sent.

---

## Token-Based Session Management

Token-based session management is a newer approach. Instead of relying on the browser's automatic cookie handling, it uses client-side code.

After authentication, the web application provides a token in the request body. Client-side JavaScript stores that token in the browser's `LocalStorage`.

When a new request is made, JavaScript reads the token from storage and attaches it as a header. A common example is a JSON Web Token (JWT), which is passed through the `Authorization: Bearer` header.

Since this does not rely on the browser's built-in cookie system, it can feel a bit like the wild west. Standards exist, but nothing forces every application to follow them properly.
