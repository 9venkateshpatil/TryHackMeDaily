# Walking An Application

## TASK 3 — Page Source Analysis

### Opening Page Source

- Right Click → View Page Source
- Ctrl + U

---

### HTML Comment Flag

```html
<!--
This page is temporary while we work on the new homepage @ /new-home-beta
-->
```

Flag:

```text
THM{HTML_COMMENTS_ARE_DANGEROUS}
```

---

### Secret Link Flag

A hidden link named `secret` was found in the source code.

Flag:

```text
THM{NOT_A_SECRET_ANYMORE}
```

---

### Directory Listing Flag

Directory discovered:

```text
/assets
```

Flag:

```text
THM{INVALID_DIRECTORY_PERMISSIONS}
```

---

### Framework Flag

Comment discovered:

```html
<!--
Page Generated using the THM Framework v1.2
-->
```

Downloaded and extracted:

```bash
unzip tmp.zip
```

Flag:

```text
THM{KEEP_YOUR_SOFTWARE_UPDATED}
```

---

## TASK 4 — Browser Inspector

CSS modified:

```css
display: block;
```

to

```css
display: none;
```

Flag:

```text
THM{NOT_SO_HIDDEN}
```

---

## TASK 5 — Debugger

JavaScript file found:

```text
flash.js
```

Breakpoint placed on:

```javascript
flash['remove']();
```

Flag:

```text
THM{CATCH_ME_IF_YOU_CAN}
```

---

## TASK 6 — Network Tab

After sending a message through the contact form, the Network tab revealed:

```text
THM{GOT_AJAX_FLAG}
```
