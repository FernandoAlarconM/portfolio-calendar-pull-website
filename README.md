# Portfolio Calendar Pull — Website (OAuth consent screen pages)

Static pages required by the Google Cloud OAuth consent screen (Branding) for project
`portfolio-calendar-pull`.

## Why this exists

Google's branding verification rejected the app with three issues:

1. Home page `https://github.com/FernandoAlarconM` is not registered to you.
2. The privacy policy page does not have sufficient content.
3. The app name "Portfolio Calendar Pull" does not match the app name on the home page.

Root cause: `github.com` is not a domain you can verify as yours. These pages move the three
URLs onto `fernandoalarconm.github.io`, which **is** verifiable in Google Search Console
(`github.io` is on the Public Suffix List, so your subdomain counts as your own site).

## Naming

Folder, GitHub repo and public URL path are all `portfolio-calendar-pull-website`.
The Google Cloud **project id** stays `portfolio-calendar-pull` — that is fixed in Google Cloud
and cannot be renamed.

## Files

| File | Becomes |
|---|---|
| `docs/index.html` | Application home page |
| `docs/privacy.html` | Application privacy policy link |
| `docs/terms.html` | Application terms of service link |
| `docs/style.css` | Shared styling (light + dark) |
| `docs/.nojekyll` | Stops GitHub Pages running Jekyll |
| `docs/logo.png` | Logo shown on the website |
| `brand/logo-120.png` | The 120x120 app logo to upload to the consent screen |
| `brand/make_logo.py` | Generates every logo file from scratch (see `brand/README.md`) |

The `<h1>` on `index.html` is exactly `Portfolio Calendar Pull` — this is what fixes issue 3.

## Local check

    cd ~/Documents/portfolio-calendar-pull-website/docs
    python3 -m http.server 8765

Open http://localhost:8765/ and confirm all three pages load and cross-link.

## Deploy to GitHub Pages

    cd ~/Documents/portfolio-calendar-pull-website
    git init
    git add .
    git commit -m "OAuth consent screen pages for Portfolio Calendar Pull"
    gh repo create portfolio-calendar-pull-website --public --source=. --push

Then: repo → Settings → Pages → Source = `Deploy from a branch`,
Branch = `main`, Folder = `/docs` → Save.

Live URLs (wait ~2 minutes for the first build):

- Home:    https://fernandoalarconm.github.io/portfolio-calendar-pull-website/
- Privacy: https://fernandoalarconm.github.io/portfolio-calendar-pull-website/privacy.html
- Terms:   https://fernandoalarconm.github.io/portfolio-calendar-pull-website/terms.html

## Verify domain ownership (required — do not skip)

1. Go to https://search.google.com/search-console signed in as **fernando.alarcon.agent@gmail.com**
   (the same account that owns the Cloud project — note the console URL uses `authuser=1`).
2. Add a **URL prefix** property: `https://fernandoalarconm.github.io/`
3. Choose the **HTML file** verification method, download `googleXXXXXXX.html`,
   drop it into `docs/`, commit, push, then click Verify.

## Update the OAuth consent screen

In https://console.cloud.google.com/auth/branding?project=portfolio-calendar-pull :

- Application home page → `https://fernandoalarconm.github.io/portfolio-calendar-pull-website/`
- Application privacy policy link → `.../privacy.html`
- Application terms of service link → `.../terms.html`
- App logo → upload `brand/logo-120.png`
- Authorized domains → add `fernandoalarconm.github.io`
- Save, then open the "Branding verification issues" panel and select
  **"I have fixed the issues"** → Proceed.

## Open item — scope minimisation

The pages document all four requested scopes:

- `https://www.googleapis.com/auth/calendar.events.readonly`
- `https://www.googleapis.com/auth/calendar.readonly`
- `https://www.googleapis.com/auth/calendar` (full read/write)
- `openid` + `https://www.googleapis.com/auth/userinfo.email`

`calendar` (full) already covers both readonly scopes. Google rejects apps that request more
than the minimum necessary. If the app only reads events, drop to
`calendar.events.readonly` + `openid` + `userinfo.email` and remove the extra rows from
`index.html` and `privacy.html` before submitting.

## Known blocker — github.io is rejected as an Authorized domain

Status as of 5 September 2026.

Branding verification cleared two of the three original issues once these pages went live:
the privacy policy content and the app-name mismatch both passed. One issue persists:

> The website of your home page URL
> "https://fernandoalarconm.github.io/portfolio-calendar-pull-website/" is not registered to you.

This is **not** a misconfiguration. All of the following were confirmed correct before a fresh
verification attempt was submitted, and it still failed:

| Check | State |
|---|---|
| Cloud project sole Owner | `fernando.alarcon.agent@gmail.com` |
| Search Console property | `https://fernandoalarconm.github.io/` — URL prefix, **Verified**, Owner |
| Search Console owner account | `fernando.alarcon.agent@gmail.com` — same as project Owner |
| Verification file | `https://fernandoalarconm.github.io/google6c1fb6337c4d4395.html` → HTTP 200 |
| Authorized domain | `fernandoalarconm.github.io` |
| Home / privacy / terms | HTTP 200 |

Read the error wording carefully: *"not registered to you"*, not "ownership could not be
verified". `github.io` is registered to GitHub. Search Console proves you control the content
at that subdomain; it does not make you the domain registrant. This check appears to want the
latter, so free hosting subdomains (`github.io`, `pages.dev`, `netlify.app`, `vercel.app`) do
not clear it.

### Options

1. **Custom domain (~$12/yr).** Register a domain, point it at GitHub Pages via CNAME — hosting
   and HTTPS stay free. Verify it in Search Console as a **Domain property** using a DNS TXT
   record. You are then the registrant, which is what the check wants. Permanent fix.
2. **Request human review.** In the issues panel choose *"I believe the issues found are
   incorrect"* and cite the Search Console verification. Free, and worth trying first.
3. **Stay in Testing mode.** No verification and no branding check at all; add yourself as a
   test user. Free. Cost: with sensitive Calendar scopes the refresh token expires every 7 days,
   so you re-authorise weekly. Fine for a personal tool, not for distribution.
