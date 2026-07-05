# 🛡️ VibeSafe — Security Scanner for AI-Built Apps

> **24 security checks for apps built with AI coding tools.**
> Lovable · Bolt · Cursor · Replit · v0 · Claude Code · Codex · Windsurf

---

## What This Repo Contains

| File | What It Is |
|------|------------|
| [`VIBESAFE-WRITEUP.md`](VIBESAFE-WRITEUP.md) | Full product writeup with real proof — 19 apps scanned, dogfood results, 24-check breakdown |
| [`CHECKLIST.md`](CHECKLIST.md) | The complete 24-point security checklist — pre-launch and post-launch |
| [`BENCHMARK.md`](BENCHMARK.md) | Real scan data from live Lovable apps |
| [`FIXES.md`](FIXES.md) | Copy-paste fixes for every issue |
| [`BLUEPRINT-TEMPLATE.md`](BLUEPRINT-TEMPLATE.md) | Reusable project blueprint template for all VibeApp Studio projects |

---

## The Problem

AI coding tools are amazing at shipping fast. Security is not their priority.

| Stat | Source |
|------|--------|
| **91.5%** of vibe-coded apps have at least one vulnerability | VibeSafe research |
| **63%** have critical/high vulnerabilities | Medium scan of 62 Lovable apps |
| **52%** of Supabase-backed Lovable apps have publicly readable tables | Vibe App Scanner |
| **95%** missing X-Frame-Options (clickjacking protection) | This repo's scan data |
| **94%** missing Content-Security-Policy (XSS prevention) | This repo's scan data |
| **380,000** vibe-coded apps exposed online | VibeSafe |
| **75%** of scanned apps had ≥1 HIGH-severity issue | VibeSafe 19-app benchmark (July 2026) |

---

## Quick Start

```bash
# Quick manual check (3 seconds):
curl -sI https://yourapp.com | grep -iE "Strict-Transport-Security|Content-Security-Policy|X-Frame-Options"

# Full 24-check automated scan → https://vibesafe.store
```

---

## The 24 Checks

### Pre-Launch (10 checks — run against your source code)
1. Exposed Secrets (Trufflehog)
2. Static Analysis (Semgrep, OWASP Top-10)
3. Supabase RLS Configuration
4. Firebase Security Rules
5. Hardcoded Credentials (150+ patterns)
6. API Routes Missing Authentication
7. Stripe Webhook Signature Verification
8. SQL Injection
9. Dependency Audit (known CVEs)
10. DB Config Exposure

### Post-Launch (14 checks — run against your live URL)
11. SSL/TLS Certificate
12. Security Headers (CSP, HSTS, XFO, XCTO, Referrer, Permissions)
13. Exposed Sensitive Files (.env, .git)
14. Secrets in JS Bundles
15. CORS Misconfiguration
16. Rate Limiting on Auth Endpoints
17. Data Breach Check (HaveIBeenPwned)
18. Trust Badge Generator
19. Source Map Exposure
20. Cookie Security (Secure, HttpOnly, SameSite)
21. Exposed Dependency Files
22. robots.txt + Sitemap Analysis
23. Subdomain Discovery (dev, staging, admin)
24. Supply Chain / SBOM Check

---

## Products

| Product | Price | What You Get |
|---------|:-----:|--------------|
| Post-Launch Scan | $19 | 14 live URL checks, report in 24h |
| Pre-Launch Audit | $39 | 10 source code checks, report in 24h |
| Full Bundle | $49 | Both scans + CI/CD badge |
| Continuous | $39/mo | Weekly rescans + breach monitoring |

→ **[vibesafe.store](https://vibesafe.store)**

---

## Real Proof

**19 real apps scanned in July 2026.** 75% had ≥1 HIGH issue. The full writeup at [`VIBESAFE-WRITEUP.md`](VIBESAFE-WRITEUP.md) breaks down every scan.

**Dogfood test:** VibeSafe scanned itself. Result: **0 critical, 0 high — trust badge earned.**

```bash
python audit.py --url https://vibesafe.store
✅ SSL/TLS valid · ✅ CSP configured · ✅ HSTS present
✅ X-Frame-Options: DENY · ✅ No exposed files · ✅ No secrets
🏅 Trust badge earned
```

---

## CI/CD Integration

```markdown
[![VibeSafe](https://vibesafe-backend-production.up.railway.app/api/badge?url=https://yoursite.com)](https://vibesafe.store)
```

Add to your README for an auto-updating security badge.

---

## License

MIT — free to use, share, and modify.

---

**Built for the builders vibe-coding in 2026.**  
Automated scanner → [vibesafe.store](https://vibesafe.store)
