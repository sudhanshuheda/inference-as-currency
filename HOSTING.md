# Hosting

The site is a single static HTML file plus a PDF. There is no build step, no server-side code, no database. Any static host will serve it. The recommendation below assumes you want a polished public URL with HTTPS, a CDN, and the ability to add a custom domain later — and you want it deployed in under ten minutes.

## Recommended: Cloudflare Pages

Free, fast, global CDN, automatic HTTPS, custom domain support, instant rollbacks, no vendor lock-in. Better than GitHub Pages for performance and feature set; better than Vercel/Netlify for AI/inference-related projects because Cloudflare doesn't impose bandwidth-based rate limits at the free tier in any way you'll notice.

### One-time setup

```bash
# 1. Initialise the git repo (run from the project root)
cd /Users/sudhanshu/Documents/Claude/Projects/economics101/site
git init -b main
git add .
git commit -m "Initial publication"

# 2. Create the GitHub repo (gh CLI, install with `brew install gh` if needed)
gh auth login                                  # one-time, opens browser
gh repo create inference-as-currency \
  --public \
  --source=. \
  --description="Inference as Currency: a monetary theory of the AI cycle" \
  --push
```

### Connect to Cloudflare Pages

1. Go to <https://dash.cloudflare.com/?to=/:account/pages>
2. **Create a project** → **Connect to Git** → authorise GitHub → select `inference-as-currency`
3. Build settings:
   - **Framework preset**: None
   - **Build command**: *(leave empty)*
   - **Build output directory**: `/`
4. **Save and Deploy**

Within about 60 seconds you'll have a URL like `inference-as-currency.pages.dev`. Every subsequent `git push` to `main` triggers a redeploy in roughly 20 seconds.

### Custom domain (optional)

Buy a domain (recommend `.com` or your initials + `.io`). Set the nameservers to Cloudflare. In the Pages project settings, add the domain. Cloudflare provisions the certificate automatically. Total time: ~15 minutes including DNS propagation.

## Backup option: GitHub Pages

If you'd rather skip Cloudflare and stay entirely on GitHub:

```bash
cd /Users/sudhanshu/Documents/Claude/Projects/economics101/site
git init -b main
git add .
git commit -m "Initial publication"
gh repo create inference-as-currency --public --source=. --push
# In repo settings → Pages → Source: Deploy from branch → main → / (root) → Save
```

URL will be `USERNAME.github.io/inference-as-currency`. Custom domains supported via `CNAME` file. Performance is good but slower than Cloudflare globally.

## Quickest possible deploy (no git, no domain)

```bash
# 1. Install Wrangler (Cloudflare's CLI)
npm install -g wrangler
wrangler login

# 2. Deploy
cd /Users/sudhanshu/Documents/Claude/Projects/economics101/site
wrangler pages deploy . --project-name=inference-as-currency
```

Site is live at `inference-as-currency.pages.dev` within 30 seconds. No git history required.

## What I'd actually do

If this were my publication I would:
1. Buy a `.xyz` domain matching the title (or a personal subdomain you already own).
2. Use Cloudflare Pages, connected to a GitHub repo, with the custom domain.
3. Add Cloudflare Web Analytics (free, privacy-respecting, no script bloat) by toggling it on in the Pages dashboard.
4. Set the GitHub repo's default README to a `LICENSE.md` (CC-BY-4.0 is good for a working paper) plus a short pointer to the live URL.

Total cost: ~$10/year for the domain. Total setup time: 15 minutes once.

## What not to do

- **Don't put it on Substack.** Substack will mangle the sidenote layout, force its own typography, and serve the PDF as a download attachment instead of a first-class artefact.
- **Don't use Notion as the public surface.** Notion's public pages don't render MathJax correctly and the typography is wrong for long-form.
- **Don't use Medium.** Same reasons as Substack, plus they monetise it.
- **Don't put it on your personal blog if your personal blog uses a heavy CMS.** The point of this site is that it loads in 200ms with no JavaScript framework, no tracking pixels, and no CMS overhead. Hosting it under a CMS defeats the design.
