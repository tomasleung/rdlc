# Page architecture patterns

**Sources:** `04-powerbi-project/Foster Program Historical & Forecast Baseline/Spec/design-theme-spec.md` §6 (component recipes), `reportlayout.md` (KPI-card-to-measure mapping pattern), `visual-naming-convention.md` (section-grouping principle, simplified for mock purposes — the full PBIR JSON mechanics in that file are `rdlc-tmdl-build-agent`'s concern, not this skill's).

## Component recipes (reuse these shapes, don't invent new ones)

**KPI Scorecard** (top row, repeating) — white card shell, 3 stacked lines: micro-label → big value → footer/comparison line. The "read nothing else" row — always at the top of an Executive Narrative page.

**Metric tile** (2–4 per panel) — same label/value/caption pattern as a KPI Scorecard but smaller (14pt value vs 16pt), laid out in a row inside a panel rather than across the full page width.

**Q-panel / driver panel** (the repeating narrative unit) — card shell → top-right eyebrow badge (own accent) → title → headline (colored, own accent) → subheadline (bold gray, factual framing) → chart → metric-tile row → insight callout box (own accent). Every new driver/question panel follows this exact order.

**Section header** (Tactical Planning page style) — section title → subtitle/description → top-right badge → Key Insights box + KPI tile row side-by-side (same y, no overlap) → hero chart → supporting table(s).

## KPI-card-to-measure mapping discipline
Every mocked KPI card must name, even with placeholder values, which *role* each visual element plays — not just a number. Pattern (from `reportlayout.md`'s actual card specs):

| Visual element | What it represents |
|---|---|
| Main value | The primary measure (illustrative number) |
| Comparison line | Prior-period value + formatted variance (e.g. "▲ 2.3% vs [prior period]") |
| Benchmark value/label | The comparison baseline used and its period |
| Status text/color | Only present if the BRD's interpretation boundary authorizes status language — see `rdlc-data-analysis` rule 6. Otherwise this row doesn't exist on the card. |

This maps a mocked card directly onto an Insight Specification register entry's Primary Measure / Comparison / (optional) status fields — a reviewer or the later semantic-model stage can trace every card back to a specific register row.

## Section naming (lightweight version of `visual-naming-convention.md`)
Name every section/panel in the mock's own code comments or `id`/`aria-label` by its *role*, matching the Insight Specification register's ID where one exists (e.g. `<!-- CATU-CENTRE-01: Centre volume & utilization -->`). This is not the full PBIR Selection-pane grouping mechanism (that's built later, in the real report, by `rdlc-tmdl-build-agent` / `powerbi-authoring` skills) — it exists here so the mock's sections can be traced 1:1 back to register entries when the real build happens, the same traceability discipline `rdlc-data-analysis` already requires of its own output.

## Known inconsistencies to not re-introduce
See `visual-style-tokens.md`'s "Resolved defaults" section — these were flagged as inconsistent across the real finished pages. Apply the resolved default, don't pick arbitrarily per new mock.
