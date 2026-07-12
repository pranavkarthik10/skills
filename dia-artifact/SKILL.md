---
name: dia-artifact
description: Generate polished, editorial-style standalone HTML Reports (memo, brief, recap, analysis, profile, comparison, project update, strategy note, explainer, or written synthesis) using the Dia report-kit design system. Use when the user wants a durable, shareable written deliverable they will read top-to-bottom — not slides, dashboards, apps, or messages to paste elsewhere. Self-contained HTML with bundled style.css, app.js, and fonts.
---

# Dia Artifact — Report Generator

Produces a clean, vertical, prose-driven Report as a self-contained HTML artifact. **You write only `index.html` content** using the provided class names. Styling, fonts, date formatting, paper color, grain, settings panel, and fade-in come from the bundled assets in this skill directory.

## Skill bundle (copy these into every artifact)

When building an artifact, copy these files from this skill directory into the output folder alongside `index.html`:

```
<artifact-name>/
├── index.html       ← you write this
├── style.css        ← copy from skill
├── app.js           ← copy from skill
└── fonts/
    ├── Exposure-400.woff2
    ├── Exposure-500.woff2
    ├── Exposure-550.woff2
    ├── Exposure-550-Italic.woff2
    └── Exposure-600.woff2
```

**Do not rely on server-side injection.** Outside Dia, nothing auto-adds CSS, JS, fonts, or metadata. Every `index.html` must explicitly link the assets and include the `report-data` script (see below).

## When to use
- **Use for:** memos, briefs, recaps, summaries, analyses, profiles, comparisons, project updates, strategy notes, explainers, guides, histories, multi-part written synthesis.
- **Do NOT use for:** slide decks, dashboards, trackers, apps, websites, games, diagrams, interactive timelines/matrices, or prose meant to be pasted into email/Slack/chat.

## Workflow

1. Gather the material (research/sources) needed to make the Report accurate.
2. Create the artifact folder and **copy `style.css`, `app.js`, and `fonts/`** from this skill directory.
3. Write a complete `index.html` following the structure below.
4. **Dia:** call `upload_artifact` with the artifact folder root.
5. **Other agents:** deliver the folder as-is (user opens `index.html` in a browser or serves the folder locally).
6. A Report is not done until files are written AND delivered. Never stop after an outline or prose summary.
7. If sources are sparse, still produce a short **caveated** Report (state what you searched and what couldn't be verified) — don't collapse back to chat.

## Required `<head>`

Every report must include these tags explicitly:

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Report Title</title>
<link rel="stylesheet" href="style.css">
<script id="report-data" type="application/json">{"date":"2026-07-12T01:08:51Z","colorIndex":3,"fontIndex":0,"chartStyleIndex":0,"storageKey":"my-report-slug"}</script>
<script src="app.js" defer></script>
```

### `report-data` fields

`app.js` reads this inline JSON block. **You must write it** — no server injects it outside Dia.

| Field | Type | Purpose |
|-------|------|---------|
| `date` | ISO 8601 string | Baked report date shown in header (not live clock) |
| `colorIndex` | 0–7 | Initial paper color (see palette below) |
| `fontIndex` | 0–2 | Initial headline font (0=Exposure, 1=Arial, 2=SF Pro) |
| `chartStyleIndex` | 0–2 | Initial chart style (0=pattern, 1=color, 2=inked) |
| `storageKey` | string (optional) | Unique slug for settings persistence; isolates color/font prefs per report when opened as local files |

**Paper colors** (`colorIndex`): 0=ivory `#FAF9F5`, 1=canary, 2=rose, 3=powder blue, 4=sage, 5=orchid, 6=salmon, 7=fog.

**Randomize seeds for each new report.** Pick a random `colorIndex` (0–7) every time — do not default to 0. Optionally vary `fontIndex` and `chartStyleIndex` too. Dia randomizes these server-side; outside Dia you must set them explicitly in `report-data`.

Set `storageKey` to a stable slug derived from the report topic (e.g. `"best-local-models-july-2026"`). Without it, `app.js` falls back to `document.title`, which also isolates reports — but an explicit slug is safer if titles collide. Users can override color/font via the settings panel (persisted per `storageKey` in localStorage).

### Charts

Add Chart.js in `<head>` (must load before inline chart scripts in `<body>`):

```html
<!-- Dia -->
<script src="https://artifactcdn.diabrowser.engineering/ajax/libs/Chart.js/chart.umd.js"></script>

<!-- Portable fallback -->
<script src="https://cdn.jsdelivr.net/npm/chart.js@4/dist/chart.umd.min.js"></script>
```

## Design constraints
- Use ONLY the provided class names. The CSS owns all colors and fonts.
- No custom colors, gradients, shadows, rounded corners, accent colors, `@font-face`, or `font-weight: bold`/`700`.
- Only include components your data supports: no metrics strip without real numbers, no charts without real data, no Slack quotes without real Slack messages.
- Chart colors: `rgba(0,0,0,N)` (N=0.06 fills, 0.4–0.65 borders). No `borderRadius`. `pointRadius: 0` for lines. Set `responsive: true`, `maintainAspectRatio: false`, and an explicit canvas `height`.

## Page structure
The `<!-- report-kit -->` comment MUST be the first thing inside `<body>`, or styling won't apply.

```html
<!-- report-kit -->
<div class="report"><div class="report-wrap">

  <header class="report-header">
    <div class="report-header-left"><span class="report-from">Report</span></div>
    <span class="report-date" id="reportDate"></span>
  </header>

  <h1 class="report-headline">Title<br>Goes Here</h1>

  <div class="report-intro">
    <p><strong>BLUF takeaway.</strong> 1–3 sentences of context.</p>
  </div>

  <!-- optional metrics-strip: only with real numbers -->

  <hr class="report-rule">

  <main class="report-body">
    <!-- stacked full-width sections -->
  </main>

  <!-- footer is auto-injected by app.js — do not add it -->

</div></div>

<!-- Settings panel (app.js populates this) — include as-is -->
<div class="settings-wrap">
  <div class="settings-panel" id="settingsPanel">
    <div class="settings-header">Customize Report</div>
    <div class="settings-swatches" id="settingsSwatches"></div>
    <div class="settings-fonts" id="settingsFonts"></div>
  </div>
  <button class="settings-btn" id="settingsBtn" title="Customize">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
      <circle cx="12" cy="12" r="3"/><path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1-2.83 2.83l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-4 0v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83-2.83l.06-.06A1.65 1.65 0 0 0 4.68 15a1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1 0-4h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 2.83-2.83l.06.06A1.65 1.65 0 0 0 9 4.68a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 4 0v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 2.83l-.06.06A1.65 1.65 0 0 0 19.4 9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 0 4h-.09a1.65 1.65 0 0 0-1.51 1z"/>
    </svg>
  </button>
</div>
```

## Component classes

**Section**
```html
<section class="report-section">
  <div class="section-header"><h2 class="section-heading">I. Section Title</h2></div>
  <div class="section-items"><!-- report-item elements --></div>
</section>
```

**Report item** (label left, body right)
```html
<article class="report-item">
  <div class="item-label">
    <h3 class="item-title">1. Item title</h3>
    <span class="item-badge">On Track</span>
  </div>
  <div class="item-body">
    <p><strong>Key finding.</strong> Supporting context.</p>
    <ul class="item-bullets"><li>Detail point.</li></ul>
  </div>
</article>
```

**Metrics strip** (2–4 metrics; `span = 12 / count`)
```html
<div class="metrics-strip">
  <div class="metrics-strip-border"></div>
  <div class="metric" style="grid-column: span 4">
    <div class="metric-value">34%</div>
    <div class="metric-label">Adoption</div>
    <div class="metric-note">vs. 25% target</div>
  </div>
</div>
```

**Data table**
```html
<table class="data-table">
  <thead><tr><th></th><th>Col</th></tr></thead>
  <tbody><tr><td>Label</td><td>Value</td></tr></tbody>
</table>
```

**Featured blockquote** (between sections, use sparingly)
```html
<div class="report-quote-break">
  <hr class="report-image-break__rule" aria-hidden="true">
  <blockquote class="report-blockquote-break">
    <p>One or two short lines that deserve a louder voice.</p>
    <cite>Optional attribution</cite>
  </blockquote>
  <hr class="report-image-break__rule" aria-hidden="true">
</div>
```
For non-featured quotes use a plain `<blockquote>` with optional `<cite>`.

**Chart item**
```html
<article class="report-item report-item--chart-right">
  <div class="item-label">
    <h3 class="item-title">1. Adoption trend</h3>
    <p>What the chart shows.</p>
  </div>
  <div class="item-body">
    <div class="chart-wrap"><canvas id="chart-1" style="height:250px"></canvas></div>
    <p class="fig-caption"><span class="fig-ref">Fig. 1</span> — Caption</p>
  </div>
</article>
```
Canvas height: horizontal bars = labels × 28px (min 200); doughnut/pie = 280px; line/area = 250px.

**Image item**: `report-item--image-right` + `image-wrap` in body, caption in label. Full-width break: `report-image-break` with `report-image-break__img` and rules.

**Citation** (include the tooltip span; omit `href` if no URL)
```html
<a class="cite-ref" href="https://example.com" target="_blank">[1]<span class="cite-tooltip">Source name</span></a>
```
Do not build a footnotes/references section.

## Writing voice
- **BLUF:** lead with the bottom line in `<strong>`.
- **Direct:** no hedging. **Concrete:** numbers over adjectives. **Terse:** bullets are single sentences.
- Section titles make a claim, not a category label.
- Keep it tight — a strong Report reads like a dense two-pager. Cut padding.
- Avoid narrating the document ("this validates…"), negation parallelisms ("not just X, it's Y"), throat-clearing, and filler adverbs ("crucially," "notably," "ultimately").

## Escape hatch
If the user wants a style incompatible with this system (dark theme, colorful infographic, brand treatment), don't force it here — build a custom artifact instead.
