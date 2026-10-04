# Into Sagittarius A*

A real-time, ray-traced fall into Sagittarius A*, the black hole at the center of the Milky Way.
It runs entirely in the browser as a single WebGL fragment shader: no libraries, no build step.

**Act one** follows the real math of a non-spinning (Schwarzschild) black hole: the lensed
accretion disk, the last stable orbit, the photon sphere, the event horizon, spaghettification and
the singularity. Instrument readouts show distance, proper time left and tidal stretch in real units
for a 4.3-million-solar-mass black hole.

**Act two** is clearly labeled speculation: a loop-quantum-gravity bounce into a white hole, a new
universe, and the ring singularity of a spinning black hole.

## Deploy to Cloudflare Workers

The site is plain static files in `site/`. `wrangler.jsonc` tells Cloudflare to serve that folder as
Worker static assets, so there is no Worker script to write.

**From your machine**

```bash
npm install
npx wrangler login
npm run deploy
```

Wrangler prints the live URL (`https://sagittarius-a-visualization.<your-subdomain>.workers.dev`).
Add a custom domain under the Worker's **Settings → Domains & Routes** in the Cloudflare dashboard.

**Automatic deploys from GitHub (Workers Builds)**

In the Cloudflare dashboard: **Workers & Pages → Create → Import a repository**, pick this repo, and use:

| Setting | Value |
| --- | --- |
| Project name | `sagittarius-a-visualization` (must match `"name"` in `wrangler.jsonc`) |
| Build command | *(leave empty)* |
| Deploy command | `npx wrangler deploy` |

Every push to `main` then redeploys the site.

## Run it locally

```bash
npm install
npm run dev
```

Or serve the folder with any static server, for example `cd site && python3 -m http.server 8080`.

## Files

| File | What it is |
| --- | --- |
| `site/index.html` | The whole film: markup, styles, the ray-tracing shader and playback controls |
| `site/score.mp3` | Original ambient score (2:30, 128 kbps), synthesized for the film and kept in sync with playback |
| `site/og-image.jpg` | 1200×630 link-preview image rendered from the film |
| `site/favicon.svg`, `site/favicon-32.png`, `site/apple-touch-icon.png` | Site icons |
| `site/404.html` | Not-found page |
| `site/robots.txt`, `site/sitemap.xml` | Crawler rules and sitemap |
| `site/site.webmanifest` | Name, colors and icons for "Add to Home Screen" |
| `wrangler.jsonc` | Cloudflare Workers config (static assets from `site/`) |

## Controls

The film opens on a start screen: **Begin the fall** starts the film and the score together
(browsers only allow sound after a click), and **Watch without sound** starts it muted. The
speaker button in the playback bar mutes and unmutes. Space plays and pauses, the arrow keys skip 5 seconds, and the chapter buttons and timeline jump
around the film. It respects `prefers-reduced-motion` by starting paused.

## SEO

The site lives at https://blackhole.jpmadrigal.dev/. `index.html` has the title, description,
canonical URL, Open Graph and Twitter card tags with absolute image URLs, and schema.org JSON-LD
(website, author, and the film as a learning resource about Sagittarius A*). A visually hidden
outline of all 12 chapters gives search engines and screen readers the film's full text.
`sitemap.xml`, `robots.txt` and `site.webmanifest` are in `site/`. If the domain changes, update
the URLs in those files and in the `<head>` of `index.html`.

Made by JP Madrigal — https://www.jpmadrigal.dev
