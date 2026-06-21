# PastPresent — marketing & legal site

The promotional website and legal documents for **PastPresent**, the rephotography
app by Berlin Experiences. Built as a zero-dependency static site so it deploys
anywhere instantly and the privacy/support URLs (required by both app stores)
always resolve.

**Live domain:** https://pastpresent.guide

## Pages

| Path | File | Purpose |
|---|---|---|
| `/` | `index.html` | Promo landing — hero, features, screenshot gallery, Free/Pro, download |
| `/privacy` | `privacy.html` | **Privacy policy** — required for App Store & Play listings |
| `/support` | `support.html` | **Support page** — required Apple Support URL; FAQ + contact |
| `/terms` | `terms.html` | Terms of use |

Clean URLs (`/privacy`, not `/privacy.html`) are handled by `vercel.json`.

## Design

Heritage-editorial aesthetic ported from the app's "Editorial Light" theme
(`lib/core/theme/app_colors.dart`): ink `#1A1208`, cream `#F5F0EA`, antique gold
`#8B6914`. Type is **Playfair Display** (display), **DM Sans** (body) and
**Instrument Serif** (italic accents), loaded from Google Fonts. All styling is
in `styles.css`; there is no build step.

## Assets

- `assets/logo*.png` — app mark.
- `assets/promo/` — the four composed **1080×1920 store panels** (store_studio
  exports) shown in the `#gallery`. They already include device frames and
  headlines, so they're displayed full-bleed (no CSS bezel/caption). To add or
  reorder, drop the PNGs here and edit the `.panel` figures in `index.html`.
- `assets/hero/` — five 400×400 then/now Berlin composites shown as a CSS
  crossfade slideshow in the hero (`.hero-stage`). Displayed at their native
  400×400 (never upscaled); on reduced-motion or no-CSS-animation they fall back
  to the first image. To change them, drop 400×400 files here and edit the
  `.hero-slide` list in `index.html` (the `--i` index sets the order).

## Store links

- **Android (live):** https://play.google.com/store/apps/details?id=com.pastpresent.app
- **iOS:** the App Store badge renders as a "Soon" placeholder. When the app is
  on the App Store, in `index.html` replace the two `<span class="badge badge--ios">…</span>`
  blocks (hero + CTA band) with an `<a class="badge" href="https://apps.apple.com/app/idXXXXXXXXX">`
  and delete the `<span class="badge__flag">Soon</span>`.

## Deploy to Vercel + connect pastpresent.guide

1. Push this repo to GitHub (done).
2. In Vercel: **Add New → Project → Import** this GitHub repo. Framework preset:
   **Other** (it's static — no build command, output is the repo root).
3. Deploy. You'll get a `*.vercel.app` URL to verify.
4. **Project → Settings → Domains → Add `pastpresent.guide`** (and `www.pastpresent.guide`).
   Follow Vercel's DNS instructions at your registrar (an `A` record to Vercel,
   or set the nameservers / a `CNAME` for `www`). HTTPS is automatic.

## Deploy via Hostinger (import from GitHub)

This repo also works on Hostinger shared/cloud hosting — it's a static site, so
there's no build step.

1. hPanel → your website → **Advanced → GIT**.
2. **Create a new repository:**
   - Repository: `https://github.com/mrob-dev/pastpresent-web.git`
   - Branch: `main`
   - Install path: leave blank / `public_html` (the repo root becomes the web root).
   *(Public repo, so no deploy key needed. For a private repo, add Hostinger's
   SSH key to GitHub → repo Settings → Deploy keys.)*
3. Click **Create**, then **Deploy** to pull the files into `public_html`.
4. **Auto-deploy on push (optional):** copy the webhook URL Hostinger shows and
   add it in GitHub → repo **Settings → Webhooks**. Pushes then redeploy
   automatically; otherwise click **Deploy** in hPanel after each change.
5. Point `pastpresent.guide` at the hosting (hPanel → Domains) and enable the
   free SSL certificate.

**Important:** `vercel.json` is ignored by Hostinger. The included **`.htaccess`**
provides the equivalent on Apache/LiteSpeed — clean URLs (`/privacy` →
`privacy.html`), the legacy redirects, security headers, HTTPS, and asset
caching. Keep it at the repo root or `/privacy`, `/support`, `/terms` will 404.

## Note — in-app privacy link

The app currently points its in-app "Privacy Policy" link and (likely) the Play
Console privacy URL at `https://www.berlinexperiences.com/pastpresent/privacy`.
Once this site is live, either:

- repoint those to `https://pastpresent.guide/privacy` (in the app's
  `landing_screen.dart` and in the Play Console / App Store Connect privacy
  fields), **or**
- add a redirect from the old path to here.

`vercel.json` already redirects `pastpresent.guide/pastpresent/privacy → /privacy`
in case anything hits the old path on this domain.

## Local preview

Any static server works, e.g.:

```bash
python -m http.server 8000
# then open http://localhost:8000
```

(Note: clean URLs like `/privacy` resolve on Vercel; locally use `/privacy.html`.)
