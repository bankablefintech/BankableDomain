# Partner Microsites — Deployment Guide

Password-protected microsites for EAPP partners, deployed via Netlify Pro.

## Architecture

Each partner gets their own private GitHub repo + Netlify site:

```
Private GitHub repo  →  Netlify site  →  unique-name.netlify.app  (password protected)
```

No connection to bankablefintech.com. No public links. Each site is fully isolated.

---

## How to Create a New Partner Microsite

### Step 1: Create a Private GitHub Repo

1. Go to https://github.com/new
2. Name it: `client-microsite-[partner-name]` (e.g., `client-microsite-uecu`)
3. Set visibility to **Private**
4. Do NOT initialize with README (we'll push files)
5. Click **Create repository**

### Step 2: Set Up the Repo Locally

```bash
# Clone the empty repo
git clone https://github.com/bankablefintech/client-microsite-[partner-name].git
cd client-microsite-[partner-name]

# Copy the template config files
cp /path/to/BankableDomain/microsites/template/netlify.toml .
cp /path/to/BankableDomain/microsites/template/_headers .
cp /path/to/BankableDomain/microsites/template/_redirects .
cp /path/to/BankableDomain/microsites/template/.gitignore .
```

### Step 3: Add Partner HTML

Copy your pre-built HTML file into the repo as `index.html`:

```bash
cp /path/to/partner-report.html ./index.html
```

If the HTML references external assets (images, CSS, JS), place them in the same directory or in subfolders.

### Step 4: Push to GitHub

```bash
git add .
git commit -m "Initial partner microsite"
git push -u origin main
```

### Step 5: Connect to Netlify

1. Go to https://app.netlify.com
2. Click **Add new site** → **Import an existing project**
3. Select **GitHub**
4. Find and select your `client-microsite-[partner-name]` repo
5. Build settings:
   - **Build command**: (leave blank — no build needed)
   - **Publish directory**: `/`
6. Click **Deploy site**

### Step 6: Enable Password Protection

1. In Netlify, go to your new site
2. **Site configuration** → **Access & security** → **Visitor access**
3. Under **Password protection**, click **Set password**
4. Enter a unique password for this partner
5. Click **Save**

### Step 7: (Optional) Set a Custom Site Name

1. In Netlify, go to **Site configuration** → **Site details** → **Change site name**
2. Set a clean name like `bankable-uecu` → becomes `bankable-uecu.netlify.app`

### Step 8: Share with Partner

Send the partner:
- **URL**: `https://[site-name].netlify.app`
- **Password**: the password you set in Step 6

---

## Template Files

The `/microsites/template/` folder contains:

| File | Purpose |
|------|---------|
| `netlify.toml` | Netlify build config + security headers |
| `_headers` | HTTP security headers (noindex, no caching, etc.) |
| `_redirects` | SPA redirect rule |
| `.gitignore` | Ignores OS/editor temp files |

### What the headers do:
- **X-Robots-Tag: noindex, nofollow** — prevents search engines from indexing
- **X-Frame-Options: DENY** — prevents embedding in iframes
- **Cache-Control: no-store** — prevents browsers from caching (partners always see latest)
- **Referrer-Policy: no-referrer** — prevents URL leakage in referrer headers

---

## Updating a Partner's Site

1. Edit the HTML file in the partner's repo
2. Commit and push to `main`
3. Netlify auto-deploys within ~30 seconds

---

## Changing a Partner's Password

1. Go to the partner's site in Netlify
2. **Site configuration** → **Access & security** → **Visitor access**
3. Update the password
4. Notify the partner

---

## Removing a Partner's Site

1. In Netlify: **Site configuration** → **General** → **Delete this site**
2. On GitHub: **Settings** → **Delete this repository**
