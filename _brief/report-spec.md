# Report Spec

## Report identity
- Report name: Animal Flow — Foster Pathway Analytics, Phase 1 (Trusted Baseline)
- Semantic model: `04-powerbi-project/foster analysis.SemanticModel` (Date, Centre, Animal, Intake Type, Animal Intakes; measures: Intake Count, Animals Ever Fostered, Foster Placement Rate (%))
- Audience: Animal Flow Manager / Animal Flow leadership — recurring weekly/monthly review, not a one-time board read
- Primary purpose: Establish a trusted historical baseline of foster utilization and surface the strongest associated drivers (time, geography, species, age) — explicitly *before* KPI thresholds/targets exist
- Delivery target: Local PBIP only for now

## User decisions and constraints
- Scope: 5 pages, reconciled from BRD §6/§7, Discovery Framework §7/§8, and confirmed Table Definitions v1.3 §10 (the authoritative Phase 1 scope per CLAUDE.md)
- Page count: 5
- Interactivity: Deliberately minimal — 3 light global filters (Year, Region, Species Group), inline, on every page. See "Design direction" below for why this deviates from the pure Narrative-Story archetype's "no slicers" default.
- Design direction: Narrative Story composition (landing + 3 driver chapters + closing comparative synthesis); Clinical Calm tone, remixed as "Trusted Baseline"
- Publishing: None yet — local PBIP validation only
- Tooling: `powerbi-modeling-mcp` connected and proven this session; `rdlc-tmdl-build-agent` owns the semantic model; `powerbi-report-authoring` will own PBIR file mechanics when build is approved
- Model edit permissions: No model changes anticipated — all 3 required measures already exist
- Accessibility: WCAG AA minimum on every text/background pair; alt text on every chart; no color-only encoding
- Data caveats (must stay visible in the report, not hidden): Species Group mapping incomplete ("Other" = not-yet-mapped, not a finished category); Municipality-level geography deferred — Region/Centre only; EAB/LSAI Program segmentation is Phase 2, out of scope; Prototype-mode mock data, not production ShelterBuddy data

## Narrative
- Core story: "Of the animals BC SPCA takes in, how many experience foster care, and what's associated with it — before we define what 'good' looks like."
- Audience promise: A weekly/monthly-reviewable baseline the Animal Flow Manager can trust, structured as four short evidence-backed chapters (time, geography, species, age) rather than a single dense dashboard.
- Key questions answered: How many animals entered care / were ever fostered / what % fostered (BRD §6); has foster utilization changed over time, where is it used, which animals/ages drive demand (Discovery Framework §4/§7); which single factor shows the widest spread (synthesis, page 5).

## Design identity (from `powerbi-report-design` Step 1)
- Tone: **Brand-Forward — BC SPCA Colours**. User supplied the organization's actual Power BI theme JSON (`"BC SPCA Colours "`) — per the tone catalog's Brand-Forward template, the brand *is* the tone, not a starting point to remix. Surface `#F0F0F0` (neutral light grey, exactly as the theme's page-background override specifies — no blue tint invented). Calibri throughout (title/header/label/callout text classes all specify it explicitly). Visual titles black `#000000`; header-level text navy `#00337F`. Reserve `good`/`neutral`/`bad` (`#7EA83F`/`#F9E376`/`#C64420`) for genuine status only — Phase 1 still has no KPI thresholds to be good/bad against, so `bad` is used exclusively for data-quality flags, never performance judgment. This constraint survives the palette swap unchanged.
- Signature: **Caption-style takeaway under every chart (S4) + single-accent discipline on the hero measure (S5), re-keyed to brand navy**. Every chart still carries a hand-authored one-sentence takeaway beneath it. `Foster Placement Rate (%)` now wears `dataColors[0]` (`#00337F`, BC SPCA navy) everywhere it appears — cards, lines, bars; supporting measures use the theme's own grey family (`#5F6B6D`, `#7F898A`) rather than an invented slate. Same discipline, brand-correct color.
- Deliberate deviation from archetype default: Narrative Story's own guidance says "no slicers on narrative pages." This report keeps 3 light, inline, dropdown-style filters (Year, Region, Species Group) on every page instead, because the BRD frames this as recurring weekly/monthly operational review (§3), not a single-read persuasion piece — the Animal Flow Manager needs to re-slice the same trusted story on repeat visits, not just read it once.

## Page plan (archetypes from `powerbi-report-design` Step 3)

1. **Foster Baseline** (landing)
   - Archetype: Executive Summary
   - Layout variant: A — Hero-Right — *3 KPIs + one clear hero trend, no single dominant metric outranks the others*
   - Purpose: Establish the baseline numbers the rest of the report explores
   - Visuals: 3 composite KPI cards (value + month-over-month sparkline — trend context, not a target/threshold) for Intake Count, Animals Ever Fostered, Foster Placement Rate (%); hero line chart of Foster Placement Rate (%) by month across the full 2024–2025 range; two compact "preview" mini-bars (top-3 Region, top-3 Species Group) previewing pages 3–4
   - Fields/measures: `Animal Intakes[Intake Count]`, `Animal Intakes[Animals Ever Fostered]`, `Animal Intakes[Foster Placement Rate (%)]`, `Date[Month Name]`/`Date[Year]`, `Centre[Region]`, `Animal[Species Group]`
   - Slicers/interactions: Year, Region, Species Group (inline, top-right)

2. **Time Analysis**
   - Archetype: Narrative Story
   - Layout variant: B — Single-Column-Scroll — *two genuinely distinct sub-questions (long-run trend AND seasonality), each deserving its own beat*
   - Purpose (thesis): "Has foster utilization changed over time?" (Discovery Framework Driver #1)
   - Visuals: Beat 1 — Year/Quarter trend line of Foster Placement Rate (%), annotated with the YoY delta; Beat 2 — Season comparison column chart (Winter/Spring/Summer/Fall), annotated with the peak-season callout
   - Fields/measures: `Date[Year]`, `Date[Quarter]`, `Date[Season]`, `Animal Intakes[Foster Placement Rate (%)]`
   - Slicers/interactions: Year, Region, Species Group (inline)

3. **Geography Analysis**
   - Archetype: Narrative Story
   - Layout variant: A — 7/5 Split — *single clear ranking question with room for an annotation panel*
   - Purpose (thesis): "Where is foster being utilized?" (Discovery Framework Driver #2)
   - Visuals: Anchor — Region ranked bar (Foster Placement Rate %, sorted desc, reference line at portfolio average); Annotation panel — top/bottom region gap callout + a "Spotlight" text naming the leading Region's top Centre (Centre driver coverage folded into the annotation, not a second chart, to respect the one-anchor-chart discipline)
   - Fields/measures: `Centre[Region]`, `Centre[Centre]`, `Animal Intakes[Foster Placement Rate (%)]`
   - Slicers/interactions: Year, Region, Species Group (inline)
   - Explicit exclusion: no Municipality visual — deferred, no source data (BRD §11, Table Defs §4.2)

4. **Animal Characteristics**
   - Archetype: Narrative Story
   - Layout variant: B — Single-Column-Scroll — *two named driver stories (Species, Age Group) that deserve separate beats, per Discovery Framework Drivers #3–4*
   - Purpose (thesis): "Which animals drive foster demand?"
   - Visuals: Beat 1 — Species Group ranked bar (caption explicitly flags "Other" as not-yet-mapped, not a finished category); Beat 2 — Age Group ranked bar (Neonate/Juvenile/Adult/Senior). Sex appears only as a caveated sentence in Beat 2's body text ("retained as a low-cost, plausible but untested driver — treat as hypothesis"), not as its own chart, matching its documented hypothesis-only status
   - Fields/measures: `Animal[Species Group]`, `Animal[Age Group]`, `Animal[Sex]`, `Animal Intakes[Foster Placement Rate (%)]`
   - Slicers/interactions: Year, Region, Species Group (inline)

5. **Driver Assessment** (closing synthesis)
   - Archetype: Comparative Benchmark
   - Layout variant: A — Side-by-Side — *ranking one dimension (Intake Group, 8 groups) against a portfolio baseline, plus a secondary breakdown (Breed)*
   - Purpose (thesis): Which single factor shows the widest Foster Placement Rate (%) spread?
   - Visuals: Headline — Intake Group ranked Δ-from-average bar, zero-anchored, sorted by |Δ| desc; Small multiples — top-6 Breeds, each showing current rate vs. portfolio average with Δ% annotated; closing callout naming the single widest-spread driver across the whole report
   - Fields/measures: `Intake Type[Intake Group]`, `Animal[Breed]`, `Animal Intakes[Foster Placement Rate (%)]`
   - Slicers/interactions: Year, Region, Species Group (inline)

## Design system summary
- Theme name + base palette: **"BC SPCA Colours"** (user-supplied brand theme, Brand-Forward tone) — surface `#F0F0F0`, primary accent navy `#00337F` (`dataColors[0]`), secondary amber `#D28E00` (`dataColors[1]`), grey family `#5F6B6D`/`#7F898A` for context, semantic `good`/`neutral`/`bad` = `#7EA83F`/`#F9E376`/`#C64420` reserved for genuine status — `bad` used only for data-quality alerts, never performance judgment (no Phase 1 thresholds exist to judge against)
- Color semantics: `Foster Placement Rate (%)` → navy (`measure_match` everywhere); `Animals Ever Fostered` → grey; `Intake Count` → light grey. One hue per measure, consistent report-wide.
- Typography pairing: Calibri throughout, per the supplied theme's `title`/`header`/`label`/`callout` text classes (fallback stack: Calibri, 'Segoe UI', Arial, sans-serif). Visual titles 14pt black `#000000`; header-level text 12pt navy `#00337F`. Page-level display headings scale larger (28–36pt Calibri Bold) for report-cover hierarchy the theme doesn't define at that scale. Tabular numerals on all KPI values and variance figures.
- Layout pattern: FHD 1920×1080, 12-column grid, 32px margin / 24px gutter / 8px snap. Slicers inline top-right (3 slicers = inline row, never a vertical rail). Title always left-anchored, descriptive thesis sentence, never a generic label.
- Accessibility commitments: WCAG AA contrast on every text/background pair; alt text on every chart; color never the sole channel (labels/position always pair with hue); `bad` (`#C64420`) strictly reserved for genuine data-quality flags, never performance judgment.

## Canonical design contract

```yaml
Design Brief:
  generated_by: powerbi-report-design
  contract_version: 1
  mode: greenfield
  design_identity:
    tone: "Brand-Forward -- BC SPCA Colours (user-supplied theme): surface #F0F0F0, Calibri throughout, navy #00337F primary / amber #D28E00 secondary, bad=#C64420 reserved strictly for data-quality alerts (no Phase 1 thresholds to judge performance against)"
    signature: "Caption-style one-sentence takeaway under every chart (S4), combined with single-accent discipline (S5): Foster Placement Rate (%) alone wears BC SPCA navy (#00337F) everywhere it appears; every other measure and non-highlighted category stays theme grey (#5F6B6D / #7F898A)"
  archetype: "Narrative Story (composition: Executive Summary landing -> Narrative Story chapters -> Comparative Benchmark synthesis)"
  color_map:
    - measure: "Animal Intakes[Foster Placement Rate (%)]"
      color: "#00337F"
      tint: "#99ADCC"
    - measure: "Animal Intakes[Animals Ever Fostered]"
      color: "#5F6B6D"
      tint: "#D3D8D9"
    - measure: "Animal Intakes[Intake Count]"
      color: "#7F898A"
      tint: "#E4E7E7"
  pages:
    - name: "Foster Baseline"
      role: landing
      archetype: Executive
      layout_variant: A
      variant_rationale: "3 KPIs (Intake Count, Animals Ever Fostered, Foster Placement Rate %) plus one clear hero trend metric -- exact match to Variant A's '3-4 KPIs + one hero metric' signal."
      page_background: "#F0F0F0"
      layout_contract:
        canvas: { width: 1920, height: 1080, margin: 32, gutter: 24, snap: 8 }
        grid:
          columns: 12
          rows: 12
          regions:
            header:  [1, 1, 9, 2]
            filters: [9, 1, 13, 2]
            kpis:    [1, 2, 5, 6]
            hero:    [5, 2, 13, 11]
            context: [1, 6, 5, 11]
            footer:  [1, 11, 13, 13]
        placements:
          - id: page_title
            region: header
            kind: textbox
            text: "Foster Placement Rate is 2.7% across 600 intake events (2024-2025 baseline)"
            purpose: "State the baseline headline before any chart."
          - id: year_slicer
            region: filters
            kind: slicer
            field_bindings: Date[Year]
            slicer_type: dropdown
            slot: 1
            of: 3
          - id: region_slicer
            region: filters
            kind: slicer
            field_bindings: Centre[Region]
            slicer_type: dropdown
            slot: 2
            of: 3
          - id: species_group_slicer
            region: filters
            kind: slicer
            field_bindings: Animal[Species Group]
            slicer_type: dropdown
            slot: 3
            of: 3
          - id: kpi_intake_count
            region: kpis
            kind: cardVisual
            purpose: "How many animals entered BC SPCA care?"
            field_bindings: Animal Intakes[Intake Count]
            color_strategy: measure_match
            slot: 1
            of: 3
          - id: kpi_animals_fostered
            region: kpis
            kind: cardVisual
            purpose: "How many intake events experienced foster placement?"
            field_bindings: Animal Intakes[Animals Ever Fostered]
            color_strategy: measure_match
            slot: 2
            of: 3
          - id: kpi_placement_rate
            region: kpis
            kind: cardVisual
            purpose: "What percentage of intake events were fostered?"
            field_bindings: Animal Intakes[Foster Placement Rate (%)]
            color_strategy: measure_match
            slot: 3
            of: 3
          - id: hero_rate_trend
            region: hero
            kind: lineChart
            purpose: "How has the baseline Foster Placement Rate moved month over month across the full 2024-2025 range?"
            field_bindings: { Category: Date[Month Name], Y: Animal Intakes[Foster Placement Rate (%)] }
            color_strategy: measure_match
          - id: preview_region
            region: context
            kind: barChart
            purpose: "Preview: which regions lead on foster placement? (full story on page 3)"
            field_bindings: { Category: Centre[Region], Y: Animal Intakes[Foster Placement Rate (%)] }
            sort_policy: value_desc
            color_strategy: gradient
            slot: 1
            of: 2
          - id: preview_species
            region: context
            kind: barChart
            purpose: "Preview: which species groups lead on foster placement? (full story on page 4)"
            field_bindings: { Category: Animal[Species Group], Y: Animal Intakes[Foster Placement Rate (%)] }
            sort_policy: value_desc
            color_strategy: gradient
            slot: 2
            of: 2
          - id: footer_caption
            region: footer
            kind: textbox
            text: "Source: ShelterBuddy (Prototype-mode mock data). Species Group mapping incomplete -- 'Other' is a pending-mapping placeholder, not a finished category. No Phase 1 KPI thresholds exist yet; this page reports the baseline only."
            purpose: "Carry the mandatory data-quality caveats visibly on the landing page."
        space_audit:
          content_cell_count: 132
          placed_cell_count: 132
          empty_cell_pct: 0
          unplaced_regions: []
          largest_region: { name: hero, pct_of_content: 55 }
          balance_rationale: "Hero trend is the explicit Variant-A hero and the chart that proves the KPI strip's headline; KPI cards carry sparkline context (not bare values), and the preview row bridges into pages 3-4 rather than leaving dead space."

    - name: "Time Analysis"
      role: detail
      archetype: Narrative
      layout_variant: B
      variant_rationale: "Two genuinely distinct sub-questions -- long-run/YoY trend and seasonality -- each deserves its own beat rather than being crammed into one chart."
      page_background: "#F0F0F0"
      layout_contract:
        canvas: { width: 1920, height: 1080, margin: 32, gutter: 24, snap: 8 }
        grid:
          columns: 12
          rows: 12
          regions:
            header:  [1, 1, 9, 2]
            filters: [9, 1, 13, 2]
            beat1:   [1, 2, 13, 7]
            beat2:   [1, 7, 13, 12]
            nav:     [1, 12, 13, 13]
        placements:
          - id: page_title
            region: header
            kind: textbox
            text: "Foster utilization has held steady year over year, with a consistent kitten-season peak"
            purpose: "State the page thesis before any chart."
          - id: year_slicer
            region: filters
            kind: slicer
            field_bindings: Date[Year]
            slicer_type: dropdown
            slot: 1
            of: 3
          - id: region_slicer
            region: filters
            kind: slicer
            field_bindings: Centre[Region]
            slicer_type: dropdown
            slot: 2
            of: 3
          - id: species_group_slicer
            region: filters
            kind: slicer
            field_bindings: Animal[Species Group]
            slicer_type: dropdown
            slot: 3
            of: 3
          - id: beat1_trend
            region: beat1
            kind: lineChart
            purpose: "Has the Foster Placement Rate changed year over year and quarter over quarter?"
            field_bindings: { Category: Date[Quarter], Y: Animal Intakes[Foster Placement Rate (%)] }
            color_strategy: measure_match
            insight_basis: "YoY delta annotation comparing 2024 vs 2025 full-year rate."
          - id: beat2_season
            region: beat2
            kind: columnChart
            purpose: "Is there a seasonal (kitten-season) pattern in foster placement?"
            field_bindings: { Category: Date[Season], Y: Animal Intakes[Foster Placement Rate (%)] }
            sort_policy: natural_order
            color_strategy: measure_match
            insight_basis: "Peak-season callout naming the highest-rate season."
          - id: nav_footer
            region: nav
            kind: textbox
            text: "Source: ShelterBuddy (Prototype-mode mock data)."
            purpose: "Source/refresh footer."
        space_audit:
          content_cell_count: 132
          placed_cell_count: 132
          empty_cell_pct: 0
          unplaced_regions: []
          largest_region: { name: beat1, pct_of_content: 45 }
          balance_rationale: "Both beats are near-equal-weight narrative chapters per Variant B; neither dominates, matching the archetype's 'each beat is a complete thought' principle."

    - name: "Geography Analysis"
      role: detail
      archetype: Narrative
      layout_variant: A
      variant_rationale: "Single clear ranking question (which Region leads) with a natural annotation panel for the gap callout and Centre spotlight -- classic 7/5 show-and-tell shape."
      page_background: "#F0F0F0"
      layout_contract:
        canvas: { width: 1920, height: 1080, margin: 32, gutter: 24, snap: 8 }
        grid:
          columns: 12
          rows: 12
          regions:
            header:  [1, 1, 9, 2]
            filters: [9, 1, 13, 2]
            anchor:  [1, 2, 8, 10]
            panel:   [8, 2, 13, 10]
            body:    [1, 10, 13, 13]
        placements:
          - id: page_title
            region: header
            kind: textbox
            text: "Foster placement varies by region -- [Top Region] leads, [Bottom Region] trails"
            purpose: "State the page thesis before any chart."
          - id: year_slicer
            region: filters
            kind: slicer
            field_bindings: Date[Year]
            slicer_type: dropdown
            slot: 1
            of: 3
          - id: region_slicer
            region: filters
            kind: slicer
            field_bindings: Centre[Region]
            slicer_type: dropdown
            slot: 2
            of: 3
          - id: species_group_slicer
            region: filters
            kind: slicer
            field_bindings: Animal[Species Group]
            slicer_type: dropdown
            slot: 3
            of: 3
          - id: region_ranked_bar
            region: anchor
            kind: barChart
            purpose: "Where is foster being utilized -- which regions lead and trail?"
            field_bindings: { Category: Centre[Region], Y: Animal Intakes[Foster Placement Rate (%)] }
            sort_policy: value_desc
            color_strategy: measure_match
          - id: region_gap_callout
            region: panel
            kind: textbox
            text: "[Top Region] leads at [rate]%, [pp] above [Bottom Region]."
            purpose: "Quantify the regional gap so the reader doesn't do arithmetic."
            insight_basis: "Top-minus-bottom Region gap, absolute and percentage points."
          - id: centre_spotlight
            region: panel
            kind: textbox
            text: "Within [Top Region], [Top Centre] shows the highest individual rate."
            purpose: "Fold Centre-level driver coverage into the annotation panel without a second chart."
            insight_basis: "Highest-rate Centre within the leading Region."
            slot: 2
            of: 2
          - id: body_text
            region: body
            kind: textbox
            text: "What this shows -> why it matters -> what to watch. No Municipality-level view yet -- deferred, no source data."
            purpose: "Narrative body: shows, matters, so-what."
        space_audit:
          content_cell_count: 132
          placed_cell_count: 132
          empty_cell_pct: 0
          unplaced_regions: []
          largest_region: { name: anchor, pct_of_content: 42 }
          balance_rationale: "Anchor ranked bar is the single anchor chart per Narrative discipline; annotation panel and body text carry the gap/Centre/caveat context without a second competing chart."

    - name: "Animal Characteristics"
      role: detail
      archetype: Narrative
      layout_variant: B
      variant_rationale: "Species and Age Group are two separately named driver stories in the Discovery Framework (Drivers #3-4); each earns its own beat rather than one crowded chart."
      page_background: "#F0F0F0"
      layout_contract:
        canvas: { width: 1920, height: 1080, margin: 32, gutter: 24, snap: 8 }
        grid:
          columns: 12
          rows: 12
          regions:
            header:  [1, 1, 9, 2]
            filters: [9, 1, 13, 2]
            beat1:   [1, 2, 13, 7]
            beat2:   [1, 7, 13, 12]
            nav:     [1, 12, 13, 13]
        placements:
          - id: page_title
            region: header
            kind: textbox
            text: "Species Group and Age Group are the strongest animal-level drivers of foster placement"
            purpose: "State the page thesis before any chart."
          - id: year_slicer
            region: filters
            kind: slicer
            field_bindings: Date[Year]
            slicer_type: dropdown
            slot: 1
            of: 3
          - id: region_slicer
            region: filters
            kind: slicer
            field_bindings: Centre[Region]
            slicer_type: dropdown
            slot: 2
            of: 3
          - id: species_group_slicer
            region: filters
            kind: slicer
            field_bindings: Animal[Species Group]
            slicer_type: dropdown
            slot: 3
            of: 3
          - id: beat1_species
            region: beat1
            kind: barChart
            purpose: "Which Species Groups drive foster demand?"
            field_bindings: { Category: Animal[Species Group], Y: Animal Intakes[Foster Placement Rate (%)] }
            sort_policy: value_desc
            color_strategy: measure_match
            insight_basis: "Caption explicitly flags 'Other' as a not-yet-mapped placeholder, not a finished category."
          - id: beat2_age
            region: beat2
            kind: barChart
            purpose: "Which Age Groups rely most on foster care?"
            field_bindings: { Category: Animal[Age Group], Y: Animal Intakes[Foster Placement Rate (%)] }
            sort_policy: value_desc
            color_strategy: measure_match
            insight_basis: "Body text notes Sex as a low-cost, untested, hypothesis-only driver -- no separate Sex chart."
          - id: nav_footer
            region: nav
            kind: textbox
            text: "Source: ShelterBuddy (Prototype-mode mock data). Species Group mapping incomplete."
            purpose: "Source/refresh + recurring data-quality reminder."
        space_audit:
          content_cell_count: 132
          placed_cell_count: 132
          empty_cell_pct: 0
          unplaced_regions: []
          largest_region: { name: beat1, pct_of_content: 45 }
          balance_rationale: "Matches Time Analysis's beat rhythm intentionally -- repetition across narrative pages builds scannability (layout.md principle 7)."

    - name: "Driver Assessment"
      role: detail
      archetype: Comparative
      layout_variant: A
      variant_rationale: "Ranking one 8-group dimension (Intake Group) against a portfolio baseline, plus a secondary Breed breakdown -- exact match to Side-by-Side's headline-variance + small-multiples shape."
      page_background: "#F0F0F0"
      layout_contract:
        canvas: { width: 1920, height: 1080, margin: 32, gutter: 24, snap: 8 }
        grid:
          columns: 12
          rows: 12
          regions:
            header:    [1, 1, 9, 2]
            filters:   [9, 1, 13, 2]
            headline:  [1, 2, 13, 6]
            multiples: [1, 6, 13, 11]
            footer:    [1, 11, 13, 13]
        placements:
          - id: page_title
            region: header
            kind: textbox
            text: "Intake Group shows the widest Foster Placement Rate spread of any driver assessed"
            purpose: "State the synthesis thesis before any chart."
          - id: year_slicer
            region: filters
            kind: slicer
            field_bindings: Date[Year]
            slicer_type: dropdown
            slot: 1
            of: 3
          - id: region_slicer
            region: filters
            kind: slicer
            field_bindings: Centre[Region]
            slicer_type: dropdown
            slot: 2
            of: 3
          - id: species_group_slicer
            region: filters
            kind: slicer
            field_bindings: Animal[Species Group]
            slicer_type: dropdown
            slot: 3
            of: 3
          - id: intake_group_variance
            region: headline
            kind: barChart
            purpose: "Which Intake Group deviates most from the overall Foster Placement Rate?"
            field_bindings: { Category: Intake Type[Intake Group], Y: Animal Intakes[Foster Placement Rate (%)] }
            sort_policy: value_desc
            color_strategy: gradient
            insight_basis: "Deviation from the portfolio-average Foster Placement Rate, zero-anchored reference line."
          - id: breed_small_multiples
            region: multiples
            kind: barChart
            purpose: "Which top Breeds show the largest gap to the portfolio average?"
            field_bindings: { Category: Animal[Breed], Y: Animal Intakes[Foster Placement Rate (%)] }
            sort_policy: value_desc
            color_strategy: gradient
            insight_basis: "Top-6 Breeds by |delta| to portfolio average, Delta% annotated per panel."
          - id: synthesis_callout
            region: footer
            kind: textbox
            text: "Across all drivers assessed, [Driver] shows the widest spread -- [X] percentage points between its highest and lowest segment."
            purpose: "Close the report's narrative arc with one synthesis sentence."
            insight_basis: "Cross-driver comparison of spread (max-min Foster Placement Rate) across Region, Species Group, Age Group, and Intake Group."
        space_audit:
          content_cell_count: 132
          placed_cell_count: 132
          empty_cell_pct: 0
          unplaced_regions: []
          largest_region: { name: multiples, pct_of_content: 45 }
          balance_rationale: "Headline variance chart and small-multiples trellis are the two co-equal zones Variant A specifies; footer closes the report's argument rather than sitting empty."

  interaction_pattern:
    drill_targets: []
    cross_filter_rules: "Filter (default) across all visuals within a page; the 3 inline slicers (Year, Region, Species Group) cross-filter every visual on their page."
  accessibility:
    alt_text_strategy: "headline+trend for line/trend charts; comparison framing for ranked bars and small multiples"
    contrast_notes: "Navy #00337F on #F0F0F0 surface and grey text on white card backgrounds both verified >= 4.5:1. Bad (#C64420) never used for anything except an explicit data-quality flag textbox -- never for performance/status judgment, since no Phase 1 thresholds exist."
  theme:
    base: "User-supplied BC SPCA Colours theme JSON, applied directly (dataColors, textClasses, good/neutral/bad, page background override) rather than adapting assets/base.json from scratch"
    user_overrides: "Preserve exactly: dataColors order, Calibri text classes, #F0F0F0 page background, good/neutral/bad triplet. Do not invent a competing palette."
```

## Model requirements
- Existing measures: `Intake Count`, `Animals Ever Fostered`, `Foster Placement Rate (%)` — all 3 already exist and cover every visual in this plan
- New measures: None required
- New calculated columns: None required
- Relationship/sort requirements: `Date[Month Name]` already has `sortByColumn: Month`; `Date[Season]` has no natural sort order defined in the model — if Winter→Spring→Summer→Fall ordering matters visually (it does, for the Time Analysis Beat 2 chart), a sort column may be needed at build time. Flagging for `powerbi-report-authoring`/`rdlc-tmdl-build-agent` to resolve, not resolved here.

## Canonical design contract
(embedded above)

## Implementation notes
- Model changes: None expected (see Model requirements); if the Season sort gap above needs fixing, that's a `rdlc-tmdl-build-agent` file edit, not a report-authoring task.
- PBIR/report authoring: Not started. Hand off to `powerbi-authoring:powerbi-report-authoring` only after this spec is approved.
- Validation: Standard checklist (PBIP/PBIR files exist, JSON parses, `definition.pbir` points to `foster analysis.SemanticModel`, page/visual counts match this plan, Desktop opens and reloads, screenshots captured).
- Desktop screenshot verification: Pending build approval.
- Publishing boundary: Local PBIP only — no Fabric publish step planned.
- Risks: (1) Mock/Prototype data only — visuals will show plausible-looking but non-production patterns; (2) Species Group "Other" bucket may visually dominate the Beat-1 Species chart on page 4 if the mock data skews that way — caption already flags this, but worth a visual gut-check once built; (3) Season sort-order gap noted above.
https://claude.ai/code/artifact/d3eb995e-ec73-4a5f-bed8-d2870157ac28?via=auto_preview