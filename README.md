# SOS Ennui — deploying to GitHub Pages

This folder is a complete static PWA (`index.html` + `manifest.json` +
`service-worker.js` + `/icons`) plus a GitHub Actions workflow
(`.github/workflows/deploy-pages.yml`) that deploys it automatically.
Nothing here ever needs to run on your own computer — GitHub hosts it
permanently once it's pushed.

## One-time setup (~5 minutes)

**1. Create the repo on GitHub**
Go to https://github.com/new
- Name it whatever you like, e.g. `sos-ennui`
- Set it to **Public** (required for free GitHub Pages, unless you're on a
  paid plan)
- Do **not** check "Add a README" — this folder already has one

**2. Push this folder to it**
Open a terminal in this `sos-ennui-app` folder and run:
```bash
git init
git add .
git commit -m "Initial commit: SOS Ennui PWA"
git branch -M main
git remote add origin https://github.com/<your-username>/sos-ennui.git
git push -u origin main
```
(Replace `<your-username>` and the repo name if you picked a different one.)

**3. Turn on Pages, pointed at Actions**
In the repo on GitHub: **Settings → Pages → Build and deployment → Source**
→ select **"GitHub Actions"** (not "Deploy from a branch").

That's it. The workflow you just pushed will run automatically (check the
**Actions** tab — it takes about 30–60 seconds). Once it's green, go back to
**Settings → Pages** and you'll see your live URL:

```
https://<your-username>.github.io/sos-ennui/
```

## Updating it later
Any time you edit `index.html` (or anything else in the folder), just:
```bash
git add .
git commit -m "update activities"
git push
```
The Actions workflow re-runs automatically and the live site updates within
about a minute. No manual redeploy step, ever.

## Installing it on your phone
Once it's live at that URL:
- **iPhone**: open the link in Safari → Share → "Add to Home Screen"
- **Android**: open the link in Chrome → menu (⋮) → "Install app"

It launches full-screen with its own icon — no browser chrome.

## Custom domain (optional)
If you want e.g. `mtl.yourdomain.com` instead of the github.io URL:
1. Add a `CNAME` file to this folder containing just the domain name
2. Add a `CNAME` DNS record at your domain registrar pointing that subdomain
   to `<your-username>.github.io`
3. Re-push — GitHub Pages picks it up automatically

## Swap in a real icon
`/icons/icon-192.png`, `icon-512.png`, and `icon-512-maskable.png` are
placeholder sparks matching the site's palette. Replace them with your own
artwork (same filenames/sizes), commit, and push — no code changes needed.

---

## Next step: auto-refreshing event data

Right now the event list is a hardcoded `ACTIVITIES` array inside
`index.html`, so it'll go stale as 2026 dates pass. The natural next step,
using this same repo:

1. Move `ACTIVITIES` out into a `data.json` file; have `index.html` fetch it
   instead of using an inline array.
2. Add a second GitHub Actions workflow on a `schedule:` trigger (e.g.
   weekly) that re-fetches the event sources and rewrites `data.json`, then
   commits it — running entirely on GitHub's infrastructure, still free,
   still nothing on your machine.
3. The existing `deploy-pages.yml` workflow re-deploys automatically the
   moment that commit lands.

Happy to build that workflow out when you're ready.
