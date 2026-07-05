# 📋 Vibe-Coded App Security Checklist

**24 checks every vibe-coded app needs.**

Two categories: **Pre-Launch** (source code) and **Post-Launch** (live site).  
Run the [automated scan](https://vibesafe.store) to do all 24 in one command.

---

## 📁 PRE-LAUNCH — Source Code (10 checks)

### 🔴 01 Exposed Secrets
API keys, tokens, passwords hardcoded in source files.

**Why it matters:** Anyone with access to your repo (or your JS bundles) has your keys.

**Fix it:** Move all secrets to environment variables. Use `.env` files (never commit them).

```
# BAD:
const supabaseKey = "eyJhbG...VCJ9..."

# GOOD:
const supabaseKey = process.env.SUPABASE_ANON_KEY
```

---

### 🔴 02 Static Analysis (OWASP Top-10)
Injection attacks, XSS, insecure patterns.

**Why it matters:** AI tools generate functional code, not secure code. They don't validate inputs by default.

**Fix it:** Add input sanitization. Never trust user input.

---

### 🔴 03 Supabase Row Level Security
**52% of Lovable apps have publicly readable database tables.**

**Why it matters:** Anyone with your Supabase URL and anon key (both visible in your frontend JS) can query your database.

**Fix it:** Enable RLS on every table. Verify policies are correct, not just enabled.

```sql
SELECT tablename, rowsecurity FROM pg_tables WHERE schemaname = 'public';
```

---

### 🔴 04 Firebase Rules
Open Firestore or Realtime Database rules allowing public read/write.

**Fix it:** Never use `allow read, write: if true;`. Require auth on all rules.

---

### 🔴 05 Hardcoded Credentials in All Files
API keys hide in JS, TS, Python, YAML, config files — not just `.env`.

**Fix it:** Search for `sk_live`, `pk_test`, `api_key`, `password`, `secret` across every file.

---

### 🟠 06 Unprotected Routes
API routes missing authentication checks.

**Fix it:** Add auth middleware to every API route. Verify session before returning data.

---

### 🟠 07 Stripe Webhook Verification
Webhook handlers that accept events without verifying the signature.

**Why it matters:** Anyone can send fake payment events and unlock paid features for free.

**Fix it:** Always call `stripe.webhooks.constructEvent()` with the raw body and signature header.

---

### 🟠 08 SQL Injection
String concatenation in SQL queries.

**Fix it:** Use parameterized queries. Never build SQL with string templates.

```sql
-- BAD: SELECT * FROM users WHERE email = '${userInput}'
-- GOOD: SELECT * FROM users WHERE email = $1
```

---

### 🟠 09 Dependency Audit
Outdated npm/pip packages with known CVEs.

**Why it matters:** AI tools pull in packages liberally. You may have lodash 4.17.20 (RCE), axios <1.7.4 (SSRF), or Next.js <14.2.21 (auth bypass).

**Fix it:** Run `npm audit` or `pip audit`. Upgrade vulnerable packages.

---

### 🔴 10 DB Config Exposure
Hardcoded Neon, PlanetScale, Supabase connection strings in config files.

**Fix it:** Use environment variables for all database URLs. Never commit `DATABASE_URL` or connection strings to your repo.

---

## 🌐 POST-LAUNCH — Live Site (14 checks)

### 🔴 11 SSL/TLS Certificate
Expired, weak, or missing TLS.

**Fix it:** Use Cloudflare or Let's Encrypt. Ensure TLS 1.2 minimum.

---

### 🟠 12 Security Headers
**94% of Lovable apps are missing Content-Security-Policy.**  
**95% are missing X-Frame-Options.**  
**97% are missing Permissions-Policy.**

**Fix it:** Add these headers:

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

---

### 🔴 13 Exposed Sensitive Files
`.env`, `.env.local`, `.git/config`, source maps publicly accessible.

**Fix it:** Block these in your server config, hosting platform, or Cloudflare WAF.

---

### 🔴 14 Secrets in JS Bundles
API keys embedded in production JavaScript bundles.

**Fix it:** Anon keys are expected, but service role keys and Stripe secret keys should never be in your frontend bundle.

---

### 🔴 15 CORS Misconfiguration
Wildcard origin with credentials enabled.

**Fix it:** Restrict to specific origins. Never use `Access-Control-Allow-Origin: *` with credentials.

---

### 🟠 16 Rate Limiting
No rate limiting on auth endpoints.

**Fix it:** Add Cloudflare Rate Limiting or a library like `express-rate-limit`.

---

### 🟠 17 Data Breach Check
Check if your domain or emails have been in a known breach.

**Fix it:** Use HaveIBeenPwned. Rotate compromised credentials.

---

### 🏅 18 Trust Badge
Earned by passing with no critical/high findings.

**Fix it:** Embed the generated badge HTML on your landing page. Publicly verifiable.

---

### 🟠 19 Source Map Exposure
`.js.map` files publicly accessible — full source code readable in DevTools.

**Fix it:** Disable source maps in production: `sourcemap: false` in Vite, `GENERATE_SOURCEMAP=false` in CRA.

---

### 🟠 20 Cookie Security
Cookies missing Secure, HttpOnly, or SameSite flags.

**Fix it:** Always set `Set-Cookie: name=value; Secure; HttpOnly; SameSite=Lax`.

---

### 🔴 21 Exposed Dependency Files
`package.json`, `yarn.lock`, `Dockerfile` publicly accessible.

**Fix it:** Block `/package.json` and `/yarn.lock` in your host config. Attackers use them to find vulnerable versions.

---

### 🟢 22 robots.txt + Sitemap Analysis
robots.txt may reveal hidden admin paths.

**Fix it:** Don't rely on robots.txt to hide sensitive paths — use authentication instead.

---

### 🟢 23 Subdomain Discovery
Dev, staging, admin, or API subdomains may be exposed.

**Fix it:** Ensure all subdomains are authenticated. Staging environments should not have production data.

---

### 🟢 24 Supply Chain / SBOM
Check for signed commits and software bill of materials (SBOM).

**Fix it:** Enable GPG/SSH commit signing. Generate SBOM with `cyclonedx-bom` for compliance.

---

## Done manually? Now run the automated scan.

These 24 checks take **5 minutes** with [VibeSafe](https://vibesafe.store).  
Pre-launch repo scan + post-launch live scan = **$49 one-time**, full report in 24 hours.

[→ Scan My App](https://vibesafe.store)
