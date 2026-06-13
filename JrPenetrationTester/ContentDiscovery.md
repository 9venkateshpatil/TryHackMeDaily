# Content Discovery

## TASK 2 — robots.txt and sitemap.xml

The `robots.txt` file tells search engine crawlers which pages they may index. Site owners often list sensitive directories there to prevent them from appearing in search results, which makes it a ready-made list of interesting locations for a penetration tester.

This `robots.txt` file tells web crawlers how to interact with the site. It allows all bots to access most of the site (`Allow: /`) but asks them not to visit `/staff-portal`. Keep in mind that this is only a guideline for bots, not a security control, so restricted paths may still be accessible if visited directly.

It reveals information that is important and not meant to be accessed by someone who is not authorized.

### `sitemap.xml`

Unlike `robots.txt`, which restricts crawlers, `sitemap.xml` tells search engines which pages the owner wants listed. These files sometimes include staging pages, old content, or URLs that are hard to reach via the normal site.

It can reveal additional useful information if you examine the code carefully.

---

## TASK 3 — HTTP Headers and Framework Stack

When a web server responds to a request, it includes HTTP headers that can reveal useful technical details. Headers like `Server` and `X-Powered-By` often expose the web server software and the language or framework the application runs on.

### Framework Stack

Once you have identified the framework from the favicon, headers, or by inspecting the page source for comments and copyright notices, visit the framework’s own website to learn more. Documentation pages often describe default directory structures, admin panel paths, and default credentials.

This helps with additional enumeration by using HTTP headers and then checking the framework documentation for normal or default directory structures.

---

## TASK 4 — Google Hacking / Dorking and Wappalyzer

Google’s advanced search operators let you filter results in ways that can surface sensitive content indexed from your target. By combining operators, you can find exposed admin panels, leaked documents, and login pages that the site owner never intended to be public.

### Wappalyzer

Wappalyzer is a browser extension and online tool that identifies the technologies a website uses, including:

- frameworks
- CMS platforms
- CDNs
- analytics tools
- payment processors
- version numbers

It is useful when searching for known vulnerabilities. Install it from your browser’s extension store and visit any site to see the tech stack immediately.

---

## TASK 5 — Wayback Machine and S3 Buckets

The Wayback Machine is an archive of the Internet dating back to the late 1990s. Search for a domain and you will see every snapshot captured over time. This is useful for finding pages that have been removed from the live site but may still be accessible, such as old login forms, forgotten API endpoints, or content that was published briefly before being taken down.

### S3 Buckets

Amazon S3 is a cloud storage platform that many organisations use to host files and static website content. The URL format for an S3 bucket is:

```text
https://{name}.s3.amazonaws.com
```

Bucket owners set permissions, but misconfigurations are common. A publicly accessible bucket can expose files that were never meant to be seen.

---

## TASK 7 — Subdomain Enumeration

For example, if TryHackMe owns `tryhackme.thm` and `mobile.tryhackme.thm`, there may be a vulnerability in `mobile.tryhackme.thm` that is not present in `tryhackme.thm`. That is why it is important to search for subdomains as well.

Common enumeration methods include:

- directory enumeration
- DNS enumeration
- virtual host enumeration
