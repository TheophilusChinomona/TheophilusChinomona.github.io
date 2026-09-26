# EdgeTech Public Site (Edgetech-pub-site)

Static mirror of https://www.edgetechsolutions.co.za (WordPress/Astra/Elementor),
captured 2026-09-26 as a faithful baseline for client-specific improvements.

Zero build: plain HTML/CSS/JS served by any static file server. Same convention
as `edgetech-web-app` (Dokploy + Nginx).

## Pages

| File                | Route          |
| ------------------- | -------------- |
| `index.html`        | `/`            |
| `about.html`        | `/about/`      |
| `services.html`     | `/services/`   |
| `our-process.html`  | `/our-process/`|
| `contact.html`      | `/contact/`    |
| `privacy-policy.html` | `/privacy-policy/` |

`deploy/nginx.conf` maps `/about/` etc. to the flat files via `try_files`.

## Structure

- `css/<page>.bundle.css` — every page's stylesheets concatenated into one
  render-blocking file (was ~20 `<link>` tags per page); `url()` paths
  rewritten relative to the bundle.
- `overrides.css` — audit-fix styles (retagged heading replicas, `!important`
  pinned); also appended to each bundle.
- `wp-content/…`, `wp-includes/…` — theme/plugin assets (Astra, Elementor,
  Font Awesome, Swiper, WPForms, jQuery) and uploaded images, paths preserved
  from the original site.
- `s/…` + `google-fonts.css` — Google Fonts (Poppins, Roboto, Roboto Slab,
  Raleway) self-hosted; the remote `<link>` tags were replaced.
- `og-image.jpg`, `robots.txt`, `sitemap.xml` — SEO basics.
- Elementor frontend JS + Swiper are kept and `defer`red, so carousels/sticky
  behaviour work.

## Applied fixes (2026-09-26 SEO + a11y audit)

- **SEO**: canonical per page, keyword titles + tailored meta descriptions,
  `og:image`/`twitter:image`, `LocalBusiness` JSON-LD on all pages,
  `robots.txt` + `sitemap.xml`.
- **A11y**: body/heading grey `#808285` → `#666a6e` and link blue
  `#4169e1` → `#3557c9` (WCAG AA on white and `#f3f3f3`); heading outline
  repaired (h6 eyebrows → `p`, skipped levels retagged) with pixel-identical
  `.etfx-*` style replicas; form inputs given `aria-label`s.
- **Perf**: ~20 stylesheets bundled to 1 per page; heavy scripts `defer`red;
  below-fold images `loading="lazy"` + `decoding="async"`.

## What was removed vs the live site

- Cloudflare email-protection: obfuscated emails decoded to plain text
  (`info@edgetechsolutions.co.za`), decoder script removed.
- WP live-chat widget (needs its own server; dead weight in a static mirror).
- Astra starter-template preview script, WP emoji loader, WP REST/feeds/XML-RPC.

## Known gaps (faithful to the source, broken by nature of a static mirror)

- The WPForms contact form still posts to WordPress `admin-ajax.php` — it
  renders but **will not send email** until re-pointed at a real endpoint.
- Live site's own bugs are reproduced as-is: the contact page shows phone
  `+27 12 345 6789` (likely a placeholder) while services page shows
  `+27 65 076 2860`; some counters render as "0 +".
- Inherited a11y quirks kept for fidelity: 10px eyebrow text at tablet
  widths, uppercase eyebrow styling, cramped Elementor paddings.

## Local run

```bash
python3 -m http.server 8811
# http://127.0.0.1:8811/index.html
```

## Deploy

`deploy/Dockerfile` + `deploy/nginx.conf` build an Nginx image (same pattern
as `edgetech-web-app`'s Dokploy Application). Wire a Dokploy app to this repo
when ready.
