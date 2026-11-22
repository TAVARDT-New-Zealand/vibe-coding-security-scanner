# Vibe Coding Security Scanner & AI-Generated App Auditor | TAVARDT New Zealand

[![Licence MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Vibe Coding Security](https://img.shields.io/badge/Vibe%20Coding-Security%20Hardened-00d26a.svg)](README.md)
[![Language](https://img.shields.io/badge/Language-English%20(New%20Zealand)-green.svg)](README.md)
[![Engineering](https://img.shields.io/badge/TAVARDT-Cybersecurity-5b3cf5.svg)](https://ag.tavardt.com/nz/cybersecurity/)

Open-source security scanner auditing live applications built with vibe coding tools (Cursor, Lovable, Bolt, v0). Detects leaked frontend API keys, Supabase RLS gaps, exposed sourcemaps, and sensitive files.

## Interactive Web Application

Access the live online tool and evaluate in real-time:

- [https://tavardt-new-zealand.github.io/vibe-coding-security-scanner/](https://tavardt-new-zealand.github.io/vibe-coding-security-scanner/)

---

## 1. Vibe Coding Security Scanner Overview

AI-assisted software creation and *vibe coding* allows teams to rapidly publish full-stack services using **Cursor, Lovable.dev, Bolt.new, v0, and Replit**.

However, rapid deployment without verification routinely leaks commercial API secrets, unshielded database endpoints, and raw source code directly into production.

This repository provides an automated Python scanner to audit **the public perimeter of any deployed web application**, identifying critical vulnerabilities with verified evidence.

---

## 2. Audited Vulnerabilities & Attack Vectors

| Risk Vector | Root Cause in Vibe Coding | Technical Impact |
| :--- | :--- | :--- |
| **Frontend API Key Leaks** | Private OpenAI (`sk-...`), Stripe, or AWS keys bundled in client code. | Visitors extract credentials from DevTools, draining API credits. |
| **Unprotected Supabase RLS** | Databases provisioned without active Row Level Security (RLS). | Attackers leverage the public `anon_key` to dump or wipe tables via REST. |
| **Exposed Production Sourcemaps** | Production web builds publishing `.js.map` files publicly. | Complete proprietary application source code is available for download. |
| **Sensitive File Disclosure** | Hosting configurations leaving `/.env` or `/.git/HEAD` public. | Exposure of backend environment secrets and infrastructure keys. |
| **Missing Security Headers** | Absence of CSP, HSTS, and X-Frame-Options headers. | Increased susceptibility to Clickjacking and transport-level tampering. |

---

## 3. Installation & Implementation Instructions

### Quick Installation

```bash
git clone https://github.com/TAVARDT-New-Zealand/vibe-coding-security-scanner.git
cd vibe-coding-security-scanner
```

### Scan a Target URL

```bash
python scanner.py https://your-app.vercel.app
```

### CLI Parameters

```bash
# Generate full structured JSON output
python scanner.py https://your-app.vercel.app --json

# Omit sourcemap inspection
python scanner.py https://your-app.vercel.app --no-sourcemaps
```

---

## 4. Security Architecture & Engineering Support

If your diagnostic identified leaked credentials, unprotected database tables, or requires professional security engineering:

* **Corporate Portal:** [TAVARDT New Zealand](https://ag.tavardt.com/nz/cybersecurity/)
* **Technical Contact:** [Contact Engineering Team](https://ag.tavardt.com/nz/contact/)

---

## Licence

Distributed under the open-source MIT Licence. View the [`LICENSE`](LICENSE) file for terms.  
Copyright (c) 2026 TAVARDT New Zealand.
