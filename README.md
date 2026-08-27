# Grey Zone Intel Memo

A static, multi-report archive of Grey Zone Intelligence & Analytics briefings — each a
standalone, sourced analysis styled as an intelligence memo. No build step: every page is
plain HTML/CSS (plus a few lines of vanilla JS), deployed to Vercel as static files.

## How the site is organised

```
/index.html              Homepage. Fetches /reports/manifest.json, finds the most
                          recently published report, and renders it in place. Always
                          shows the latest report — nothing here needs editing when a
                          new report is added.
/archive.html             Lists every published report (title, date, desk, tags, risk/threat
                          badge), newest first, each linking to its own page. Also driven
                          entirely by manifest.json.
/reports/
  manifest.json           The single source of truth for the archive: one JSON object per
                          report (slug, memoId, publishedAt, title, dek, desk, tags, level,
                          levelBadge). index.html and archive.html both read this file.
  <slug>.html             One full, self-contained report page per memo.
/assets/                  Shared brand asset (logo.png) plus each report's own images,
                          referenced with root-absolute paths (/assets/...) so a report
                          renders identically whether it's opened directly or loaded by
                          index.html.
```

Every report page's masthead logo links back to `/` (home, which always shows the latest
report), and its top-right nav links to `/archive.html`.

## Publishing a new report

1. Add the new page at `reports/<slug>.html`. Keep the masthead pattern used by the
   existing reports: logo links to `/`, nav includes a link to `/archive.html`, and any
   image references use root-absolute paths (`/assets/...`), not relative ones — the page
   needs to work both at its own URL and when injected into `/index.html`.
2. Add any report-specific images under `/assets/` (a per-report subfolder is fine, e.g.
   `/assets/<slug>/figure-1.png`).
3. Add one entry to `reports/manifest.json` with an accurate `publishedAt` timestamp (ISO
   8601, UTC). Order in the file doesn't matter — both `index.html` and `archive.html` sort
   by `publishedAt` themselves.
4. Commit and push to `main`. Vercel redeploys automatically. The homepage will pick up the
   new report as "latest" the moment its `publishedAt` is the newest in the manifest — no
   other file needs to change.

## Reports

- **`reports/us-iran-israel-war-2026.html`** — US-Iran vs Iran War 2026 analysis. A
  politically neutral, multi-lens strategic analysis (systems thinking, Cynefin, Wardley-style
  value-chain mapping), presented without naming the underlying frameworks.
  Source data: `us-israel.owm`, `iran.owm` (Wardley Map DSL, paste into
  [onlinewardleymaps.com](https://onlinewardleymaps.com)); `cynefin-initial-scenario.mmd`,
  `cynefin-current-scenario.mmd` (Mermaid `quadrantChart` source); `kumu_elements.csv`,
  `kumu_connections.csv`, `kumu_loops.csv`, `kumu_loop_membership.csv` (Kumu.io-importable
  systems-loop dataset).
- **`reports/ai-infrastructure-water-australia.html`** — AI Infrastructure & Water:
  Australia. A sense-making analysis of the water footprint of Australia's AI/data-centre
  buildout (Systems Thinking, Cynefin, Wardley Mapping). This page keeps its figures
  (Wardley maps, causal loop diagrams) as embedded data-URI images rather than separate
  files under `/assets/` — a deliberate exception, made when this report was first
  published, to avoid transferring binary image files through the tooling used at the time.
  A future edit is welcome to externalize them to `/assets/ai-infrastructure-water-australia/`
  for consistency with the other report, if convenient.

## Status

Dated snapshots. See each report's own Methodological Note / Open Questions and Sources
sections for sourcing and confidence caveats.
