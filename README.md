# Rassegna Stampa Inglese — World News Review

A bilingual (English / Italian) daily press review, built as a single self-contained
static web page. It covers geopolitics, economy, technology, politics, society,
medicine, and Italian/international crime & local news ("cronaca"), always citing the
original source (reputable news outlets), and is designed to help Italian speakers
study English through real news content.

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
  Medicine, Italian Crime & Local News, International Crime & Local News) — each
  category is always shown as a small colored dot next to its name, never as a
  filled background.
- A "📌 Daily Update" section, placed right after the hero, offering 8 running
  story dossiers (US–Iran, Russia–Ukraine, Israel–Palestine, Italian Politics,
  International Politics, International Violence Against Women, AI, International
  Investment Markets) as switchable tabs. AI and Investment Markets pull together
  items tagged with that dossier's topic regardless of category (e.g. the AI
  dossier surfaces Technology items tagged `ai`) — the mechanism is generic and
  works the same way for every dossier.
- Both the Daily Update dossier tabs/cards and the general News Feed (all
  categories or filtered to one) show only as many cards as fit in one row of the
  grid by default, with a "View all" button to expand to the complete list — this
  keeps the home page short. The row size is computed live from the actual
  rendered layout, not hard-coded.
- Clicking a category pill in the top menu jumps straight to that category's
  filtered feed — it does not pass through the Daily Update section. Clicking the
  "📌 Daily Update" pill scrolls to the Daily Update section instead.
- A social-feed-style card layout for every news item, with key terms highlighted
  and clickable — clicking a term opens a glossary drawer explaining it in both
  languages.
- Clicking a card opens a detail modal with the full story, a link to the original
  article, and a "verified with 2 independent sources" box with reliability ratings.
- A calendar view to browse stories by day, combinable with the category filter.
- An interactive world map (D3 + world-atlas, loaded from a CDN) showing pins per
  place and dashed lines between related stories. The femicide-pattern connection
  (Colleferro ↔ Ottawa) renders in a dedicated magenta (`#D6336C`), distinct from
  the 8 category colors, so it stands out on the map. If the map CDN or the Google
  Font CDN is unreachable, the rest of the page (feed, Daily Update, calendar,
  modal, drawer, timeline) still works normally — only the map area shows a
  translated fallback message.
- A bottom timeline with one lane per country/place (unchanged by the redesign —
  it always shows everything, no "View all" gating).
- Instant EN ⇄ IT language switch (top-right pill) that re-renders the whole page
  without reloading, preserving the active filters and any expanded/collapsed
  "View all" sections.

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
