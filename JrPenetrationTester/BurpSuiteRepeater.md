# Burp Suite Repeater

## TASK 2 — What is Repeater?

Repeater helps us modify and resend intercepted requests to our desired targets.

It takes requests captured in **Burp Proxy**, allows us to alter them, and resend them repeatedly as needed.

We can also manually create requests in a way similar to using `curl`.

### Why Repeater is Useful

- Modify intercepted requests
- Resend requests multiple times
- Test different payloads quickly
- Manually craft custom HTTP requests
- View responses immediately

Its repetitive workflow makes it one of the most commonly used Burp Suite tools because it provides a GUI for crafting and testing requests.

---

## TASK 3 — Basic Usage

While manual request crafting is possible, most users send requests directly from the **Proxy** tab to **Repeater**.

### Common Methods

#### Keyboard Shortcut

```text
Ctrl + R
```

#### Right Click Menu

```text
Right Click → Send to Repeater
```

Once a request is inside Repeater, you can edit any part of it by clicking directly on the section you want to modify.

After making changes, send the request again and observe how the response changes.

### Typical Workflow

1. Intercept the request in Proxy.
2. Send the request to Repeater.
3. Modify parameters, headers, cookies, or the body.
4. Send the request.
5. Analyse the response.
6. Repeat as needed.

---

## TASK 4 — Message Analysis Toolbar

The message analysis toolbar allows requests and responses to be viewed in multiple formats.

### Pretty

A formatted and easier-to-read representation of the message.

### Raw

Displays the complete raw HTTP request or response.

### Hex

Shows the data in hexadecimal format.

### Render

Renders the response similarly to how it would appear in a browser, making it easier to understand the resulting webpage.

---

## Summary

Key concepts covered in this room:

- Understanding Burp Repeater
- Sending requests from Proxy to Repeater
- Editing and resending requests
- Analysing responses
- Using Pretty, Raw, Hex, and Render views
