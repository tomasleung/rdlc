# Examples — provenance and warning

Copied (not moved) from `04-powerbi-project/Foster Program Historical & Forecast Baseline/Spec/`:

- `foster_program_executive_dashboard_v1.html` — first full mockup (4 pages: Director Summary, Growth Analysis, Operations Planning, Methodology). 1,238 lines.
- `foster_program_executive_dashboard_v2.html` — refined iteration, 748 lines. Default active tab moved from Page 1 to Page 3 (Operations Planning); Page 3 gained explicit named sections (Key Insights + Peak/Lowest cards, KPI scorecards, hero chart, benchmark/variance table, heatmap) that match the structure of the actually-delivered PDF's "Current-Year Monitoring" page more closely than v1 does. Treat v2 as the more authoritative structural reference of the two.
- `foster_program_methodology_page.html` — standalone mockup of the Methodology page specifically.

**These are Phase-1-scoped: single dimension (Time only), no Centre/Region/Animal Type breakdown.** They are structural/pattern precedent only — the Tailwind-CSS-plus-tab-navigation HTML pattern, the page archetypes, the component shapes. Never copy their specific pages, questions (Q1–Q4 framing), KPIs, or narrative content into a mock for a different BRD without re-deriving everything from that BRD's own confirmed Insight Specification register. This is the same non-negotiable rule already established for `rdlc-data-analysis` (never silently carry a prior phase's content forward) — it applies here with equal force to a prior phase's *visual* content, not just its written content.
