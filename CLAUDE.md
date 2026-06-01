# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static website for the **DevOps Micro Internship with Agentic AI** program, live at `https://dmi.pravinmishra.com`. No build step, no framework — plain HTML, CSS, and JavaScript.

## Deployment

**Automatic:** Pushing to `main` triggers GitHub Actions (`.github/workflows/deploy.yml`), which syncs to S3 and invalidates CloudFront.

**Manual full deploy:**
```bash
aws s3 sync . s3://dmi.pravinmishra.com \
  --exclude ".git/*" --exclude ".github/*" --exclude "*.md" --exclude ".DS_Store"

aws cloudfront create-invalidation --distribution-id E2N4UG9MOMV660 --paths "/*"
```

**Deploy a single file:**
```bash
aws s3 cp data/champion.json s3://dmi.pravinmishra.com/data/champion.json
aws cloudfront create-invalidation --distribution-id E2N4UG9MOMV660 --paths "/data/champion.json"
```

CloudFront invalidation is required after every upload — without it, changes won't be visible for up to 24 hours.

## Architecture

Each HTML page fetches its own data via an inline `<script>` block at the bottom using `fetch()`:

- `leaderboard.html` → fetches `data/leaderboard.csv`, parses it client-side, renders table with group filter + name search
- `champion.html` → fetches `data/champion.json`, renders champion cards per group, browsable by week
- `graduates.html` → fetches `data/cohort1.json` and `data/cohort2.json`, renders graduate cards with cohort tabs
- `index.html` → no data fetch; static content only (curriculum, mentor bio, teaching team, CTA)

`styles.css` is a single shared stylesheet for all pages using CSS custom properties (`--accent`, `--bg-card`, `--border`, etc.).

`app.js` is a shared script (just mobile nav toggle) included in every page.

## Data file formats

**`data/leaderboard.csv`** — columns: `Name, Group, Total`
- Group is a number (1–6); Total is cumulative score
- Scoring: 10 pts attendance · 20 pts assignment · 10 pts LinkedIn post · 30 pts blog

**`data/champion.json`** — top-level fields: `cohort`, `current_week`, `weeks[]`
- Each week has `week` number and `champions[]` array with one entry per group
- Champion fields: `group`, `name`, `score`, `comment`, `linkedin`, `photo` (photo is path or empty string)

**`data/cohort1.json` / `data/cohort2.json`** — arrays of graduate objects
- Fields: `name`, `country`, `role`, `linkedin`, `photo`, `group`
- If `photo` is non-empty it should be a path under `images/`

## Adding a new cohort's graduates

1. Populate `data/cohort3.json` with graduate objects
2. In `graduates.html`, add `<button class="cohort-tab" data-cohort="3">Cohort 3</button>` and wire up the fetch in the inline script
3. Deploy both files

## Preview locally

Open any `.html` file directly in a browser. Data files are fetched with relative paths so they work from `file://` as long as the browser allows local file fetches (Chrome may require a local server):

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```
