# Developing Claude Instruct

Everything the README leaves out, so the front page can stay a front page.

## Run it locally

Open `index.html` in a browser. No build step, no dependencies, no server.

## Deploy

Hosted at **[claudeinstruct.surge.sh](https://claudeinstruct.surge.sh)**.

```bash
npm i -g surge     # once
surge ./           # CNAME sets the target domain, .surgeignore keeps docs/ out
```

Files that ship with the site:

| File | Purpose |
| --- | --- |
| `index.html` | The whole app |
| `og.png` | 1200×630 social share card (`og:image` / `twitter:image`) |
| `robots.txt` | Allows everything, points at the sitemap |
| `sitemap.xml` | One URL; update `lastmod` on meaningful changes |
| `CNAME` | Deploy target for Surge |
| `.surgeignore` | Keeps `docs/`, the README and git metadata out of the deploy |

> If you ever switch to GitHub Pages, delete or change `CNAME` first — Pages reads the same file and would try to serve the site from the Surge hostname.

## Extend the block library

Everything lives in two arrays near the top of the first `<script>` block in `index.html`:

```js
const SECTIONS = [ { id, title, mode: "prose" | "list", p: profiles, hint } ];
const ITEMS    = [ { s: sectionId, p: profiles | "*", t: "the line of instruction text" } ];
```

`p` is either `"*"` (applies to every profile) or an array of profile ids. Add a line to `ITEMS`, reload, done.

## Attribution line

Generated files end with a separator and one credit line:

```
---
Generated with Claude Instruct — claudeinstruct.surge.sh
```

A checkbox under the Generate button turns it off, and the choice is remembered in `localStorage` under `cpi.attrib`. Default is on.

## SEO notes

- Canonical, Open Graph and Twitter tags point at `https://claudeinstruct.surge.sh/`. If the domain changes, update them in `<head>`, plus `sitemap.xml`, `robots.txt`, `CNAME`, and the `@id`/`url` fields in the JSON-LD.
- `application/ld+json` at the end of `<body>` carries **WebApplication** and **FAQPage** schema. The FAQ answers there mirror the visible FAQ — edit one, edit both, or the rich result gets suppressed.
- Roughly 590 words render without JavaScript (the explainer, the anatomy list and the FAQ), so the page is indexable by crawlers that do not execute scripts.

## localStorage keys

| Key | What it holds |
| --- | --- |
| `cpi.state.v1` | The whole builder state: profile, blocks, items, checked lines |
| `cpi.theme` | `light` or `dark` |
| `cpi.attrib` | `on` / `off` for the credit line |
| `cpi.nudge` | `seen` / `off` for the coffee prompt after a generate |
