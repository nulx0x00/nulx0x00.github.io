# NulX 0x00 — Personal Cybersecurity Portfolio

A terminal-themed static portfolio and blog built with **Astro**, deployable to Vercel or GitHub Pages in minutes.

## Features

- `/` — Hero landing page with quick stats
- `/journey` — Platform ranks, progress bars, and milestone timeline
- `/bounties` — Bug bounty ledger with severity tags and running total
- `/blog` — Write-up index + full Markdown blog posts with syntax highlighting
- `/projects` — GitHub REST API integration (auto-fetches your public repos at build time)

## Tech stack

- **Astro 4** — Static site generator, Markdown-native
- **JetBrains Mono + Syne** — Typography
- **No JavaScript frameworks** — Pure Astro components, NulX client-side JS overhead
- **GitHub API** — Pulls repo data at build time (no API calls from the browser)

## Quick start

```bash
git clone https://github.com/yourusername/zer0isdead
cd zer0isdead
npm install
cp .env.example .env     # add your GitHub username
npm run dev              # localhost:4321
```

## Configuration

Edit `.env`:

```env
GITHUB_USERNAME=yourusername
GITHUB_TOKEN=ghp_xxxxxxxxxxxxx   # optional — higher rate limit
```

Edit `src/pages/index.astro` to update your stats, tags, and bio.
Edit `src/pages/bounties.astro` to add real bounty entries.
Edit `src/pages/journey.astro` to update platform stats and timeline.

## Writing blog posts

Create a new Markdown file in `src/pages/blog/`:

```bash
touch src/pages/blog/my-new-writeup.md
```

Add frontmatter:

```md
---
layout: '../../layouts/BlogPost.astro'
title: "My Write-up Title"
date: "May 1, 2025"
tag: "HTB WRITE-UP"    # or: VULN RESEARCH / CRYPTOGRAPHY / CTF
readTime: "10 min"
description: "SEO description for the post."
---

Your content here in Markdown. Code blocks get syntax highlighting automatically.
```

Push to `main` and your site redeploys automatically.

## Deployment

### Vercel (recommended)

```bash
npm install -g vercel
vercel login
vercel --prod
```

Or connect your GitHub repo in the Vercel dashboard — it auto-deploys on every push.
Add `GITHUB_USERNAME` and `GITHUB_TOKEN` in **Vercel → Project → Settings → Environment Variables**.

### GitHub Pages

1. Add `base: '/repo-name'` to `astro.config.mjs` if deploying to a project page.
2. Push to GitHub and enable **Pages → GitHub Actions** in repo settings.
3. Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run build
        env:
          GITHUB_USERNAME: ${{ secrets.GITHUB_USERNAME }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist/
      - uses: actions/deploy-pages@v4
```

## Customisation checklist

- [ ] Update name/bio in `src/pages/index.astro`
- [ ] Set `GITHUB_USERNAME` in `.env` and Vercel env vars
- [ ] Replace placeholder bounties in `src/pages/bounties.astro`
- [ ] Replace timeline milestones in `src/pages/journey.astro`
- [ ] Update social links in `src/layouts/Base.astro`
- [ ] Update `site` URL in `astro.config.mjs`
- [ ] Write your first blog post in `src/pages/blog/`
