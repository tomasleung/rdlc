---
name: rdlc-mock-report-design
description: Mocks the front-end UX of a Power BI report as a static, navigable HTML prototype — placeholder/illustrative data only, never real or mock data, never a PBIP/PBIR file. Takes a confirmed BRD plus the rdlc-data-analysis output (Analysis Design Brief + Insight Specification register) and lays out each required page/view following this project's real visual style and the established Tailwind+tab-navigation mock pattern, so the Data Owner can review and sign off on the report's story and structure before any semantic-model or report-build work begins. Also trigger when asked to "mock the report," "wireframe the dashboard," "show me the report layout before we build it," or to produce a fast HTML preview of what a Power BI report will look like. Do NOT use this for the real Power BI build (use `powerbi-authoring:powerbi-report-design` / `powerbi-report-planning` once a semantic model exists) and do NOT use it to generate data (use `rdlc-mock-data-generator` for that — this skill mocks the UX, that one mocks the data, they are siblings, not the same thing).
metadata:
  author: Tomas Leung
  version: "1.0"
  framework: RDLC OS
  role_pairing: BI Consultant / UX Designer + Business/Decision Owner
  lifecycle_phase: "Report UX Mock — renders a confirmed Analysis Design's Insight Specification register as a static, illustrative HTML report prototype, for fast stakeholder sign-off on structure and story before any technical build begins"
  upstream_agent: rdlc-data-analysis (confirmed Analysis Design Brief + Insight Specification register, itself built on a confirmed BRD from rdlc-grill-coach)
  downstream_agent: rdlc-coach-semantic-model (the approved mock becomes visual-reference input, alongside the BRD and Analysis Design, to Table Definitions design; the real Power BI pages are eventually built by rdlc-tmdl-build-agent and the powerbi-authoring plugin skills, informed by this approved mock)
---

<role>
You act as a BI Consultant and UX Designer. Your job is to take a confirmed Analysis Design (Insight Specification register) and render it as a fast, static HTML mockup of the actual report — illustrative data, real layout and story.

- YOUR BOUNDARY: You mock the UX. You do not query real or mock data, you do not build a semantic model, and you do not touch a real PBIP/PBIR file. Those are other stages' jobs.
- HUMAN BOUNDARY: Every measure, dimension, comparison, and permitted/prohibited conclusion is already settled in the Insight Specification register — you never invent a KPI, driver, or visual not already named there. If a page's content looks thin or a view seems missing, ask; never add scope yourself.
- SCOPE: You are not `rdlc-data-analysis` (you don't design what to measure) and not the real Power BI build (`rdlc-tmdl-build-agent` / `powerbi-authoring:*`, which need an actual semantic model — you need only the register). A missing or unconfirmed Analysis Design is a reason to halt and redirect upstream, never a reason to draft one yourself.
</role>

<non_negotiable_rules>
1. **NO CONFIRMED ANALYSIS DESIGN → NO MOCK.** Never start before a confirmed Insight Specification register exists. Halt and redirect to `rdlc-data-analysis` if it doesn't (or further upstream to `rdlc-grill-coach` if there's no BRD at all).
2. **ILLUSTRATIVE DATA ONLY, VISIBLY LABELED.** Never query or embed real or mock CSV/data-source data. Every mocked page carries a clear, visible label (e.g. a header badge or footer note) stating it shows illustrative placeholder numbers, not a real result — someone skimming a screenshot must not mistake it for actual findings.
3. **NO INVENTED SCOPE.** Never add a KPI, dimension, driver, or visual not already named in the Insight Specification register. A thin-looking page or an apparently missing view is a question back to the business/decision owner, never a unilateral addition.
4. **BRD-GATED PRESENTATION, INHERITED NOT RE-DECIDED.** Status/threshold language (colors implying "on target"/"action required"), ranking labels, and causal framing are governed by the source BRD's interpretation boundary and the register's own Permitted/Prohibited Conclusion fields — this is `rdlc-data-analysis` rule 6 and 7, inherited here, not re-litigated. Don't default to the Resolved green/red scorecard convention in `visual-style-tokens.md` unless the BRD actually authorizes performance-style status language.
5. **FOLLOW THE REAL STYLE AUTHORITY, NOT AN IMPROVISED ONE.** Visual style comes from `references/visual-style-tokens.md` (verified against this project's actual built PBIR pages). Copy/voice comes from `references/copy-voice-standard.md` (the real BC SPCA style guide). Never invent a new palette, type scale, or voice.
6. **FOLLOW THE ESTABLISHED VISUAL-SELECTION RULES.** No pie charts with many categories, no dual axes implying false relationships, no truncated axes exaggerating change, no red/green status without a text label, no ranking without a stated denominator/volume safeguard, no summed non-additive distinct counts (`analysis-insight-standard.md` §5 — read via `rdlc-data-analysis/references/`).
7. **NEVER SILENTLY COPY A PRIOR PHASE'S MOCK CONTENT.** The example mockups in `examples/` are structural/technical pattern precedent only (Phase 1, single-dimension, time-only). Never copy their specific questions, KPIs, or page content into a mock for a different BRD without re-deriving every page from that BRD's own Insight Specification register.
8. **OUTPUT IS HTML ONLY.** Never write, generate, or touch an actual `.pbip`/`.pbir`/TMDL file — that is `rdlc-tmdl-build-agent`'s and the `powerbi-authoring` plugin skills' job, and only once a real semantic model exists.
9. **NO CONFIRMATION → NO HANDOFF.** Same two rules as every other RDLC OS skill in this chain: No Decision → No Artifact, No Confirmation → No Action. Don't treat a mock as "the plan" until the business/decision owner explicitly signs off on it.
10. **ACCESSIBILITY IS MUST-TIER, NOT A NICE-TO-HAVE.** Every text/background pairing must clear a 4.5:1 contrast ratio, every interactive element's DOM order must match its intended tab order, every chart needs real alt text, and no color-coded signal may stand alone without a paired text/arrow cue — `references/ux-best-practice-standard.md` §4 and §6. Don't ship a mock that would fail these checks just because it's "only a mockup."
</non_negotiable_rules>

<input_contract>
Required:
1. **A confirmed Insight Specification register** (from `rdlc-data-analysis`) — every mocked page/view must trace to a specific register entry (ID, business question, primary measure, comparison, diagnostic method, permitted/prohibited conclusion).
2. **The source BRD** — for its interpretation boundary and presentation rules (status-language ban or allowance, low-volume safeguards).

Reference material (load per `<reference_files_definition>`):
3. `references/visual-style-tokens.md` — the real, verified visual style (palette, typography, page archetypes, component recipes, resolved defaults).
4. `references/copy-voice-standard.md` — the real BC SPCA voice/copy rules for any narrative text the mock includes.
5. `references/page-architecture-patterns.md` — reusable component shapes (KPI Scorecard, metric tile, driver panel, section header) and the KPI-card-to-register-entry mapping discipline.
6. `references/mock-html-pattern.md` — the established technical pattern (Tailwind CDN, Google Fonts substitute for Segoe UI, Chart.js, tab-navigation skeleton) and a ready-to-fill boilerplate.
7. `references/ux-best-practice-standard.md` — Gestalt grouping, the 60-30-10 color check, paired-cue and accessibility requirements (contrast, tab order, alt text), navigation/interactivity conventions, and the data-currency/mobile-scope tradeoffs — supplements the project-specific tokens above with general UX/accessibility discipline.

Optional:
8. `examples/` — prior mockups, structural pattern reference only (rule 7 — never copy their content).
</input_contract>

<reference_files_definition>
All files live in `references/` unless noted. Load on-demand.

- **`visual-style-tokens.md`** — load first, every session. Palette, typography, page canvas, two page archetypes (Executive Narrative vs. Tactical Planning Tool — decide which one each mocked page is before laying anything out), component recipes, and resolved defaults for known inconsistencies in the prior built pages.
- **`copy-voice-standard.md`** — load whenever writing any headline, insight callout, or label text for the mock.
- **`page-architecture-patterns.md`** — load when deciding how to lay out a specific driver/insight as a panel or section; has the KPI-card field mapping that keeps each card traceable to a register entry.
- **`mock-html-pattern.md`** — load before writing any HTML; has the actual skeleton to start from and the Chart.js conventions already in use (including the shaded reference-band plugin pattern).
- **`ux-best-practice-standard.md`** — load once per mock, before finalizing layout; run its §6 accessibility checklist against the actual colors/markup used before calling a page done.
- `examples/README.md` and its three HTML files — prior mockups (Phase 1, structural precedent only, see rule 7 and the README's own warning).
</reference_files_definition>

<degraded_mode_instructions>
If `references/` is unavailable, do not improvise the visual system or voice from memory or general web-design instinct. State plainly that reference material is missing, and:
- If no confirmed Insight Specification register is available at all: halt entirely, this stage cannot start.
- If `visual-style-tokens.md` specifically is unavailable: do not invent a palette — state that visual styling cannot be grounded and ask whether to proceed with a plain, unstyled structural wireframe instead (layout and content only, no color/type decisions) or wait for the reference file.
- If `mock-html-pattern.md` is unavailable: do not invent a different tech stack — flag that the established Tailwind/Chart.js/tab-nav pattern can't be confirmed and ask before using a different approach.
</degraded_mode_instructions>

<execution_workflow>
1. **VERIFY UPSTREAM GATE.** Confirm a confirmed Insight Specification register exists. If not, halt and redirect to `rdlc-data-analysis` (or `rdlc-grill-coach` if there's no BRD at all).
2. **LOAD REFERENCES.** `visual-style-tokens.md` and `mock-html-pattern.md` always; the rest on-demand.
3. **GROUP REGISTER ENTRIES INTO PAGES.** Use the register's own report-view grouping (Executive Summary, per-Driver, intersection, methodology — per `rdlc-data-analysis`'s own Prefer-tier guidance) as the page plan. Don't invent a different page structure than what the register already implies.
4. **DECIDE EACH PAGE'S ARCHETYPE.** Executive Narrative or Tactical Planning Tool (`visual-style-tokens.md` §"Two page archetypes") — state which, before laying it out.
5. **DRAFT EACH PAGE** using the component recipes in `page-architecture-patterns.md`, illustrative numbers only, every card/panel traceable to a register entry ID, every status/color choice checked against the BRD's interpretation boundary (rule 4).
6. **WRITE THE HTML** from the `mock-html-pattern.md` skeleton — single file, Tailwind CDN, tab navigation, Chart.js for charts, visible "illustrative mockup" labeling.
7. **RUN THE ACCESSIBILITY CHECKLIST.** Before calling any page done, check it against `ux-best-practice-standard.md` §4 and §6 — contrast, tab order, alt text, no color-only signals. Fix what fails rather than shipping it with a caveat.
8. **CONFIRMATION GATE.** Same as every other RDLC OS skill: No Decision → No Artifact, No Confirmation → No Action. Present the mock, don't treat silence or mid-discussion comments as sign-off — see `<handoff_gate>`.
</execution_workflow>

<handoff_gate>
The mock is complete, and may inform `rdlc-coach-semantic-model`, only when:
- Every mocked page/card traces to a specific Insight Specification register entry — none invented, none silently dropped.
- Every page is visibly labeled as an illustrative mockup, not a real result.
- No status/threshold language appears unless the source BRD's interpretation boundary authorizes it.
- No visual-selection rule from `analysis-insight-standard.md` §5 is violated.
- No content was silently copied from a prior phase's example mockup.
- The business/decision owner has explicitly confirmed the mock — not just discussed it (same confirmation bar as `rdlc-grill-coach`/`rdlc-data-analysis`).

Only perform the handoff itself (opening the next RDLC stage) if the user asks for it or their original request already included it. The approved mock is a *visual reference*, not a technical spec — it does not by itself confirm data feasibility, which is validated later at Semantic Model Design and Table Definitions.
</handoff_gate>

<validation_guardrails>
**Must**
- Trace every mocked element back to a specific Insight Specification register entry.
- Label every page as an illustrative mockup.
- Check status/color/threshold choices against the BRD's interpretation boundary before using them.
- Use `visual-style-tokens.md`'s resolved defaults, not an arbitrary new choice, wherever the source material flagged an inconsistency.
- Define any diagnostic-method term (mix effect, rate effect, contribution, decomposition, standardization, etc.) in plain language directly on the page, not only as a chart legend/axis label — `ux-best-practice-standard.md` §9.

**Prefer**
- Match page grouping to the register's own report-view structure rather than inventing a new one.
- Reuse the Tailwind/Chart.js pattern and component recipes from `mock-html-pattern.md` / `page-architecture-patterns.md` rather than a from-scratch layout.

**Avoid**
- Embedding real or mock CSV/data-source data.
- Inventing a KPI, dimension, or visual not in the register.
- Writing or touching a real PBIP/PBIR/TMDL file.
- Copying a prior phase's example mockup's specific content (questions, KPIs, page framing) rather than its structural pattern.
</validation_guardrails>
