# Jasper website

Three static pages and one stylesheet. No build step, no framework, no
dependencies — open `index.html` in a browser and what you see is what ships.

```
index.html     landing page
privacy.html   → App Store Connect "Privacy Policy URL"  (required)
support.html   → App Store Connect "Support URL"         (required)
style.css      shared styles, using the app's palette
CNAME          your custom domain, read by GitHub Pages
```

## Before deploying

Contact email is set to `uyjosephn@gmail.com` (6 places across `privacy.html`
and `support.html`). To change it later:

```sh
grep -rln uyjosephn@gmail.com . | xargs sed -i '' 's/uyjosephn@gmail.com/NEW/g'
```

`CNAME` still needs your domain — see below.

## Deploying to GitHub Pages

```sh
git init && git add -A && git commit -m "Jasper website"
gh repo create jasper-webpage --public --source=. --push
```

Then in the repo: **Settings → Pages → Source: Deploy from a branch → `main` /
(root)**. It goes live at `https://<user>.github.io/jasper-webpage/` within a
minute or two.

## Pointing a GoDaddy domain at it

1. Put the domain in `CNAME` (apex, no `https://`, no trailing slash):
   ```
   jasperbudget.app
   ```
2. In GoDaddy → **My Products → DNS → Manage Zones**, set:

   | Type | Name | Value |
   |---|---|---|
   | A | @ | 185.199.108.153 |
   | A | @ | 185.199.109.153 |
   | A | @ | 185.199.110.153 |
   | A | @ | 185.199.111.153 |
   | CNAME | www | `<user>.github.io` |

   Delete GoDaddy's default parking A record and any conflicting CNAME on `@`.
3. Back in **Settings → Pages**, enter the domain under *Custom domain*, wait
   for the DNS check, then tick **Enforce HTTPS**.

DNS usually propagates in minutes but can take up to 48 hours. HTTPS only
becomes available once GitHub has issued the certificate, which happens after
the DNS check passes.

## After it's live

Paste the URLs into **App Store Connect → Jasper → App Information**:

- Privacy Policy URL → `https://yourdomain/privacy.html`
- Support URL → `https://yourdomain/support.html`

Both must stay reachable. Apple re-checks them, and a dead privacy policy link
can get a published app removed.
