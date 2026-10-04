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
| `site/robots.txt` | Crawler rules |
| `wrangler.jsonc` | Cloudflare Workers config (static assets from `site/`) |

## Controls

Sound is on by default and starts on the first tap, click or key press (browsers block audio
before that); the speaker button in the playback bar mutes it. Space plays and pauses, the arrow keys skip 5 seconds, and the chapter buttons and timeline jump
around the film. It respects `prefers-reduced-motion` by starting paused.

## Custom domain and link previews

The link-preview tags use relative image paths so the site works on any domain. Once you have a
final URL, add `<link rel="canonical">` and an `og:url` tag, and make the `og:image` and
`twitter:image` URLs absolute, so every chat app and social network picks up the preview image.

Made by JP Madrigal — https://www.jpmadrigal.dev
