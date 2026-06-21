# PastPresent — marketing & legal site

The promotional website and legal documents for **PastPresent**, the rephotography
app by Berlin Experiences. Built as a zero-dependency static site so it deploys
anywhere instantly and the privacy/support URLs (required by both app stores)
always resolve.

**Live domain:** https://pastpresent.app

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

## Deploy to Vercel + connect pastpresent.app

1. Push this repo to GitHub (done).
2. In Vercel: **Add New → Project → Import** this GitHub repo. Framework preset:
   **Other** (it's static — no build command, output is the repo root).
3. Deploy. You'll get a `*.vercel.app` URL to verify.
4. **Project → Settings → Domains → Add `pastpresent.app`** (and `www.pastpresent.app`).
   Follow Vercel's DNS instructions at your registrar (an `A` record to Vercel,
   or set the nameservers / a `CNAME` for `www`). HTTPS is automatic.

## Note — in-app privacy link

The app currently points its in-app "Privacy Policy" link and (likely) the Play
Console privacy URL at `https://www.berlinexperiences.com/pastpresent/privacy`.
Once this site is live, either:

- repoint those to `https://pastpresent.app/privacy` (in the app's
  `landing_screen.dart` and in the Play Console / App Store Connect privacy
  fields), **or**
- add a redirect from the old path to here.

`vercel.json` already redirects `pastpresent.app/pastpresent/privacy → /privacy`
in case anything hits the old path on this domain.

## Local preview

Any static server works, e.g.:

```bash
python -m http.server 8000
# then open http://localhost:8000
```

(Note: clean URLs like `/privacy` resolve on Vercel; locally use `/privacy.html`.)
