# Mock HTML technical pattern

**Source:** the two real prior mockups in `examples/` (`foster_program_executive_dashboard_v1.html`, `_v2.html`). This is the proven, already-used technical shape for a report mock in this project — use it as the starting skeleton, don't invent a different stack.

**Deliberately different from the BRD/Analysis Design HTML pattern.** Those documents (`rdlc-grill-coach`, `rdlc-data-analysis` output) are print-paginated governance documents — single scroll, custom CSS, Letter-page sizing, meant to be read or printed. A report mock is the opposite: it should *feel like a multi-page Power BI report*, with tab navigation between pages, not a scrolling document. Don't reuse the BRD's CSS pattern here.

## Stack
- **Tailwind CSS via CDN** (`<script src="https://cdn.tailwindcss.com"></script>`) — all layout/spacing/color utility classes, no separate stylesheet file.
- **Fonts:** Google Fonts `Poppins` (headings, 400/600/700) + `Lato` (body, 400/700) — these are the mock's substitute for the real report's Segoe UI (a Microsoft system font not reliably available in a browser). Map weight/size intent from `visual-style-tokens.md`'s typography scale onto Poppins/Lato, don't try to load Segoe UI itself.
- **Chart.js via CDN** (`cdn.jsdelivr.net/npm/chart.js`), plus `chartjs-plugin-annotation` when a chart needs a shaded reference band or annotation line (e.g. a peak-activity window, a benchmark zone).
- **Tab navigation, single file, no routing:** every report "page" is a `<div id="pageN" class="tab-content">` (hidden unless `.active`), switched by a `switchTab(pageId)` JS function wired to header nav buttons. This is the mechanism that makes the mock feel like a real multi-page report instead of one long scroll.

## Skeleton (fill in, don't restructure)
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title><!-- report title --></title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&family=Lato:wght@400;700&display=swap" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body { font-family: 'Lato', sans-serif; }
    h1, h2, h3, h4 { font-family: 'Poppins', sans-serif; }
    .tab-content { display: none; }
    .tab-content.active { display: block; }
  </style>
</head>
<body class="bg-slate-100 text-slate-800 antialiased">
  <header class="bg-slate-900 text-white shadow-lg sticky top-0 z-50">
    <!-- title + nav buttons, one per mocked page, onclick="switchTab('pageN')" -->
  </header>
  <main class="max-w-7xl mx-auto px-6 py-8">
    <div id="page1" class="tab-content active space-y-8"> <!-- first page --> </div>
    <div id="page2" class="tab-content space-y-8"> <!-- ... --> </div>
  </main>
  <script>
    function switchTab(pageId) { /* toggle .active on tab-content + nav button state — copy verbatim from an example file */ }
    // one `new Chart(...)` block per canvas, see examples for options shape
  </script>
</body>
</html>
```

## Chart.js conventions already established
- Line charts for trend-over-time; shaded `fill: true` area under the line for a single series, plain `fill: false` line when overlaying a second comparison series on the same axes.
- Bar charts for discrete period-over-period comparison (e.g. annual totals), with the current/incomplete period often shown as a lighter shade of the same color to signal "in progress."
- A `beforeDraw` plugin for a shaded reference band (e.g. peak-season window) — see `page3Plugin` in the v2 example for the exact pattern: compute pixel positions from category indices, `fillRect` a low-opacity band, label it inline.
- Color charts from `visual-style-tokens.md`'s semantic palette (indigo = analytical/growth, amber = seasonal/attention, green = stable/positive) — these are close to but not pixel-identical to the Chart.js hex values used in the prior examples (`#4f46e5`/`#f59e0b`/`#10b981` there vs. `#4F46E5`/`#D97706`/`#059669` in the canonical token file); treat the token file's hex values as authoritative for new work, the prior examples' slightly different shades as historical artifacts.
- Never fabricate an axis range or data label that implies more precision than "illustrative" — see `SKILL.md` non-negotiable rule on placeholder data.
