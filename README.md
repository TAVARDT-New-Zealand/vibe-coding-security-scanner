# Vibe Coding Security & AI Architecture Auditor

[![Live Demo](https://img.shields.io/badge/Live%20Demo-GitHub%20Pages-5b3cf5.svg)](https://tavardt-new-zealand.github.io/vibe-coding-security-scanner/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![Security](https://img.shields.io/badge/Security-AI%20Architecture-blue.svg)](https://tavardt-new-zealand.github.io/vibe-coding-security-scanner/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Corporate defensive engineering specification, vulnerability mitigation framework, and deterministic auditing utility for software applications generated with artificial intelligence development assistants.

Maintained by the engineering practice at **TAVARDT**.

***

## 1. Architectural Reality of Vibe Coding

The paradigm shift toward accelerated AI-assisted programming allows engineering teams and solo founders to generate full-stack web applications at unprecedented velocity. However, autonomous code generators optimize primarily for immediate functional completion rather than defensive boundary isolation, least-privilege architecture, or asset sanitization.

When developers instruct AI models to ship features quickly, resulting software artifacts inherit systemic architectural vulnerabilities:

* **Compilation Leakage**: Development configurations and debugging hooks leak into production builds without compilation guards.
* **Service Key Exposure**: Administrative database service keys and AI provider tokens are mistakenly exposed in public client bundles.
* **Authorization Gaps**: Relational database tables lack declarative authorization rules and Row Level Security policies.
* **Perimeter Header Omission**: Crucial defensive HTTP headers are omitted from static and dynamic web responses.

***

## 2. Attack Vectors in AI-Generated Applications

Autonomous code generators consistently introduce five distinct vulnerability classes into production environments:

* **Production Sourcemap Exposure**: Bundling systems like Vite, Webpack, and Rollup frequently deploy `.js.map` files to public content delivery networks. These files permit full algorithmic deobfuscation of proprietary TypeScript sources, backend models, and hidden administrative routes.
* **Exposed AI Provider Keys and Service Roles**: Secret keys prefixed with public variables are routinely baked directly into static client assets. Attackers extract OpenAI, Anthropic, Gemini, and Supabase service role keys to execute unmetered API calls.
* **Absence of Row Level Security (RLS)**: Client-side SDKs frequently initialize database tables without enabling Row Level Security. Unauthenticated browser visitors can issue direct REST queries to extract customer records and sensitive transactional tables.
* **Omission of Core Defensive Headers**: Default web server definitions omit essential security headers like Content-Security-Policy, Strict-Transport-Security, and X-Frame-Options. This leaves the host vulnerable to cross-site script injection, clickjacking overlays, and MIME sniffing exploits.
* **Sensitive Environment File Routes**: Web servers frequently leave `.env` and `.env.local` files reachable over public HTTP endpoints. Automated scanners harvest database credentials, salts, and webhook tokens directly from root paths.

***

## 3. Workflow Integration and Execution

Integrate automated perimeter auditing directly into your GitHub Actions deployment pipelines:

```yaml
name: Vibe Coding Security Gate

on:
  push:
    branches: [main]
  pull_request:

jobs:
  security-audit:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Run TAVARDT Vibe Coding Security Scanner
        uses: TAVARDT-New-Zealand/vibe-coding-security-scanner@v1
        with:
          target-url: 'https://your-application.com'
```

### Local CLI Execution

You can also run the deterministic audit utility directly in terminal environments:

```bash
python audit_vibe_security.py https://your-application.com
```

***

## 4. Production Hardening Blueprints

This repository includes copy-ready hardening blueprints designed to secure modern AI application stacks:

### Vite Configuration (`vite.config.ts`)

Disables production sourcemaps completely and removes development artifact markers:

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  build: {
    sourcemap: false,
    chunkSizeWarningLimit: 1000
  }
});
```

### Next.js Configuration (`next.config.js`)

Suppresses client sourcemap generation and enforces defensive HTTP headers:

```javascript
const nextConfig = {
  productionBrowserSourceMaps: false,
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          { key: 'X-Frame-Options', value: 'DENY' },
          { key: 'X-Content-Type-Options', value: 'nosniff' },
          { key: 'Referrer-Policy', value: 'strict-origin-when-cross-origin' },
          { key: 'Strict-Transport-Security', value: 'max-age=63072000; includeSubDomains; preload' }
        ]
      }
    ];
  }
};

module.exports = nextConfig;
```

### Supabase Row Level Security Hardening

Enforces strict table isolation and revokes unauthenticated data exfiltration paths:

```sql
/* 1. Force Row Level Security across public schema tables */
ALTER TABLE public.profiles ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.organizations ENABLE ROW LEVEL SECURITY;

/* 2. Revoke default public execution privileges */
REVOKE ALL ON ALL TABLES IN SCHEMA public FROM anon;

/* 3. Grant strict authenticated read-only access scoped by user identity */
CREATE POLICY "Users can only view own profile" 
ON public.profiles FOR SELECT 
TO authenticated 
USING (auth.uid() = id);
```

***

## 5. Frequently Asked Technical Questions (FAQ)

### What specific security risks arise from Vibe Coding and AI software generators?
AI models generate code based on statistical pattern matching rather than architectural risk modeling. They routinely place secret API keys in client components, neglect to enable database row level security, and leave frontend bundler debugging flags enabled.

### Why is publishing sourcemaps to production environments dangerous?
Sourcemap files contain the exact inverse mapping of minified bundles back to original source code. Publishing js.map files enables external actors to reconstruct full application source code, review database schemas, and identify logic vulnerabilities.

### How should frontend applications consume AI APIs securely?
Frontend applications must never communicate directly with AI provider APIs using administrative tokens. All AI interactions must be proxied through secure backend API routes or serverless edge functions that validate authenticated user sessions and inject secrets securely.

### Does the deterministic Python auditor disrupt live production services?
The CLI utility operates strictly as a read-only passive diagnostic inspector. It transmits standard HTTP GET and HEAD requests to discover public scripts, headers, and exposed files without altering host states or degrading performance.

### How does the GitHub Action integrate into continuous deployment pipelines?
The Action accepts a target application URL and executes automated perimeter inspections on each push or pull request. Teams can incorporate this quality gate to catch exposed environment secrets, missing security headers, and sourcemap leaks prior to production release.

***

## Useful Links

* Corporate Portal: https://ag.tavardt.com/nz/
* Diagnostic Suite: https://tools.tavardt.com/nz/
* Technical Contact: https://ag.tavardt.com/nz/contact/

***

## License

Released under the MIT License.

***

© 2026 TAVARDT. All rights reserved.
