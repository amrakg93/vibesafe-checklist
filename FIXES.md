# 🔧 Copy-Paste Fixes

Quick fixes for the most common vulnerability patterns in vibe-coded apps.

---

## 1. Fix Missing Security Headers (Cloudflare)

If your app is behind Cloudflare (most Lovable/Bolt apps are):

1. Go to Cloudflare Dashboard → **Rules** → **Transform Rules**
2. Click **Add Rule** → **Modify Response Headers**
3. Add these headers:

| Header | Value |
|--------|-------|
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` |
| `Content-Security-Policy` | `default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'` |
| `X-Frame-Options` | `DENY` |
| `X-Content-Type-Options` | `nosniff` |
| `Referrer-Policy` | `strict-origin-when-cross-origin` |
| `Permissions-Policy` | `camera=(), microphone=(), geolocation=()` |

---

## 2. Fix Supabase RLS

```sql
-- List all tables with RLS status
SELECT tablename, rowsecurity 
FROM pg_tables 
WHERE schemaname = 'public';

-- Enable RLS on a table
ALTER TABLE your_table_name ENABLE ROW LEVEL SECURITY;

-- Example: users can only read their own data
CREATE POLICY "Users can view own data" 
ON your_table_name 
FOR SELECT 
USING (auth.uid() = user_id);
```

Then in Lovable chat, paste:
```
Review all RLS policies in my Supabase database. 
Fix any policies that expose personally identifiable information 
or let users access other users' data.
```

---

## 3. Fix Unprotected API Routes

### Next.js App Router
```javascript
import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { cookies } from 'next/headers'

export async function GET(request) {
  const supabase = createRouteHandlerClient({ cookies })
  const { data: { user } } = await supabase.auth.getUser()
  
  if (!user) {
    return Response.json({ error: "Unauthorized" }, { status: 401 })
  }
  
  // your route logic here
}
```

### Express.js
```javascript
function requireAuth(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1]
  if (!token) return res.status(401).json({ error: "Unauthorized" })
  // verify token...
  next()
}

app.get('/api/data', requireAuth, (req, res) => {
  res.json({ data: 'protected' })
})
```

---

## 4. Fix Stripe Webhook Verification

```javascript
import Stripe from 'stripe';
const stripe = new Stripe(process.env.STRIPE_SECRET_KEY);

export async function POST(request) {
  const body = await request.text();
  const sig = request.headers.get('stripe-signature');
  
  let event;
  try {
    event = stripe.webhooks.constructEvent(body, sig, process.env.STRIPE_WEBHOOK_SECRET);
  } catch (err) {
    return Response.json({ error: 'Invalid signature' }, { status: 400 });
  }
  
  // safe to process event
}
```

---

## 5. Fix CORS

```javascript
// BAD — allows any site to make authenticated requests
res.setHeader('Access-Control-Allow-Origin', '*');
res.setHeader('Access-Control-Allow-Credentials', 'true');

// GOOD — restrict to your domain
const allowedOrigins = ['https://yourapp.com', 'https://www.yourapp.com'];
const origin = request.headers.get('origin');
if (allowedOrigins.includes(origin)) {
  res.setHeader('Access-Control-Allow-Origin', origin);
  res.setHeader('Access-Control-Allow-Credentials', 'true');
}
```

---

## 6. Add Rate Limiting

### Cloudflare (easiest)
Go to **Security** → **WAF** → **Rate Limiting Rules** → Create rule:
- **Field:** URI Path
- **Value:** `/api/auth/*`
- **Requests:** 10 per 60 seconds

### In code
```bash
npm install express-rate-limit
```

```javascript
import rateLimit from 'express-rate-limit';

const authLimiter = rateLimit({
  windowMs: 60 * 1000, // 1 minute
  max: 10, // 10 requests
  message: 'Too many requests, please try again later.'
});

app.use('/api/auth', authLimiter);
```

---

## 7. Block Exposed Files

Add to your `vercel.json` or `_redirects`:

### Vercel
```json
{
  "redirects": [
    { "source": "/.env(.*)", "destination": "/404", "permanent": false },
    { "source": "/.git(.*)", "destination": "/404", "permanent": false }
  ]
}
```

### Cloudflare WAF
Create a WAF rule blocking requests to:
- `.env`
- `.git`
- `*.map` (source maps in production)

---

## 8. Fix Source Map Exposure

Source maps let anyone read your full uncompiled code in DevTools.
```js
// Vite — disable in production
// vite.config.ts
export default defineConfig({
  build: { sourcemap: false }
})

// Next.js
// next.config.js
module.exports = {
  productionBrowserSourceMaps: false,
}

// Create React App
// Build with: GENERATE_SOURCEMAP=false react-scripts build
```

---

## 9. Fix Cookie Security

```javascript
// Express.js — secure cookies
res.cookie('session', token, {
  httpOnly: true,    // not accessible via JavaScript
  secure: true,      // only sent over HTTPS
  sameSite: 'lax',   // prevents CSRF
  maxAge: 7 * 24 * 60 * 60 * 1000 // 7 days
});
```

---

## 10. Fix Exposed Dependency Files

Block these in your server config:
- `/package.json`
- `/package-lock.json`
- `/yarn.lock`
- `/requirements.txt`
- `/Dockerfile`

### Hostinger (.htaccess)
```apache
<FilesMatch "\.(json|lock|txt)$">
  Order allow,deny
  Deny from all
</FilesMatch>
```

### Vercel (vercel.json)
```json
{ "source": "/package.json", "destination": "/404", "permanent": false }
```

---

## 11. Fix Dependency Vulnerabilities

```bash
# Check for known CVEs
npm audit

# Fix automatically
npm audit fix

# For specific packages
npm install lodash@4.17.21       # fixes CVE-2024-4068
npm install axios@1.7.4          # fixes CVE-2024-39338
npm install next@14.2.21         # fixes CVE-2024-51479
```

---

## 12. Enable Commit Signing (Supply Chain)

```bash
# GPG signing
git config --global user.signingkey YOUR_GPG_KEY
git config --global commit.gpgsign true

# SSH signing
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true
```

---

## Run the Automated Scan

Each fix above was identified from actual vibe-coded app vulnerabilities.  
A [VibeSafe scan](https://vibesafe.store) catches all 24 checks automatically.  
**$49 one-time, report within 24 hours.**
