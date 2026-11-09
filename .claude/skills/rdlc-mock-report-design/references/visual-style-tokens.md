# Visual style tokens

**Source:** `04-powerbi-project/Foster Program Historical & Forecast Baseline/Spec/design-theme-spec.md`, itself reverse-engineered from the actual built, finished PBIR pages (not estimated). This is the real, verified visual style authority for this project — use it, not an improvised palette.

**Important correction:** `04-powerbi-project/Design Guide/BC SPCA style guidelines.pdf` is NOT a visual/brand design guide — it is a writing/voice/copy style guide (terminology, grammar, animal-pronoun rules, number formatting). It has no color palette, typography, or layout content. See `copy-voice-standard.md` for what it actually governs. Do not look to it for colors or fonts.

`04-powerbi-project/Foster Program Historical & Forecast Baseline/Spec/theme.md` exists but is raw, unverified notes (conflicting header-color values) — superseded by `design-theme-spec.md`, which was checked against the real built files. Prefer `design-theme-spec.md` wherever they conflict.

## Page canvas
| Property | Value |
|---|---|
| Page width | 1920px (desktop-report scale, not print) |
| Page background | `#F1F5F9` |
| Content card fill | `#FFFFFF`, corner radius 8px, subtle shadow (`#0F172A` at low opacity, 8px blur, 2px distance), no outline |

## Header & navigation
- Header bar: full width × 96px, solid `#0D2F6B` (navy).
- Title: single line, Segoe UI Semibold 24pt, white, no eyebrow line above it.
- Nav buttons: active = `#4F46E5` (indigo) fill, bold white text; inactive = invisible fill, `#DCE6F5` text; active tab gets a separate underline bar.

## Typography scale
| Role | Font | Size | Weight | Color |
|---|---|---|---|---|
| Card/section title | Segoe UI Semibold | 16pt | bold | `#0f172a` |
| Eyebrow/badge pill text | Segoe UI Semibold | 11pt | bold | panel accent |
| Headline (colored verdict) | Segoe UI Semibold | 12pt | not bold | panel accent |
| Subheadline (factual framing) | Segoe UI | 12pt | bold | `#475569` |
| KPI micro-label | Segoe UI Semibold | 12pt | not bold | `#475569` |
| KPI big value | Segoe UI Semibold | 16pt | bold | accent |
| Metric-tile value | Segoe UI Semibold | 14pt | **not bold** (resolved default — see below) | accent |
| Insight box heading | Segoe UI Semibold | 16pt (standalone) / 10-11pt (inline) | — | matches box accent |
| Insight box body | Segoe UI | 11pt (resolved default — see below) | not bold | matches box accent |

## Color system (semantic, not decorative)
| Color | Hex | Meaning |
|---|---|---|
| Navy | `#0D2F6B` | Structural chrome — header bar only |
| Indigo | `#4F46E5` | Primary / analytical / growth accent |
| Green | `#059669` | On-target / stable / positive |
| Amber/Orange | `#D97706` / `#B45309` | Seasonal / attention / operational |
| Slate body | `#475569` | Secondary text, labels, captions |
| Slate heading | `#0f172a` | Primary heading text |

Insight-box background/text pairs (one per "mode"):
| Mode | Background | Text |
|---|---|---|
| Growth / analytical (indigo) | `#EEF2FF` | `#312e81` |
| Seasonal / attention (amber) | `#FFFBEB` | `#78350f` |
| Stable / positive (green) | `#ECFDF5` | green family |

## Two page archetypes — decide this before laying out any new page
**A. Executive Narrative** — read top-to-bottom for the story, not raw numbers. Opens with a one-paragraph verdict before any chart, a KPI scorecard row, then self-contained Q&A panels each ending in a colored insight callout.

**B. Tactical Planning Tool** — a working tool for an operational decision. One central question answered once, one hero chart immediately followed by a KPI/insight snapshot, tables over prose (variance tracker, heatmap), shorter insight boxes.

## Resolved defaults (the source material flagged these as inconsistent across the finished pages — pick one, don't propagate the inconsistency into a new mock)
- Metric-tile value weight: **not bold** (majority pattern already in the built pages).
- KPI scorecard value color: **indigo by default; green/red only when the number has an approved target to beat** — never color a purely descriptive number green/red (this also matches `rdlc-data-analysis` rule 6: no performance-style status language unless the BRD authorizes it).
- Insight-box body text size: **11pt**, on the same scale as the rest of the type ramp.
