---
title: "Setting Up a Custom Domain for GitHub Pages — The Full Journey"
date: 2026-03-17
description: "Everything that can go wrong when pointing a custom domain to GitHub Pages — DNS records, TTL, nameservers, Namecheap conflicts, and how to fix them all."
tags: ["github", "dns", "devops", "web", "namecheap", "digitalocean"]
---

# Setting Up a Custom Domain for GitHub Pages

This is a real-world walkthrough — not a clean tutorial. Every problem listed here actually happened.

---

## The Goal

Take a Hugo site deployed on GitHub Pages at:
```
https://username.github.io/my-repo/
```
And serve it at a clean custom domain:
```
https://danialzendehdel.com
```

---

## Step 1 — Add DNS Records

GitHub Pages requires **4 A records** pointing to their servers, plus a CNAME for `www`.

### GitHub Pages IPs (A Records)
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

### DNS Records to Add

| Type  | Host | Value                    | TTL  |
|-------|------|--------------------------|------|
| A     | @    | 185.199.108.153          | 300  |
| A     | @    | 185.199.109.153          | 300  |
| A     | @    | 185.199.110.153          | 300  |
| A     | @    | 185.199.111.153          | 300  |
| CNAME | www  | yourdomain.com.          | 300  |

**TTL note:** Use `300` (5 minutes) while testing — faster propagation. Once confirmed working, bump to `3600` (1 hour) for stability.

---

## Step 2 — Add CNAME File to Your Repo

GitHub Pages needs to know your custom domain. Create `static/CNAME` in your Hugo project:

```
danialzendehdel.com
```

Hugo will copy this to the root of the built site automatically.

---

## Step 3 — Update hugo.toml baseURL

```toml
baseURL = "https://danialzendehdel.com/"
```

This is critical. Without it, GitHub Actions will build the site with the wrong base URL and all CSS/JS paths will be broken.

---

## Step 4 — Configure GitHub Pages

**GitHub → repo → Settings → Pages → Custom domain**

Type your domain, click Save. Wait for the DNS check to go green ✅, then enable **Enforce HTTPS**.

> ⚠️ This step triggers a rebuild. If you skip it, the GitHub Actions workflow will still output the old base URL — even if hugo.toml is correct.

---

## Problems We Hit (And How to Fix Them)

### Problem 1 — NXDOMAIN (Domain Not Found)

Querying `dns.google` returned:
```json
{ "Status": 3 }
```
Status 3 = NXDOMAIN = domain doesn't exist in global DNS.

**Cause:** The domain's nameservers at the registrar weren't pointing to DigitalOcean.

**How to check:**
```bash
curl -s "https://dns.google/resolve?name=yourdomain.com&type=NS"
```

If the `Answer` array shows `dns1.registrar-servers.com` → you're using **Namecheap's nameservers**, not DigitalOcean's.

---

### Problem 2 — DNS Records in DigitalOcean Being Ignored

You added A records in DigitalOcean's Networking → Domains panel, but the domain resolves to a different IP. 

**Cause:** DigitalOcean's DNS panel ≠ your domain's authoritative nameserver. Just adding a domain to DO's panel doesn't make DO authoritative for it — you need to update the nameservers at your registrar.

**Two fixes:**

**Option A — Keep Namecheap DNS, just update records there:**
Log into Namecheap → Advanced DNS → add the 4 A records + CNAME directly in Namecheap.

**Option B — Switch to DigitalOcean DNS:**
Namecheap → Nameservers → Custom DNS:
```
ns1.digitalocean.com
ns2.digitalocean.com
ns3.digitalocean.com
```
Then your DO panel records take effect.

---

### Problem 3 — URL Redirect Overriding A Records

Even after adding correct A records in Namecheap, the domain still resolved to the wrong IP.

**Cause:** Namecheap had a **URL Redirect Record** (`@` → `http://www.yourdomain.com`) left over from the default parking setup. This intercepts traffic before A records are checked.

**Fix:** Delete:
- URL Redirect Record for `@`
- Old CNAME for `www` → `parkingpage.namecheap.com`

Keep only:
- 4 A records pointing to GitHub Pages
- CNAME `www` → `yourdomain.com.`

---

### Problem 4 — CSS/JS Not Loading After Domain Switch

Site loads but renders blank white or completely unstyled.

**Cause:** The HTML contains:
```html
<base href="https://username.github.io/my-repo/">
```
All asset paths (CSS, JS) are relative to this old base URL, so they 404 on the new domain.

**Root cause:** GitHub Actions uses `${{ steps.pages.outputs.base_url }}` for the base URL. This only updates to your custom domain *after* you set it in GitHub Pages Settings. The `baseURL` in `hugo.toml` is overridden by the workflow flag `--baseURL "${{ steps.pages.outputs.base_url }}/"`.

**Fix:** Set the custom domain in GitHub Pages Settings → this triggers a correct rebuild.

---

### Problem 5 — Layout Broken (Mobile Stacked View)

The site loaded but the sidebar was stacked on top of the content instead of appearing on the left.

**Cause:** The Poison/Hyde theme uses `body { overflow: hidden; height: 100vh; }` with a flex row layout. On viewports narrower than `48em` (768px), it switches to a stacked column layout.

This isn't a bug — it's responsive design. The site looks correct at full desktop width.

---

## DNS Propagation Times

| Change Type         | Typical Time    |
|---------------------|-----------------|
| A record update     | 5 min – 1 hour  |
| CNAME update        | 5 min – 1 hour  |
| Nameserver change   | 1 – 24 hours    |
| New domain purchase | 1 – 24 hours    |

**How to check propagation:**
```bash
curl -s "https://dns.google/resolve?name=yourdomain.com&type=A"
```
If `Answer` array has IPs → propagated ✅  
If empty or Status 3 → still waiting ⏳

---

## Final Checklist

- [ ] 4 A records added at your actual registrar (not just DO panel)
- [ ] CNAME `www` added
- [ ] Old parking redirects deleted
- [ ] `static/CNAME` file in Hugo repo
- [ ] `baseURL` updated in `hugo.toml`
- [ ] Custom domain set in GitHub Pages Settings
- [ ] DNS check passed ✅ in GitHub Settings
- [ ] HTTPS enforced ✅

---

## Quick DNS Debug Commands

```bash
# Check A records
curl -s "https://dns.google/resolve?name=yourdomain.com&type=A"

# Check nameservers
curl -s "https://dns.google/resolve?name=yourdomain.com&type=NS"

# Check CNAME
curl -s "https://dns.google/resolve?name=www.yourdomain.com&type=CNAME"

# Check what's actually serving
curl -sI https://yourdomain.com | head -5
```

---

Painful to figure out the first time. Trivial once you know where to look.
