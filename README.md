# Lab website template

A modern, content-driven **research-lab website** built with **[Astro](https://astro.build) +
[Tailwind CSS v4](https://tailwindcss.com)**, designed to deploy free to **GitHub Pages**. It's
made to be customized *with [Claude](https://claude.com/claude-code)* — point Claude at your CV and
roster and it fills in the content for you.

> This is a starter template. It ships with placeholder content so it builds and deploys out of the
> box. Replace the placeholders with your lab's details (see **[SETUP.md](./SETUP.md)** and
> **[CLAUDE.md](./CLAUDE.md)**).

## What you get

- **Pages:** Home, People (with per-person profiles), Research (grouped by topic/approach),
  Publications (filter by role/topic + search + sort), Figures (copyright-gated), Lab Life
  (photo gallery with lightbox), Contact (map + links), and a 404.
- **Content as Markdown** via Astro content collections (`src/content/`) with typed schemas —
  no database, no CMS required (a CMS can be layered on later with no schema change).
- **A research-topic taxonomy** that auto-groups the Research page and drives Publications filters.
- **Image optimization** built in (Astro `<Image>` → responsive WebP).
- **Automatic citation counts** — an optional weekly GitHub Action refreshes per-paper Google
  Scholar counts via SerpAPI.
- **Copyright-safe figures** — figures are hidden unless you explicitly set `rightsConfirmed: true`.
- **One-click deploy** to GitHub Pages via GitHub Actions (works with a custom domain or a
  `username.github.io/repo` project page).

## Quick start

```bash
npm install
npm run dev        # http://localhost:4321
```

Then customize: edit `src/data/site.ts`, swap the content in `src/content/`, and replace the
placeholder favicon in `public/`. The fastest path is to **ask Claude** — see
**[CLAUDE.md](./CLAUDE.md)** for a guided, copy-pasteable workflow.

To go live, follow **[SETUP.md](./SETUP.md)** (GitHub Pages + optional custom domain + the
citation-refresh secret).

## Commands

| Command           | Action                                              |
| :---------------- | :-------------------------------------------------- |
| `npm install`     | Install dependencies                                |
| `npm run dev`     | Start the local dev server at `localhost:4321`      |
| `npm run build`   | Build the production site to `./dist/`              |
| `npm run preview` | Preview the production build locally                |
| `npx astro check` | Type-check `.astro` files and content frontmatter   |

## Project layout

```
src/
  content/        people · publications · figures · gallery  (your content, as Markdown)
  content.config.ts   collection schemas + the research-topic vocabulary
  data/site.ts    site identity, contact, nav  ← EDIT THIS FIRST
  lib/content.ts  taxonomy labels/groups + query helpers
  components/     layout + UI (incl. PipelineHero.astro — an example custom hero)
  pages/          one file per route
public/           favicon set, CNAME, robots.txt
scripts/          optional helpers (citation refresh, link check, CV importer)
.github/workflows/ deploy.yml (Pages) + refresh-citations.yml (weekly Scholar counts)
```

## ⚠️ A note on figure copyright

The `figures` collection is **gated**: a figure renders only if its frontmatter says
`rightsConfirmed: true` (the default is `false`, fail-closed). Journal figures are usually
copyrighted — only mark `rightsConfirmed: true` for figures you have the right to post (open-access
/ CC-licensed, or your own author-reuse rights). Don't post other people's figures.

## Credit & license

Created by the [Cohen Laboratory of Translational Neuroimaging](https://bchcohenlab.com) and shared
as a template. MIT licensed — see [LICENSE](./LICENSE). The placeholder content and images are
filler; replace them with your own.
