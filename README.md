[![License: MIT](https://img.shields.io/badge/License-MIT-84cc16?style=for-the-badge)](./LICENSE)
**Credits:** [maria-lake.vercel.app](https://maria-lake.vercel.app/)

## Getting Started

```bash
npm install
npm run dev
```

Build for production:

```bash
npm run build
```

Preview the production build locally:

```bash
npm run preview
```

## Template Setup

The main template settings live in:

- [src/config/site.ts](./src/config/site.ts)

Update this file before publishing:

- `name`
- `title`
- `description`
- `email`
- `authorName`
- `authorRole`
- social links

Set your production domain with an environment variable before publishing:

- `SITE_URL=https://your-domain.com`
- or `PUBLIC_SITE_URL=https://your-domain.com`

This keeps canonical URLs, `robots.txt`, and the sitemap aligned without editing source for each environment.

## SEO

The template includes:

- canonical URLs
- meta descriptions
- keyword meta
- Open Graph tags
- Twitter card tags
- sitemap generation
- dynamic `robots.txt`
- JSON-LD structured data defaults
- `noindex` handling for the 404 page

Main SEO files:

- [src/layouts/Layout.astro](./src/layouts/Layout.astro)
- [astro.config.mjs](./astro.config.mjs)
- [src/pages/robots.txt.ts](./src/pages/robots.txt.ts)
- [public/og-image.svg](./public/og-image.svg)

## Cookies and Consent

The theme includes a client-side cookie consent system with:

- a bottom banner for first visit consent
- a preferences modal with essential, analytics, and marketing categories
- saved consent in `localStorage` under `maria-cookie-consent`
- a footer `Cookie Preferences` button for reopening the modal
- a `Cookies` policy page at `/cookies`

The theme also saves the visitor's color theme in `localStorage` under `maria-theme`.

### How consent works

- Essential storage is always active because it remembers theme and consent choices.
- Analytics and marketing are optional categories and default to off until the visitor opts in.
- The consent UI works out of the box even if you have not connected analytics or marketing tools yet.

### Client API

The consent script exposes `window.mariaCookieConsent` in the browser:

```js
window.mariaCookieConsent.getConsent();
window.mariaCookieConsent.hasConsent();
window.mariaCookieConsent.canUse('analytics');
window.mariaCookieConsent.canUse('marketing');
window.mariaCookieConsent.openPreferences();
```

Whenever a visitor updates their preferences, the site dispatches:

```js
window.addEventListener('maria:cookieConsentChanged', (event) => {
  console.log(event.detail);
});
```

### Hooking in analytics or marketing scripts

Only load optional third-party scripts after checking consent. Example:

```html
<script>
  if (window.mariaCookieConsent?.canUse('analytics')) {
    // load your analytics script here
  }

  window.addEventListener('maria:cookieConsentChanged', (event) => {
    if (event.detail.analytics) {
      // load or re-enable analytics here
    }
  });
</script>
```

If you add a new provider, also update:

- [src/pages/cookies.astro](./src/pages/cookies.astro)
- [src/pages/privacy.astro](./src/pages/privacy.astro)
- banner/modal copy in [public/cookie-consent.js](./public/cookie-consent.js)

## Content and Pages

Theme behavior:

- the site respects the visitor's system color scheme by default
- the header includes an icon-only theme toggle for switching between light and dark mode
- the selected theme is saved in `localStorage`

Main pages:

- `/`
- `/about`
- `/resume`
- `/work`
- `/work/nextpoint`
- `/privacy`
- `/cookies`
- `/terms`
- `/404`

At the moment, `Nextpoint` is the only fully built case study page in the theme. The other homepage project cards intentionally point to `/work/nextpoint` as placeholders until you add their own case study pages.

## Images and Assets

Portfolio images live in:

- [src/assets/images](./src/assets/images)

Tool logos live in:

- [src/assets/logos](./src/assets/logos)

Notes:

- Portfolio and case study screenshots use Astro's image pipeline for responsive optimized output.
- Tool logos are self-hosted SVGs.
- `public/` is reserved for files that should be served as-is, such as favicons and the Open Graph image.
- Cookie consent assets live in [public/cookie-consent.js](./public/cookie-consent.js) and [public/cookie-consent.css](./public/cookie-consent.css).

## Deployment

Included config:

- [netlify.toml](./netlify.toml)
- [vercel.json](./vercel.json)

If you only deploy to one platform, delete the other config file before wiring up CI so platform auto-detection stays predictable.

## License

This project is licensed under the [MIT License](./LICENSE).

## Fonts

The site uses **Alte Haas Grotesk** from `public/fonts/`:

- `AlteHaasGroteskRegular.ttf` — mapped to `font-weight: 400–600` (body, nav, buttons, H1 headings)
- `AlteHaasGroteskBold.ttf` — mapped to `font-weight: 700–900` (H2, H3, sub-headings)

The `@font-face` declarations and weight ranges live in `src/layouts/Layout.astro` (look for the two `@font-face` blocks near the top of the `<style>` tag).

### Changing H1 font weight

H1 headings intentionally use the **regular** weight (400). To switch them back to bold, find every `h1 { font-weight: 400; }` rule and change it to `font-weight: 800;` (or any value 700–900). Affected files:

- `src/layouts/Layout.astro` — `.display-title { font-weight: 400 }` (used on index, about, units, nextpoint)
- `src/pages/index.astro` — standalone `h1` override after the `h1, h2, h3` rule
- `src/pages/products/index.astro` — `h1` rule
- `src/pages/products/nextpoint.astro` — standalone `h1` override after the `h1, h2` rule
- `src/pages/products/page/[page].astro` — `h1` rule
- `src/pages/terms.astro`, `cookies.astro`, `privacy.astro`, `404.astro` — each has its own `h1` rule

### Swapping the typeface entirely

1. Drop new TTF/WOFF2 files into `public/fonts/`.
2. Update the two `@font-face` blocks in `src/layouts/Layout.astro` (change `src:` and `font-family:` values).
3. Update the `--font-sans` CSS variable (also in `Layout.astro`) if you renamed the family.
4. Remove or update the `<link rel="preload">` tag in the `<head>` of `Layout.astro` to point at the new file.

## Notes

- Replace the example project copy and images with your own work.
- Set `SITE_URL` or `PUBLIC_SITE_URL` before deploying so SEO URLs do not point to the demo domain.
- The social share image is a template default and can be replaced with your own branded preview.
