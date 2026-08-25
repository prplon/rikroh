# röhr.se — static site

Deploy the **contents of this folder** to the repo root of a GitHub Pages repo.

## Files

```
index.html      ← the whole site (markup + logic)
app.js          ← render runtime
uploads/        ← fonts, images, gif loops
CNAME           ← custom domain
.nojekyll       ← stops Jekyll from eating _-prefixed paths
```

No build step, no npm, no server.

## Setup

1. Push these files to the repo root (not a `site/` subfolder).
2. Settings → Pages → Source: **Deploy from a branch** → `main` / `/ (root)`.
3. Settings → Pages → Custom domain: the value in `CNAME`.
4. Point DNS at GitHub: `A` records to `185.199.108.153`, `.109.153`, `.110.153`, `.111.153` — or a `CNAME` to `<user>.github.io` for a subdomain.
5. Tick **Enforce HTTPS** once the cert issues (can take ~15 min).

## Load path

`index.html` + `app.js` + React, and nothing else. Every image is `loading="lazy"`,
so the hero paints before a single photo is requested. GitHub Pages gzips the HTML
and JS automatically.

Keep the `<head>` intact when editing: the preloads, the `@font-face` block, the
`x-dc { display:none }` rule and the `defer` on `app.js` are what remove the load
waterfall. The hide rule is permanent by design — the runtime renders into its own
`#dc-root`.

## Notes

- The loading screen (RÖHR wordmark filling left-to-right) is plain HTML + inline JS
  at the top of `index.html`: no dependencies, releases on real signals (React ready,
  first painted canvas frames, fonts, above-the-fold images), 8 s safety timeout.
- GIF loops show a WebP still until hovered/tapped, then swap to the animation and
  keep playing.
- Canvas resolution is pixel-budgeted per device (78 k px mobile / 62 k desktop), the
  render loop pauses on hidden tabs, and parallax runs at half rate on mobile.
- Mug break: shard sprites are cut once, untinted, and coloured by a CSS filter on
  their own layer, so they always match the cup's current tint.
