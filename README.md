# 🛡️ Vibe-Coded App Security Checklist

> **Free, community-driven security guide for apps built with AI coding tools.**
> Lovable · Bolt · Cursor · Replit · v0 · Claude Code · Codex · Windsurf

---

## The Problem

AI coding tools are amazing at shipping fast. Security is not their priority.

| Stat | Source |
|------|--------|
| **63%** of vibe-coded apps have critical/high vulnerabilities | Medium scan of 62 Lovable apps |
| **91.5%** have at least one vulnerability | VibeSafe research |
| **52%** of Supabase-backed Lovable apps have publicly readable database tables | Vibe App Scanner |
| **95%** are missing X-Frame-Options (clickjacking protection) | This repo's scan data |
| **94%** are missing Content-Security-Policy (XSS prevention) | This repo's scan data |
| **380,000** vibe-coded apps are exposed online | VibeSafe |

> A real example: **Quittr** hit $1M in revenue with their Firebase database publicly readable. All 39,000 users' data exposed. They found out from a security researcher.

---

## Quick Start

1. **Read the checklist** → [`CHECKLIST.md`](CHECKLIST.md) (5-minute read)
2. **Run the automated scan** → [VibeSafe](https://vibesafe.store) does all 16 checks automatically ($49 one-time, pre+post+shield)
3. **Fix the issues** → Each finding includes the exact fix

Or skip straight to scanning:

```bash
# Run a free manual check first:
curl -sI https://yourapp.com | grep -iE "Strict-Transport-Security|Content-Security-Policy|X-Frame-Options"
# 3/6 missing? You need a full scan → https://vibesafe.store
```

---

## What's Here

| File | What It Covers |
|------|----------------|
| [`CHECKLIST.md`](CHECKLIST.md) | The full 16-point security checklist — pre-launch and post-launch |
| [`BENCHMARK.md`](BENCHMARK.md) | Real scan data from live Lovable apps (so you know what's actually out there) |
| [`FIXES.md`](FIXES.md) | Copy-paste fixes for each issue |

---

## How to Contribute

Found a vulnerability pattern I missed? Open a PR. This is community-driven.

---

## License

MIT — free to use, share, and modify.

---

**Built for the 8,000+ builders vibe-coding in 2026.**  
Automated scanner → [vibesafe.store](https://vibesafe.store)
