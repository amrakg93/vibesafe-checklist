# 📋 Vibe-Coded App Security Checklist

**16 checks every vibe-coded app needs.**

Two categories: **Pre-Launch** (source code) and **Post-Launch** (live site).  
Run the [automated scan](https://vibesafe.store) to do all 16 in one command.

---

## 📁 PRE-LAUNCH — Source Code (8 checks)

### 🔴 01 Exposed Secrets
API keys, tokens, passwords hardcoded in source files.

**Why it matters:** Anyone with access to your repo (or your JS bundles) has your keys.

**Fix it:** Move all secrets to environment variables. Use `.env` files (never commit them).

```
# BAD:
const supabaseKey = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."

# GOOD:
const supabaseKey = process.env.SUPABASE_ANON_KEY
```

---

### 🔴 02 Static Analysis (OWASP Top-10)
Injection attacks, XSS, insecure patterns.

**Why it matters:** AI tools generate functional code, not secure code. They don't validate inputs by default.

**Fix it:** Add input sanitization. Never trust user input.

**Tools:** Semgrep, ESLint security plugin, or run [VibeSafe](https://vibesafe.store).

---

### 🔴 03 Supabase Row Level Security
**52% of Lovable apps have publicly readable database tables.**

**Why it matters:** Anyone with your Supabase URL and anon key (both visible in your frontend JS) can query your database.

**Fix it:** Enable RLS on every table. Verify policies are correct, not just enabled.

```sql
-- Check if RLS is enabled:
SELECT tablename, rowsecurity FROM pg_tables WHERE schemaname = 'public';

-- BAD: RLS disabled → anyone can read/write
-- GOOD: RLS enabled with proper policies
```

---

### 🔴 04 Firebase Rules
Open Firestore or Realtime Database rules allowing public read/write.

**Fix it:** Lock down rules. Never use `allow read, write: if true;`.

```
// BAD:
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}

// GOOD:
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

---

### 🔴 05 Hardcoded Credentials in All Files
Not just `.env` files. API keys hide in JS, TS, Python, YAML, config files.

**Fix it:** Search your entire codebase for patterns like `sk_live`, `pk_test`, `api_key`, `password`, `secret`. Use a scanner.

---

### 🟠 06 Unprotected Routes
API routes missing authentication checks.

**Why it matters:** Anyone who finds the endpoint URL can access it. No auth required.

**Fix it:** Add middleware to every API route.

```javascript
// Next.js App Router example:
export async function GET(request) {
  const { data: { user } } = await supabase.auth.getUser();
  if (!user) {
    return Response.json({ error: "Unauthorized" }, { status: 401 });
  }
  // route logic here
}
```

---

### 🟠 07 Stripe Webhook Verification
Webhook handlers that accept events without verifying the signature.

**Why it matters:** Anyone can send fake payment events. Your app grants access without payment.

**Fix it:** Always verify the Stripe signature.

```javascript
// BAD:
app.post('/webhook', (req, res) => {
  const event = req.body; // UNSAFE — anyone can fake this
  // grant access...
});

// GOOD:
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);
app.post('/webhook', express.raw({type: 'application/json'}), (req, res) => {
  const sig = req.headers['stripe-signature'];
  const event = stripe.webhooks.constructEvent(req.body, sig, endpointSecret);
  // now safe to process
});
```

---

### 🟠 08 SQL Injection
String concatenation in SQL queries.

**Fix it:** Use parameterized queries. Never build SQL with string templates.

```sql
-- BAD (vulnerable):
SELECT * FROM users WHERE email = '${userInput}'

-- GOOD (safe):
SELECT * FROM users WHERE email = $1
```

---

## 🌐 POST-LAUNCH — Live Site (8 checks)

### 🔴 09 SSL/TLS Certificate
Expired, weak, or missing TLS.

**Fix it:** Use Cloudflare or Let's Encrypt. Ensure TLS 1.2 minimum.

---

### 🟠 10 Security Headers (👈 every Lovable app fails this)
**94% of Lovable apps are missing Content-Security-Policy.**  
**95% are missing X-Frame-Options.**  
**97% are missing Permissions-Policy.**

**Why it matters:** Missing headers make your app vulnerable to XSS, clickjacking, and data theft.

**Fix it:** Add these headers (copy-paste for Cloudflare → Transform Rules):

```
Strict-Transport-Security: max-age=31536000; includeSubDomains
Content-Security-Policy: default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

---

### 🔴 11 Exposed Sensitive Files
`.env`, `.env.local`, `.git/config`, source maps publicly accessible.

**Fix it:** Block these in your server config, hosting platform, or Cloudflare WAF.

---

### 🔴 12 Secrets in JS Bundles
API keys embedded in production JavaScript.

**Fix it:** Anon keys for Supabase/Firebase are expected, but service role keys and Stripe secret keys should never be in your frontend bundle. Review what's shipped.

---

### 🔴 13 CORS Misconfiguration
Wildcard origin with credentials enabled.

**Fix it:** Restrict CORS to specific origins. Never use `Access-Control-Allow-Origin: *` with credentials.

---

### 🟠 14 Rate Limiting
No rate limiting on auth endpoints.

**Why it matters:** Brute force attacks are trivial. Your login endpoint gets hammered.

**Fix it:** Add rate limiting with Cloudflare or a library like `express-rate-limit`.

---

### 🟠 15 Data Breach Check
Check if your domain or emails have been in a known breach.

**Fix it:** Use [HaveIBeenPwned](https://haveibeenpwned.com). Rotate any compromised credentials.

---

### 🏅 16 Trust Badge
Earned by passing all checks with no critical/high findings.

Install the badge on your landing page. Publicly verifiable, auto-updating.

---

## Done manually? Now run the automated scan.

These 16 checks take **5 minutes** with [VibeSafe](https://vibesafe.store).  
Pre-launch repo scan + post-launch live scan = **$49 one-time**, full report in 24 hours.

[→ Scan My App](https://vibesafe.store)
