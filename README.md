# DMI — DevOps Micro Internship with Agentic AI

The official website for **DevOps Micro Internship with Agentic AI** by [Pravin Mishra](https://pravinmishra.com) & [The CloudAdvisory](https://thecloudadvisory.com).

Live at: **https://dmi.pravinmishra.com**

---

## About the Program

A hands-on, mentor-led **14-week program** combining DevOps and Agentic AI. Students deploy real apps, build CI/CD pipelines, automate infrastructure, and ship a capstone project powered by AI.

**Cohort 3 starts June 6, 2026** — applications open via the website.

---

## Pages

| Page | File | Description |
|---|---|---|
| Home | `index.html` | Program overview, curriculum timeline, mentor bio, CTA |
| Champion | `champion.html` | Weekly champion per group, browsable by week |
| Leaderboard | `leaderboard.html` | Live rankings with group filter and name search |
| Graduates | `graduates.html` | Graduate showcase by cohort |

---

## Project Structure

```
.
├── index.html          # Landing page
├── champion.html       # Champion of the Week
├── leaderboard.html    # Student leaderboard
├── graduates.html      # Graduate directory
├── styles.css          # Shared stylesheet
├── app.js              # Shared JS (mobile nav)
├── images/
│   └── pravin-mishra.jpg
├── data/
│   ├── leaderboard.csv  # Student scores (Name, Group, Total)
│   ├── champion.json    # Weekly champion data per group
│   ├── cohort1.json     # Cohort 1 graduates
│   └── cohort2.json     # Cohort 2 graduates
└── HOW-TO-UPDATE.md    # Deployment & content update guide
```

---

## Data Files

### `data/leaderboard.csv`
CSV with columns: `Name, Group, Total`
- Group is a number (1–6)
- Total is the overall score
- Scores: 10 pts attendance · 20 pts assignment · 10 pts LinkedIn post · 30 pts blog

### `data/champion.json`
JSON with `current_week`, `cohort`, and a `weeks` array. Each week lists one champion per group with name, score, mentor comment, LinkedIn, and optional photo.

### `data/cohort1.json` / `data/cohort2.json`
JSON arrays of graduate objects: `name`, `country`, `role`, `linkedin`, `photo`, `group`.

---

## Deployment

This site is hosted on **AWS S3 + CloudFront** as a static site.

See [HOW-TO-UPDATE.md](HOW-TO-UPDATE.md) for:
- First-time deployment (S3, CloudFront, Route 53)
- Day-to-day content updates with cache invalidation commands
- A quick reference table for each data file

**Quick deploy (all files):**
```bash
aws s3 sync . s3://dmi.pravinmishra.com \
  --exclude ".claude/*" \
  --exclude "*.md" \
  --exclude ".DS_Store"

aws cloudfront create-invalidation \
  --distribution-id YOUR_DISTRIBUTION_ID \
  --paths "/*"
```

---

## Tech Stack

- Plain HTML, CSS, JavaScript — no build step, no framework
- Google Fonts (Inter)
- AWS S3 static hosting + CloudFront CDN

---

## Contact

hello@thecloudadvisory.com · Helsinki, Finland

Community: [discord.pravinmishra.com](https://discord.pravinmishra.com/)

test
