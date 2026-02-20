# PRD: Get Ad Reels — Website Deployment
**For:** Claude Code  
**Project:** Deploy getadreels.com landing page via GitHub + Vercel  
**Complexity:** Low — static site, no backend  

---

## Context

You are deploying a single-page marketing website for Get Ad Reels (getadreels.com). The site is a static HTML file — no framework, no build step, no database. The user will provide the HTML file (`index.html`). Your job is to get it live on Vercel via a GitHub repository.

---

## Deliverables

1. A GitHub repository containing the site files
2. The site deployed and live on Vercel, connected to that repository
3. Deployment configured so that every future push to `main` auto-deploys

---

## File Structure

The final repository should look like this:

```
getadreels/
├── index.html          ← the landing page (user will provide this)
├── vercel.json         ← Vercel config (you create this)
└── README.md           ← brief project readme (you create this)
```

That's it. No `/src`, no `/public`, no framework scaffolding.

---

## Step-by-Step Instructions

### Step 1 — Create the GitHub repository

1. Create a new **public** GitHub repository named `getadreels`
2. Do not initialise with a README (you'll add one manually)
3. Clone it locally or initialise a local git repo and set the remote

### Step 2 — Add the files

Place the user-provided `index.html` in the repository root.

Create `vercel.json` in the root with the following content:

```json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

This ensures clean routing — any path hits the index. For a single-page site this is the correct config.

Create `README.md`:

```markdown
# Get Ad Reels

Landing page for [getadreels.com](https://getadreels.com).

Static HTML site deployed via Vercel.

To update the site: edit `index.html` and push to `main`. Vercel auto-deploys.
```

### Step 3 — Commit and push

```bash
git add .
git commit -m "Initial deploy: Get Ad Reels landing page"
git push origin main
```

### Step 4 — Deploy to Vercel

1. Go to [vercel.com](https://vercel.com) and log in (or create an account if needed — prompt the user to do this if no account exists)
2. Click **Add New → Project**
3. Import the `getadreels` GitHub repository
4. Framework preset: select **Other** (not Next.js, not Vite — this is plain HTML)
5. Build settings:
   - Build command: leave **empty**
   - Output directory: leave **empty** (or set to `.` if Vercel requires a value)
   - Install command: leave **empty**
6. Click **Deploy**

Vercel will detect the static HTML and deploy it. This takes under a minute.

### Step 5 — Connect the custom domain

After deployment:

1. In the Vercel project dashboard, go to **Settings → Domains**
2. Add: `getadreels.com` and `www.getadreels.com`
3. Vercel will provide DNS records — either:
   - **A record** pointing to Vercel's IP, or
   - **CNAME** pointing to `cname.vercel-dns.com`
4. Log into the Hostinger DNS dashboard for getadreels.com and add those records
5. DNS propagation typically takes 5–30 minutes, up to 48 hours maximum

**Note for Claude Code:** You cannot do the DNS step programmatically — surface the exact DNS values from Vercel to the user and instruct them to add these in Hostinger. Confirm once propagation is complete.

---

## What You Do NOT Need to Do

- No backend, server, or API
- No database or environment variables
- No contact form backend (the CTA links use `mailto:` — this is intentional)
- No analytics setup (user can add later)
- No framework installation (plain HTML, zero dependencies)
- No CI/CD beyond Vercel's built-in auto-deploy on push

---

## Success Criteria

The task is complete when:

- [ ] `https://getadreels.com` loads the landing page
- [ ] `https://www.getadreels.com` redirects or resolves to the same page
- [ ] Pushing a new commit to `main` on GitHub triggers an automatic Vercel redeploy
- [ ] The page loads without console errors
- [ ] All `mailto:daniel@getadreels.com` links work correctly

---

## If Something Goes Wrong

**Vercel shows a 404:** Check that `index.html` is in the root of the repo, not in a subdirectory. Also verify `vercel.json` is present and valid JSON.

**Domain not resolving:** DNS propagation may still be in progress. Wait up to 1 hour and check again using `dig getadreels.com` or [dnschecker.org](https://dnschecker.org).

**Vercel asking for a framework:** Select "Other" and leave all build fields empty. It will serve the static HTML directly.

**Build failing:** There is no build step. If Vercel runs one, it's because a framework was accidentally selected. Reset the project settings to "Other" with no build command.

---

## Notes

- The domain `getadreels.com` was purchased via Hostinger. DNS is managed there.
- Email `daniel@getadreels.com` is also set up via Hostinger — no action needed for email, just don't change the MX records when editing DNS.
- The HTML file uses Google Fonts loaded via CDN — no local asset hosting required.

---

## Changelog

### 2026-02-20 — Content Updates
- **Delivery time:** Updated from 48 hours to 72 hours across all sections (hero stat, ticker, how-it-works, pricing)
- **Hero CTA:** Removed "See a Sample" button, kept only "How it works" link
- **Sample section:** Added clickable mailto link to `daniel@getadreels.com` in the sample note text
