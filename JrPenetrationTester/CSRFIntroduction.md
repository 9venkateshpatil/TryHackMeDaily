# CSRF Introduction
Modern web applications rely heavily on authenticated sessions to perform actions on behalf of users. When you log in to a website, your browser stores a session cookie that allows the application to recognise you in future requests. While this makes web applications convenient to use, it also creates opportunities for attackers to abuse this trust. One such attack is Cross-Site Request Forgery (CSRF).

Instead of stealing credentials, CSRF tricks a victim’s browser into performing actions on a website where the victim is already authenticated. Because the browser automatically includes cookies with requests, the web application may treat the malicious request as legitimate.

In this room, you will explore how attackers exploit this behaviour and how pentesters can identify vulnerable endpoints in real web applications.

---
## What is CSRF?

_
_
_


## What is CSRF?
CSRF is a web vulnerability where an attacker tricks a user’s browser into sending a request to a website where the user is already authenticated. Because the browser automatically includes session cookies with every request, the web application assumes the request was made intentionally by the user.

Instead of stealing login credentials, the attacker abuses the trust relationship between the browser and the web application. If the request performs a sensitive action, such as changing an email address or updating account settings, the attacker may be able to modify the victim’s account without their knowledge.

For example, imagine a user is logged in to an internal portal while browsing the internet. If the user visits a malicious webpage, that page can silently trigger a request to the portal. If the application does not verify the origin of the request, it may process it as if the user initiated it.

A typical CSRF attack usually follows three simple steps: 

The victim logs in to a legitimate web application, and their browser stores a session cookie.
The attacker tricks the victim into visiting a malicious webpage containing a crafted request.
The victim’s browser automatically sends the request to the target application along with the stored session cookie.
Because the request contains the valid session cookie, the server treats it as a legitimate user action.


_
_
_


## Understanding why CSRF works
Understanding why CSRF works is important before trying to exploit it. The vulnerability does not exist because browsers are broken. In fact, browsers are behaving exactly as they were designed to. The problem occurs when a web application trusts requests too much.

When a user logs in to a website, the server usually creates a session and sends a session cookie to the browser. This cookie acts like an identity card. Every time the browser sends a request to that website, the cookie is automatically included, so the server knows which user is making the request.

The key detail here is that the browser automatically sends cookies with every request to the same domain. It does not matter whether the request came from a legitimate page on the site or from a malicious webpage somewhere else on the internet.

For example, if a user is logged in to staffhub.thm and visits another website, that site can trigger a request to staffhub.thm. When the browser sends the request, it automatically includes the session cookie. From the server’s perspective, the request appears to come from the authenticated user.Why CSRF works.This is the exact behaviour attackers exploit during a CSRF attack. If the application does not verify the origin of the request, it may process the request without realising it was triggered by a malicious page. 


## Key Conditions for a CSRF Attack
Key Conditions for a CSRF Attack
For a CSRF attack to work, three main conditions usually have to exist:

The victim must be authenticated to the target application.
The application must perform a state-changing action such as updating settings or modifying account data.
The application must not verify whether the request came from a trusted source. 
If these conditions exist, an attacker may be able to perform actions on behalf of the victim without ever knowing their credentials.

_
_
_


## Before exploiting CSRF
Before exploiting CSRF, a pentester must first identify where the vulnerability might exist. Not every feature in a web application is a good target. CSRF attacks usually focus on actions that change data or modify account settings.

Whenever a user performs an action on a website, the browser sends an HTTP request to the server. Some requests simply retrieve information, while others modify the application’s state. These state-changing requests are the primary targets for CSRF attacks. 

As a pentester, you should carefully inspect application functionality and ask a simple question:

Can this action be triggered without verifying that the request actually came from the user?

If the answer is yes, the feature might be vulnerable to CSRF.


## Common Features Vulnerable to CSRF
Common Features Vulnerable to CSRF
Certain actions frequently appear in CSRF vulnerabilities because they modify important user data. While testing an application, pay special attention to requests that perform operations such as:

Common functions with CSRF vulnerabilities.If these requests do not include any protection mechanism, such as CSRF tokens, they may be susceptible to forged requests.


## GET vs POST - A Common Misconception
GET vs POST - A Common Misconception
Many developers assume that using the POST method automatically protects against CSRF attacks. This is not true.

Both GET and POST requests can be abused if the application does not verify that the request originated from a trusted page. In fact, many CSRF attacks rely on simple GET requests that can be triggered through links or images.

Because of this, pentesters should never rely solely on the request method when testing for CSRF vulnerabilities. 

_
_
_

