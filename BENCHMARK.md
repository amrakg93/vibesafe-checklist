# 📊 Live Lovable App Security Benchmark

> Real scan results from live Lovable apps hosted on `*.lovable.app` subdomains.  
> Scanned June 19, 2026.

---

## Summary

| Metric | Result |
|--------|--------|
| Apps scanned | 6 |
| Apps missing CSP | **6/6 (100%)** |
| Apps missing X-Frame-Options | **6/6 (100%)** |
| Apps missing Permissions-Policy | **6/6 (100%)** |
| Apps with HSTS | 6/6 (100%) |
| Apps with X-Content-Type-Options | 6/6 (100%) |
| Apps with Referrer-Policy | 6/6 (100%) |
| Average headers missing per app | **3 out of 6 critical headers** |

---

## Apps Tested

| App | Missing Headers |
|-----|----------------|
| [JobCraftHub](https://jobcrafthubweb.lovable.app) | CSP, X-Frame-Options, Permissions-Policy |
| [Cloud Connect](https://cloudconnectweather.lovable.app) | CSP, X-Frame-Options, Permissions-Policy |
| [Berna Agar Portfolio](https://berna.lovable.app) | CSP, X-Frame-Options, Permissions-Policy |
| [Builder's Mind Podcast](https://builder-mind-website-oasis.lovable.app) | CSP, X-Frame-Options, Permissions-Policy |
| [AI Recess](https://ai-recess.lovable.app) | CSP, X-Frame-Options, Permissions-Policy |
| [Cavin Dennis Portfolio](https://cavin-impact-axis.lovable.app) | CSP, X-Frame-Options, Permissions-Policy |

---

## Broader Context

These results align with the Medium article [*"I Scanned Over 50 Lovable Apps For Security Vulnerabilities"*](https://medium.com/@jacobp96/i-scanned-over-50-lovable-apps-for-security-vulnerabilities-d05b2ad94006):

| Finding | Rate |
|---------|------|
| Publicly readable Supabase tables | **52%** |
| Unprotected RPC functions | **33%** |
| Weak password policies | **21%** |
| Missing Content-Security-Policy | **94%** |
| Missing X-Frame-Options | **95%** |
| Missing Permissions-Policy | **97%** |
| Missing HSTS | **100% of Lovable apps (not hosted on custom domain)** |
| Exposed secrets in JS bundles | Found in production apps |
| Unprotected Stripe webhooks | **Found in production** |

---

## What This Means

If you built your app with Lovable (or Bolt, Cursor, Replit, v0) and deployed it:

1. **Your database might be publicly readable** — check your Supabase RLS policies **and test them as an unauthenticated user**
2. **Your site can be embedded in an iframe** (clickjacking) — because X-Frame-Options is missing
3. **XSS payloads aren't blocked by CSP** — because Content-Security-Policy is missing
4. **Your site can access camera/mic/geolocation** — because Permissions-Policy is missing
5. **Your API routes might be unprotected** — because Lovable optimizes for function, not auth

---

## Methodology

All scans used standard `curl -sI` to check response headers. Findings are verifiable — run the same commands yourself:

```bash
curl -sI https://your-lovable-app.lovable.app | grep -iE "Content-Security-Policy|X-Frame-Options|Permissions-Policy"
```

If those lines don't appear, your app has the same vulnerabilities.

---

## Run a Full Scan

These are just the header checks. A complete security audit covers all 16 checks:

- Exposed secrets (Trufflehog)
- Semgrep static analysis (OWASP Top-10)
- Supabase RLS configuration
- Firebase rules
- Hardcoded credentials
- Unprotected routes
- Stripe webhook verification
- SQL injection
- SSL/TLS certificate
- Security headers
- Exposed sensitive files
- Secrets in JS bundles
- CORS misconfiguration
- Rate limiting
- Data breach check
- Trust badge

**Full scan → [VibeSafe](https://vibesafe.store) — $49 one-time, all 24 checks, report in 24 hours.**

---

## Updated Benchmark — 19 Apps Scanned (July 2026)

Using the VibeSafe 24-check scanner against real vibe-coded apps:

| Metric | Result |
|--------|--------|
| Apps scanned | 19 |
| Apps with ≥1 HIGH issue | **15/19 (79%)** |
| Missing CSP | **100%** |
| Missing X-Frame-Options | **80%** |
| Exposed dependency files | **40%** |
| Secrets in JS bundles | **15%** |

See VIBESAFE-WRITEUP.md for the full breakdown.
