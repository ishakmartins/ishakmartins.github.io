[![License: GPLv3](https://img.shields.io/badge/License-GPLv3-blue.svg?style=for-the-badge)](./LICENSE)

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

## Site Configuration

Main site settings are in [src/config/site.ts](./src/config/site.ts). Configure:

- `name`
- `title`
- `description`
- `email`
- `authorName`
- `authorRole`
- social links

Set your production domain with an environment variable:

- `SITE_URL=https://your-domain.com`
- or `PUBLIC_SITE_URL=https://your-domain.com`

This keeps canonical URLs, `robots.txt`, and the sitemap consistent across environments.

## SEO

The site includes built-in SEO features:

- canonical URLs
- meta descriptions
- Open Graph and Twitter card tags
- sitemap generation
- dynamic `robots.txt`
- JSON-LD structured data
- `noindex` handling for error pages

Key SEO files:

- [src/layouts/Layout.astro](./src/layouts/Layout.astro)
- [astro.config.mjs](./astro.config.mjs)
- [src/pages/robots.txt.ts](./src/pages/robots.txt.ts)
- [public/og-image.svg](./public/og-image.svg)

## Cookie Consent & Preferences

The site includes a client-side cookie consent system with:

- first-visit consent banner
- preferences modal (essential, analytics, marketing)
- persistent consent settings
- footer preferences button
- dedicated `/cookies` policy page

Theme preference is saved separately in localStorage.

### Consent API

Access consent status via `window.cookieConsent`:

```js
window.cookieConsent.getConsent();
window.cookieConsent.hasConsent();
window.cookieConsent.canUse('analytics');
window.cookieConsent.canUse('marketing');
window.cookieConsent.openPreferences();
```

Listen for consent changes:

```js
window.addEventListener('cookieConsentChanged', (event) => {
  console.log(event.detail);
});
```

### Integrating Analytics

Only load third-party scripts after checking consent:

```html
<script>
  if (window.cookieConsent?.canUse('analytics')) {
    // load analytics here
  }

  window.addEventListener('cookieConsentChanged', (event) => {
    if (event.detail.analytics) {
      // enable or reload analytics
    }
  });
</script>
```

When adding new consent categories, update:

- [src/pages/cookies.astro](./src/pages/cookies.astro)
- [src/pages/privacy.astro](./src/pages/privacy.astro)
- [public/cookie-consent.js](./public/cookie-consent.js)

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

This project is licensed under the [GNU General Public License v3.0](./LICENSE).

**Credits:** Built with design inspiration from [maria-lake.vercel.app](https://maria-lake.vercel.app/)

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
