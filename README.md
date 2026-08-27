# Rassegna Stampa Inglese — World News Review

A bilingual (English / Italian) daily press review, built as a single self-contained
static web page. It covers geopolitics, economy, technology, politics, society and
medicine, always citing the original source (reputable news outlets), and is designed
to help Italian speakers study English through real news content.

## What's in this repo

- **`index.html`** — the entire site: markup, CSS and JavaScript in one file, with the
  full dataset inlined as a JS constant (`const DATA = {...}`). No build step, no
  dependencies to install. It works when opened directly from disk (`file://`) and
  when served over HTTPS (e.g. GitHub Pages).
- **`data.json`** — a standalone copy of the same dataset (items, sources, places,
  glossary "deep dives", special topics and cross-story connections), kept for
  reference/backup. `index.html` does not fetch this file at runtime — the data is
  inlined directly in the page to avoid any CORS/fetch issues when opened locally.

## Features

- Category filter pills (Geopolitics, Economy, Technology, Politics, Society,
  Medicine) plus a "Daily Update" section with three running story threads
  (US–Iran, Russia–Ukraine, Israel–Palestine).
- A social-feed-style card layout for every news item, with key terms highlighted
  and clickable — clicking a term opens a glossary drawer explaining it in both
  languages.
- Clicking a card opens a detail modal with the full story, a link to the original
  article, and a "verified with 2 independent sources" box with reliability ratings.
- A calendar view to browse stories by day, combinable with the category filter.
- An interactive world map (D3 + world-atlas, loaded from a CDN) showing pins per
  place and dashed lines between related stories. If the map CDN or the Google Font
  CDN is unreachable, the rest of the page (feed, calendar, modal, drawer, timeline)
  still works normally — only the map area shows a translated fallback message.
- A bottom timeline with one lane per country/place.
- Instant EN ⇄ IT language switch (top-right pill) that re-renders the whole page
  without reloading, preserving the active filters.

## Publishing on GitHub Pages

1. Create a new GitHub repository (or use an existing one) and push the contents of
   this folder to it, e.g.:
   ```bash
   git init
   git add index.html data.json README.md
   git commit -m "Rassegna Stampa Inglese — world news review"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```
2. On GitHub, go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to "Deploy from a branch", pick the
   `main` branch and the `/ (root)` folder, then **Save**.
4. After a minute or two your site will be live at:
   `https://<your-username>.github.io/<your-repo>/`
   (it will load `index.html` automatically).

No further configuration, build tooling or server-side code is required — it's a
static file.

## Updating the news

To refresh the review with new items, regenerate `data.json` (same schema) and
re-embed it into `index.html` by replacing the JSON literal assigned to
`const DATA = {...}` near the top of the `<script>` section. Then commit and push —
GitHub Pages redeploys automatically.
