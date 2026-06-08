# Customizing this site with Claude

This file is both a **guide for you** and **context for [Claude Code](https://claude.com/claude-code)**.
Open this repo in Claude Code (or the Claude app with this folder attached) and work through the
steps below — most are a single copy-pasteable prompt. Run `npm run dev` in another terminal and
watch the site update at http://localhost:4321 as you go.

> Tip: after each step, ask Claude to run `npm run build` to confirm the site still compiles.

---

## How this site is structured (so Claude gets it right)

- **Content is Markdown** in `src/content/{people,publications,figures,gallery}/`. Schemas are in
  `src/content.config.ts` — Claude should keep frontmatter valid against them.
- **Site identity** (name, PI, contact, nav, social) lives in `src/data/site.ts`.
- **The research taxonomy** (topic/approach tags) is the `areas` enum in `src/content.config.ts`,
  mirrored by labels/groups in `src/lib/content.ts`. The two must stay in sync.
- **Pages** are in `src/pages/`; **components** in `src/components/`. Most pages are data-driven, so
  you rarely edit them directly — you edit content + `site.ts` + `lib/content.ts`.

---

## Step 1 — Site identity

> **Prompt:** “Fill in `src/data/site.ts` for my lab. Lab name: ___. Short name: ___. PI: ___.
> Institution: ___. University: ___. Contact email/phone/address: ___. One-sentence mission: ___.
> Scholar/Twitter/GitHub links: ___.”

## Step 2 — Research topics (the taxonomy)

Decide 3–6 topics and a couple of method/approach tags for your field.

> **Prompt:** “Replace the example research taxonomy with mine. Topics: ___, ___, ___.
> Approaches: ___, ___. Update the `areas` enum in `src/content.config.ts` and the matching
> `AreaSlug` / `AREA_LABELS` / `RESEARCH_GROUPS` / `FILTER_GROUPS` in `src/lib/content.ts`, and the
> homepage `themes` in `src/pages/index.astro`. Keep `review` and `letter` as publication-type tags.”

## Step 3 — People

> **Prompt:** “Create `src/content/people/` entries for my lab from this roster: [paste names,
> roles, groups (Faculty/Researchers/Staff/Students/Affiliates/Alumni), and a sentence of bio
> each]. Use the schema in content.config.ts. I’ll drop headshots into `src/assets/people/` named
> to match the `headshot:` paths.” Then set the PI in `NON_MENTEE_SLUGS` in `src/lib/content.ts`.

Add headshots as ~600px square images in `src/assets/people/`. (Ask Claude how to crop/resize them
with ImageMagick if needed.)

## Step 4 — Publications

Two options:

- **Quick:** “Here’s my CV / Google Scholar — create `src/content/publications/*.md` for each paper
  (one file per paper, frontmatter per the schema). Tag each with my `areas`, set
  `piFirstOrSenior`/`isMenteePaper` appropriately, and set `openAccess` where there’s a PMCID.”
- **Scripted (bulk):** generate `scripts/data/cv-publications.json`
  (`[{title, authors:[{name,mentee,coFirstSenior}], journal, year, doi, pmid, pmcid}]`), set the
  PI surname via `PI_SURNAME`, then `node scripts/migrate-publications.mjs`. It’s additive — re-run
  it when you publish new papers and it adds only the new ones.

Citation counts fill in automatically once you enable the weekly Action (see SETUP.md), or ask
Claude to add `citations:` manually.

## Step 5 — Figures (mind the copyright gate)

Figures render **only** if `rightsConfirmed: true` (default is `false`). Only post figures you have
the right to (open-access/CC, or your own author-reuse rights).

> **Prompt:** “Add a figure: image at `src/assets/figures/___.png`, from paper `<pub-slug>`, caption
> ___, license CC-BY (or publisher-permission). Set rightsConfirmed only if I confirm I have the
> rights.”

## Step 6 — Lab Life photos

> **Prompt:** “Add my photos in `src/assets/gallery/` as gallery entries with captions and dates.”
Photos get a click-to-zoom lightbox automatically.

## Step 7 — Logo & hero (make it yours)

The favicon in `public/` is a neutral placeholder, and `src/components/PipelineHero.astro` is an
**example** custom hero (the original lab’s) that the homepage does *not* use by default.

> **Prompt:** “Design a simple SVG logo for my lab themed around ___, on a [color] tile; output
> `public/favicon.svg` and regenerate `favicon.ico` + the PNG icon set.” and/or
> “Build a custom hero illustration for my field (___) as an inline-SVG Astro component and render
> it on the homepage.”

## Step 8 — Ship it

Follow **[SETUP.md](./SETUP.md)**: push to your default branch, set **Pages → Source → GitHub
Actions**, (optional) wire up your custom domain and the `SERPAPI_KEY` secret + `SCHOLAR_AUTHOR_ID`
variable. After that, every push to the default branch redeploys.

---

## Hit a bug *in the template*? Send it upstream

This site was created from the **[lab-website-template](https://github.com/bchcohenlab/lab-website-template)**
(upstream: `bchcohenlab/lab-website-template`). When a problem or improvement belongs to the template
rather than to your own content, report it back so it's fixed for everyone.

**Claude: watch for template-level problems as you work, and when one comes up, proactively offer to
file it upstream — don't wait to be asked.**

- **Template-level → report upstream:** bugs or rough edges in `src/components/`, `src/pages/`,
  `src/lib/`, the schemas in `content.config.ts`, `scripts/`, `.github/workflows/`, styles, the
  build/deploy, or these docs; missing features; confusing or wrong instructions.
- **Your own content → just fix it here, never report:** anything under `src/content/`,
  `src/data/site.ts`, your images, or your copy.

**Always confirm with the user before submitting** (issues and PRs are public), draft the
title/body for their review, and **never include their lab's content, names, emails, or keys** in the
report. Requires the GitHub CLI authenticated (`gh auth status`); if `gh` is missing, point them to
the repo's **Issues** tab on github.com.

**Default — open an issue** (works for anyone):

```bash
gh issue create --repo bchcohenlab/lab-website-template \
  --title "<short summary>" \
  --body "<what happened · steps to reproduce · expected vs actual · OS + Node version · a suggested fix/diff if you have one>"
```

**If you have a concrete fix — open a pull request** (Claude orchestrates this):

```bash
# 1) fork the template once (separate from this repo), then work inside the fork:
gh repo fork bchcohenlab/lab-website-template --clone --remote
# 2) Claude: in the fork, apply ONLY the template fix on a new branch, commit, push.
# 3) open the PR against upstream:
gh pr create --repo bchcohenlab/lab-website-template \
  --title "<short summary>" --body "<what this fixes and why>"
```

Keep the PR scoped to the template change — no lab-specific content, secrets, or generated `dist/`.

---

## Conventions for Claude

- Keep all frontmatter valid against `src/content.config.ts`; run `npm run build` to verify.
- A person’s “slug” is their Markdown filename without `.md` (used in `NON_MENTEE_SLUGS` and figure
  `paper:` references → publication slugs).
- Don’t set `rightsConfirmed: true` on a figure unless the user confirms they hold the rights.
- Prefer editing content + `site.ts` + `lib/content.ts` over editing the page components.
- Match the existing code style; keep components data-driven.
