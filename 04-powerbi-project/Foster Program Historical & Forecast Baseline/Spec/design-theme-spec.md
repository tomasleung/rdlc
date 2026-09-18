# Design Theme Spec — Foster Dashboard

Reverse-engineered from the finished **Director Summary** and **Operations
Planning** pages after the user's manual layout/theme pass (2026-09-18).
This is the visual system to apply when building or fixing any other page —
**layout, color, and type only**. It does not cover chart-specific
formatting (see `color-strategy.md`) or dynamic-value wiring (see
`textbox-dynamic-value-format.md`) or visual naming/grouping (see
`visual-naming-convention.md`).

All values below were read directly from the built PBIR files, not
estimated. Where the two finished pages disagree slightly, the
disagreement is called out explicitly rather than silently picked for you.

---

## 1. Two page archetypes

The pages are not all the same "shape." Before building a new page, decide
which archetype it is — this drives structure, not just color.

### A. Executive Narrative (e.g. Director Summary)
A briefing meant to be read top-to-bottom by someone who wants the story,
not the raw numbers.
- Opens with a one-paragraph verdict + status badge *before* any chart.
- A KPI scorecard row for the "read nothing else" scan.
- Then a set of self-contained Q&A panels (one question each), each ending
  in a colored insight callout that states the takeaway in plain English.
- Prose and bullet lists are load-bearing content, not decoration.

### B. Tactical Planning Tool (e.g. Operations Planning)
A working tool for someone who needs to make an operational decision, not
be told a story.
- One central question, answered once — not a multi-panel Q&A structure.
- One hero chart, immediately followed by a KPI/insight snapshot.
- Trades prose for tables: a variance tracker and/or a full historical
  detail table/heatmap. Insight boxes are shorter and more clipped.

When starting a new page, name which archetype it is before laying anything
out.

---

## 2. Page canvas

| Property | Value |
|---|---|
| Page width | 1920px |
| Page background | `#F1F5F9` |
| Content card fill | `#FFFFFF` |
| Content card corner radius | `rectangleRounded`, curve `8L` |
| Content card shadow | color `#0F172A`, transparency `88D`, blur `8D`, distance `2D` |
| Content card outline | none |

Every white panel on both pages (KPI cards, Q-panels, section cards, the
hero-chart card) uses this exact shell. It's the single most reusable
component in the system.

---

## 3. Header & Navigation

| Element | Value |
|---|---|
| Header bar | full width × 96px, solid fill `#0D2F6B` |
| Title | single dynamic run bound to `KPI[Report Title]`, **no eyebrow line above it** |
| Title style | Segoe UI Semibold, **24pt**, bold not required (measure text itself isn't bolded), color `#ffffff` |
| Title position | x=32, y=32 |
| Nav "pill" container | same fill as header bar (`#0D2F6B`, effectively invisible) + 1px outline in `ThemeDataColor(0,0)` — no longer a visually distinct capsule |

### Nav buttons (`actionButton`, `rectangleRounded`, curve `60L`)

| State | Fill (default selector) | Text (default selector) | Hover |
|---|---|---|---|
| **Active** (current page) | `#4F46E5`, opaque | Segoe UI Semibold, 16pt, **bold**, `ThemeDataColor(0,0)` | fill `#334155`; text `#FFFFFF` |
| **Inactive** | `#1E293B` at 100% transparency (invisible) | Segoe UI Semibold, 15pt, not bold, `#DCE6F5` | fill `#334155` opaque; text `#FFFFFF` |

- Reference button positions (y=24, height=48): Director Summary x=1110 w=220 · Growth Analysis x=1315 w=181 · Operations Planning x=1504 w=220 · Methodology x=1720 w=170.
- **Active-tab underline**: a separate thin `line` shape (not part of the button), `ThemeDataColor(2,0)`, height 12, centered under the active button's label (≈192px wide), positioned at y=64.
- **Page navigation is wired and confirmed working.** Each button's `visualContainerObjects.visualLink`:
  ```json
  "visualLink": [{ "properties": {
    "show": { "expr": { "Literal": { "Value": "true" } } },
    "type": { "expr": { "Literal": { "Value": "'PageNavigation'" } } },
    "navigationSection": { "expr": { "Literal": { "Value": "'<target page id>'" } } }
  }}]
  ```
  `navigationSection` holds the **target page's internal PBIR id** (not its display name) as a literal string. The button for the page you're currently on uses an empty string `''` (no-op).

---

## 4. Typography scale

All sizes below are as authored (a mix of `pt` and `px` units exists in the
files — recorded as-is, don't normalize without checking which the
containing textbox already uses).

| Role | Font | Size | Weight | Color |
|---|---|---|---|---|
| Card / Section title | Segoe UI Semibold | 16pt | bold | `#0f172a` |
| Section eyebrow badge text (pill) | Segoe UI Semibold | 11pt | bold | panel accent color (see §5) |
| Headline (colored verdict statement) | Segoe UI Semibold | 12pt | not bold | panel accent color |
| Subheadline / description (factual framing line) | Segoe UI | 12pt | **bold** | `#475569` |
| KPI scorecard micro-label (e.g. "PROGRAM GROWTH SINCE 2006") | Segoe UI Semibold | 12pt | not bold | `#475569` |
| KPI scorecard big value | Segoe UI Semibold | 16pt | bold | accent color (see §5) |
| KPI scorecard footer/comparison line | Segoe UI | 11pt | not bold | `#475569` |
| Metric-tile micro-label (e.g. "PEAK MONTH", "LONG-TERM CAGR") | Segoe UI Semibold | 12pt | not bold | `#475569` |
| Metric-tile value | Segoe UI Semibold | 14pt | inconsistent — see §7 | accent color |
| Metric-tile footer/caption | Segoe UI | 11pt | not bold | `#475569` |
| Insight box heading ("Executive Insight:", "Key Insights") | Segoe UI Semibold | 16pt (standalone box) or 10-11pt (inline lead-in) | — | matches box's accent (see §5) |
| Insight box body text | Segoe UI | 9-14pt (varies by box — not yet standardized, see §7) | not bold | matches box's accent |

---

## 5. Color system (semantic, not decorative)

| Color | Hex | Meaning |
|---|---|---|
| Navy | `#0D2F6B` | Structural chrome (header only) |
| Indigo | `#4F46E5` / `#4f46e5` | Primary / analytical / growth accent |
| Green | `#059669` | On-target / stable / positive |
| Amber/Orange | `#D97706`, `#B45309` | Seasonal / attention / operational |
| Slate (body) | `#475569` | Standard secondary text (labels, captions, subheadlines) |
| Slate (dark, headings) | `#0f172a` | Primary heading text |
| Page background | `#F1F5F9` | Canvas |

### Insight-box background/text pairs (one per "mode")
| Panel / context | Background | Heading & body text |
|---|---|---|
| Q1 Growth (indigo) | `#EEF2FF` | `#312e81` |
| Q2 Seasonality (amber) | `#FFFBEB` | `#78350f` |
| Q3 Recent Stability (green) | `#ECFDF5` | (green family, matches box) |
| Q4 Forecast (indigo) | `#EEF2FF` | `#312e81` |
| Operations Planning Key Insights (amber) | `#FFFBEB` | `#78350f` |

Eyebrow/section badge pill: fill `#EEF2FF`, text `#4F46E5` (indigo default);
Operations Planning's top-right badge uses the amber pair instead
(`#FEF3C7` fill, `#B45309` text) — badge color follows the same per-panel
accent logic as the insight box, not a fixed default.

---

## 6. Component recipes

### KPI Scorecard (top row, 5 cards on Director Summary)
White card shell (§2) containing 3 stacked lines: micro-label → big value →
footer/comparison line, per the type scale in §4. This is the "read
nothing else" row — always at the top of an Executive Narrative page,
directly under the Executive Briefing.

### Metric tile (2-4 per Q-panel, e.g. "Long-Term CAGR", "Peak Month")
Same 3-line label/value/caption pattern as the KPI scorecard, but smaller
(14pt value vs 16pt) and laid out in a row of 2-4 inside a Q-panel rather
than across the full page width. Background `#F8FAFC` when a background is
shown (some tiles on Operations Planning currently have their background
shape hidden — see §7).

### Q-panel (Director Summary's 4-panel grid)
Card shell → eyebrow-style top-right badge (own accent) → title → headline
(colored, own accent) → subheadline (bold gray) → chart → metric tile row →
insight box (own accent). This is the repeating unit that makes the
Q&A narrative structure work — every new "question panel" should follow
this exact order.

### Section header (Operations Planning style, single-question pages)
Section title (16pt bold, `#0f172a`) → subtitle/description line → top-right
badge (own accent, matches the insight box below it) → Key Insights box +
KPI tile row side-by-side, same y, no overlap → hero chart → table(s).

---

## 7. Known inconsistencies (flag, don't silently resolve)

These exist in the finished pages today. Pick a rule before propagating
them further — don't let a new page copy whichever one happened to get
built first.

- **Metric-tile value bold weight is inconsistent.** Some tiles are bold
  (e.g. Long-Term CAGR), most aren't. Recommend: not bold, since that's
  the majority pattern already applied across Q2/Q3/Q4 tiles.
- **KPI scorecard value color doesn't consistently encode status.** Some
  values are green (implying "good"), others indigo (implying "primary
  metric, no status judgment") for what look like similarly-neutral
  numbers. Needs a rule: e.g. "green/red only when the number has a
  target to beat; indigo otherwise."
- **Insight-box body font size varies by box** (seen at both 9px and up to
  14pt across different panels) — not yet on the same scale as the rest
  of §4's type ramp.
- **Two metric-tile background shapes on Operations Planning are
  authored but `isHidden: true`** (Lowest Month / Peak Month tile
  backgrounds) — intentional or leftover from the redesign is unconfirmed.
