# {{PROJECT_NAME}} — Project Blueprint

> **Status:** {{DRAFT | LIVE | MAINTENANCE}}  
> **URL:** {{url}}  
> **Last Updated:** {{date}}

---

## 1. Executive Summary

{{2-3 sentences: what it is, who it's for, why it exists.}}

**Tagline:** {{tagline}}

---

## 2. Architecture

```
{{ASCII architecture diagram showing layers:
  CDN → Hosting → Backend → Database → Payments
  or mobile/web → API → services}}
```

### Data Flow

```
{{How a user moves through the product:
  Visit → Auth gate → Free tier → Upgrade → Payment → Full access}}
```

---

## 3. Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| **Hosting** | {{provider}} | {{purpose}} |
| **Domain** | {{registrar}} | {{dns setup}} |
| **Frontend** | {{stack}} | {{rendering}} |
| **Auth** | {{provider}} | {{auth method}} |
| **Payments** | {{processor}} | {{products}} |
| **Analytics** | {{tool}} | {{tracking}} |
| **Email** | {{provider}} | {{transactional}} |
| **Content** | {{format}} | {{pages / posts}} |
| **Schema** | {{JSON-LD types}} | {{SEO / AEO}} |

---

## 4. File Tree

```
{{root}}/
├── index.html              ← {{notes}}
├── {{page}}.html           ← {{notes}}
├── {{page2}}.html          ← {{notes}}
├── data/
│   └── {{data_file}}.js    ← {{content}}
└── {{subdir}}/
    ├── {{file}}.html       ← {{notes}}
    └── {{file2}}.html      ← {{notes}}
```

---

## 5. Build Roadmap

### Phase 1: Foundation
| Task | Detail | Files |
|------|--------|-------|
| {{task}} | {{description}} | {{files}} |

### Phase 2: Features
| Task | Detail |
|------|--------|
| {{task}} | {{description}} |

### Phase 3: Launch
| Task | Detail |
|------|--------|
| {{task}} | {{description}} |

---

## 6. Feature Inventory

| Feature | Detail | Status |
|---------|--------|--------|
| {{feature}} | {{description}} | {{status}} |

---

## 7. UI Layout & Visual Design

### Brand Tokens

| Token | Value | Usage |
|-------|-------|-------|
| Background | `{{color}}` | Page bg |
| Primary | `{{color}}` | CTAs, accents |
| Secondary | `{{color}}` | Alt accents |
| Text | `{{color}}` | Body |
| Muted | `{{color}}` | Secondary text |
| Font heading | `{{font}}` | Headlines |
| Font mono | `{{font}}` | Code, prices |
| Radius | `{{px}}` | Cards |

### Page Layouts

| Page | Layout | Key Elements |
|------|--------|-------------|
| Landing | {{type}} | {{elements}} |
| App | {{type}} | {{elements}} |
| Blog | {{type}} | {{elements}} |

---

## 8. Pricing Strategy

| Tier | Price | Target | Features |
|------|-------|--------|----------|
| {{tier}} | {{price}} | {{target}} | {{features}} |

### Competitive Positioning
| Competitor | Price | {{feature}} | {{feature}} |
|-----------|-------|:-----------:|:-----------:|
| {{name}} | {{price}} | {{value}} | {{value}} |

**Strategy:** {{notes on freemium, anchoring, billing}}

---

## 9. SEO & Content Strategy

### Technical SEO
| Element | Status | Detail |
|---------|--------|--------|
| Sitemap.xml | {{status}} | {{urls}} |
| Robots.txt | {{status}} | {{directives}} |
| Canonical URLs | {{status}} | {{notes}} |
| Meta descriptions | {{status}} | {{count}} |
| Semantic HTML | {{status}} | {{notes}} |
| Mobile responsive | {{status}} | {{notes}} |
| SSL / HTTPS | {{status}} | {{notes}} |

### Content Marketing
| Asset | Volume | Target Keywords |
|-------|--------|-----------------|
| {{asset}} | {{count}} | {{keywords}} |

### AEO (AI Answer Engine Optimization)
- {{schema types used}}
- {{direct answer formatting}}
- {{internal linking strategy}}

### Search Console Status
| Engine | Property | Verification | Sitemap |
|--------|----------|:------------:|:--------:|
| Google | {{url}} | {{method}} | {{status}} |
| Bing | {{url}} | {{method}} | {{status}} |

---

## 10. Domain & DNS

| Setting | Value | Provider |
|---------|-------|---------|
| **Domain** | {{domain}} | {{registrar}} |
| **A Record** | `@` → `{{ip}}` | {{provider}} |
| **CNAME** | `www` → `{{target}}` | {{provider}} |
| **Nameservers** | {{servers}} | {{provider}} |

---

## 11. Launch Checklist

### Done
- [x] {{item}}

### Pending
- [ ] {{item}}

---

## 12. Revenue Model

```
{{User acquisition funnel}}
    │ {{step}}
    │ {{step}}
    ▼
{{Conversion target}}
    │ {{detail}}
    ▼
{{Revenue projection}}
    └── {{scenario 1}}
    └── {{scenario 2}}
```

---

## 13. Lessons Learned

### What Worked
1. {{lesson}}
2. {{lesson}}

### What to Avoid
1. {{pitfall}}
2. {{pitfall}}

---

*Template — last updated {{date}}*
