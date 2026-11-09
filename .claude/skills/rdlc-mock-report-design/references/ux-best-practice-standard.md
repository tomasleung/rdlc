# UX best-practice standard

**Source:** adapted from a general Power BI UX/data-visualization framework the user supplied directly (not derived from this project's own prior artifacts, unlike the other three reference files). Filtered to what's genuinely new relative to `visual-style-tokens.md`, `page-architecture-patterns.md`, and `mock-html-pattern.md` — not duplicated here. Where the existing tokens already satisfy a principle, this file says so and points there instead of restating it.

## 1. Z-pattern hierarchy — already your page archetypes, now named explicitly
Top-left to bottom-right reading flow: critical KPIs first, supporting trend/context charts in the middle, detailed tables/matrices last. This is already what `visual-style-tokens.md`'s two page archetypes encode (Executive Narrative: verdict → KPI row → Q&A panels; Tactical Planning Tool: hero chart → KPI/insight snapshot → tables) — no new rule needed, just confirm each new page's archetype choice actually produces this top-to-bottom hierarchy rather than a flat grid.

## 2. Gestalt grouping — concrete, enforceable
- **Proximity:** a KPI card and its own comparison/benchmark line sit inside one bordered container, never split across two.
- **Similarity:** the same insight/question type uses the same chart type and color-mode pairing across every page of one mock — don't switch a trend line to a bar chart on page 3 for no reason.
- **Common region:** group related cards/panels inside a shared background container (already the card-fill-on-page-background pattern in `visual-style-tokens.md`) rather than letting them float independently.
- **Padding:** 10–15px internal padding inside every card/panel, consistent gap between sibling cards. Treat as a Tailwind spacing default in `mock-html-pattern.md`'s skeleton, not a per-page decision.

## 3. 60-30-10 color check — your tokens already roughly do this; verify, don't redo
Page background `#F1F5F9` + white card fill ≈ the 60% neutral base. Navy header `#0D2F6B` + slate text `#475569`/`#0f172a` ≈ the 30% structural/brand layer. Indigo/green/amber accents ≈ the 10% deliberate-signal layer. Before shipping a mock, check that accent colors (indigo/green/amber) are not creeping past roughly 10% of the page's visual weight — if a page looks "full of color," that's the signal something has drifted from an accent into a base color.

## 4. Never rely on color alone (Must — ties directly to rule 4's BRD-gating)
Every color-coded signal needs a paired, non-color cue: a direction arrow (▲/▼), a text label, or a numeric delta sitting next to it — not color alone. This applies even to purely descriptive (non-performance) indicators: a neutral "▲ +1.2pp vs. prior year" is fine under a descriptive-only BRD; a bare green dot with no label is not, regardless of what the color-mode table in `visual-style-tokens.md` resolves to. This is also an accessibility requirement (color-blind readers) and a governance one (BRD §13.1-style bans on unlabeled status implications) at the same time.

## 5. Navigation & interactivity
- Tab-based page navigation (already `mock-html-pattern.md`'s established pattern) stands in for Power BI's Page/Bookmark Navigator — keep it.
- If the mock includes any filter/slicer control (year selector, comparison-period picker), it needs an accessible, visible "Reset filters" control next to it — don't ship a filter with no way back to the default view.
- Tooltips explaining a KPI's calculation (not just its value) are good practice to mock where the real report would have one — but keep them short; a full methodology explanation belongs on the methodology page, not a tooltip.

## 6. Accessibility (Must, not Prefer)
- **Contrast:** every text/background pairing in `visual-style-tokens.md`'s color system must be checked against a 4.5:1 contrast ratio before use in a mock, same bar the real report should meet. Flag any pairing that doesn't meet it rather than silently shipping it — don't assume the existing tokens already pass; they were reverse-engineered from the built pages for color *values*, not contrast-audited.
- **Tab order:** when the mock includes any interactive element (tabs, filters, reset button), the HTML's DOM order should already match the intended reading/tab order — don't rely on CSS positioning alone to fix a logical top-to-bottom, left-to-right order.
- **Alt text:** every chart needs a short, descriptive `alt` or `aria-label` stating what it shows (e.g. "Line chart of monthly Fostered Animals, 2006 to 2026"), not a decorative/empty one.

## 7. Data currency and transparency (ties to rule 2's illustrative-label requirement)
A real report shows a "Data current as of [date/time]" marker so viewers know how fresh the numbers are. A mock should carry the equivalent in the same visual slot, but phrased as what it actually is — e.g. "Illustrative mockup — not real data" rather than a fabricated refresh timestamp, which would misleadingly imply the numbers are live. Reuse one visual placement for both real-report and mock purposes so the real build's eventual freshness indicator slots into the same place the mock already reserved for it.

## 9. Name every diagnostic-method term in plain language, every time it appears
Any term that comes from `rdlc-data-analysis/references/diagnostic-method-tiering.md` (mix effect, rate effect, contribution, decomposition, standardization, concentration, residual, etc.) is technical vocabulary a business stakeholder has not necessarily seen before. A legend label or axis title using the term alone is not enough — found during review of the Centre × Type decomposition view, where "Mix effect" / "Rate effect" appeared with no explanation anywhere on the page and the reviewer could not tell what the chart meant. Every time such a term appears on a mocked page:
- A short, plain-language definition must appear on the page itself (not only in chat/conversation) — a small explainer box near the chart is the established pattern, not a tooltip alone.
- A hover tooltip repeating the same plain-language text is good *supplementary* practice (ties to §5's tooltip guidance) but does not substitute for the on-page explainer — a viewer who never hovers must still be able to understand the chart.
- State the reconciling identity in plain terms where one exists (e.g. "Mix effect + Rate effect = Total observed difference, always") — this is what makes the decomposition trustworthy rather than two unexplained numbers next to each other.

## 10. Responsive/mobile layout — Prefer, not Must, and say why
`visual-style-tokens.md` fixes the canvas at 1920px desktop-report scale, matching how the real Power BI report is actually consumed (desktop/large-monitor, not mobile). Building a dedicated mobile layout for an illustrative HTML mock of a desktop-scale BI report is generally not worth the effort unless the BRD or Data Owner specifically asks for mobile consumption — treat as Prefer-tier, and say so explicitly rather than silently skipping it or silently over-building it.
