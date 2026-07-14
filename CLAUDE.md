# CLAUDE.md

Guidance for Claude Code working in the `next-nf/next-nf.github.io` repo.

> **Not published.** This file is listed under `exclude:` in `_config.yml`, so
> Jekyll does not copy it into `_site/`. Keep it excluded — it is internal
> notes, not site content.

## What this repo is

The GitHub Pages **landing site** for the `next-nf` project, served at the root
`https://next-nf.github.io/`. Because `next-nf` is a **User** account, this is a
user Pages site: the repo must stay named exactly `next-nf.github.io` and the
site lives at the domain root (no `/repo` path).

## Why it exists (the point of the whole thing)

It is an **owned, indexable property** for search discoverability. `github.com`
pages can't be submitted to Search Console (the org doesn't own that domain), so
this site is the thing we *can* verify and submit a sitemap for. Its jobs:

1. Rank for the project's keywords (3GPP, SMF/PGW, HSS/UDR, PCF/PCRF, CHF, etc.).
2. Link out to every component repo, giving crawlers a trusted path into the org.
3. Be the single external backlink target — the profile README
   (`next-nf/next-nf`) links here, which is what gets the profile page itself
   crawled/indexed.

## How it builds

GitHub Pages' **native Jekyll** builds on every push to `main` — no GitHub
Actions workflow. Two allowlisted plugins do all the SEO work:

- `jekyll-seo-tag` → emits `<title>`, meta description, canonical, Open Graph,
  and (when `google_site_verification` is set) the Search Console verify tag.
  Invoked by `{% seo %}` in `_layouts/default.html`.
- `jekyll-sitemap` → generates `/sitemap.xml` at build time. Do **not** hand-write
  a sitemap file.

## Files

| File | Role |
|---|---|
| `_config.yml` | Site metadata, plugin list, `google_site_verification`, `exclude`. Keep `url: https://next-nf.github.io` with **no trailing slash**. |
| `_layouts/default.html` | The only layout: `{% seo %}` in `<head>` + inline CSS; renders `{{ content }}`. |
| `index.md` | The single landing page (front matter + mission + component table). |
| `robots.txt` | Plain file (no front matter) allowing all + the `Sitemap:` line. |
| `Gemfile` | For optional local builds only; GitHub Pages ignores it. |
| `README.md` | Short repo description. |
| `CLAUDE.md` | This file (excluded from the build). |

## Conventions / guardrails

- **Single page only.** No per-component pages, no theme, no JS, no analytics,
  no custom domain, unless the owner asks. This is deliberate YAGNI.
- **Component list** in `index.md`, in this order, each linking to
  `https://github.com/next-nf/<name>`: `smf`, `udr`, `pcf`, `chf`,
  `support-containers`.
- Content edits → `index.md`; metadata → `_config.yml`.
- Prose stays keyword-rich (that's the SEO surface).

## Validating a change

The origin environment has **no Ruby/Jekyll**, so builds are validated against
the **live site after push** (Pages rebuilds in ~30–60s):

```sh
curl -sS -o /dev/null -w '%{http_code}\n' https://next-nf.github.io/            # 200
curl -sS -o /dev/null -w '%{http_code}\n' https://next-nf.github.io/sitemap.xml # 200
curl -sS https://next-nf.github.io/ | grep -o '<title>[^<]*</title>'            # populated
curl -sS -o /dev/null -w '%{http_code}\n' https://next-nf.github.io/CLAUDE.md   # 404 (excluded)
```

If Ruby *is* available locally: `bundle exec jekyll build` then inspect `_site/`.

## Search Console

- The URL-prefix property `https://next-nf.github.io/` is verified via the
  `google_site_verification` token in `_config.yml` (jekyll-seo-tag emits the
  `<meta>` tag). Submit / re-check `sitemap.xml` from the Sitemaps report.
- Judge the sitemap by the **Sitemaps report**, not the URL Inspection tool —
  URL Inspection errors on XML files ("something went wrong") and on a new site
  the "Couldn't fetch" status is just first-crawl lag, not a block. Googlebot
  gets a clean 200 from the sitemap.

## Wider context

Part of the `next-nf` org discoverability effort. Related, but **not** in this
repo: the `smf` repo is a fork (Google won't index forks until the fork
relationship is severed), and `pcf`/`chf` had issues filed to add READMEs +
`About` metadata so their landing pages become indexable.
