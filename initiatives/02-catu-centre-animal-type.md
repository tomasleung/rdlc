# Initiative 02 — Centre & Animal Type Utilization (CATU)

This is the repo's actual "Phase 2" (see naming note in `PROJECT_STATUS.md` — don't reuse this number for a different initiative).

**Status:** BRD at v0.3, reviewed 2026-11-18 — strong document, several concrete fixes needed before business review.

## Key artifacts

- `.claude/skills/rdlc-grill-coach/example/BRD_Foster_Centre_Animal_Type_Utilization_v0.1–0.3.html` (confirmed stages through v0.2; v0.3 now also sitting in `01-inputs/BRD_Foster_Centre_Animal_Type_Utilization_v0.3.html`, suggesting it's staged for the Semantic Model Design handoff)
- `.claude/skills/rdlc-data-analysis/example/Analysis_Design_Foster_Centre_Animal_Type_v0.1.md` (includes `CATU-TRANSFER-DIR-01`, added for the From→To transfer-direction question)
- `.claude/skills/rdlc-mock-report-design/example/Foster_Centre_Animal_Type_Report_Mock_v0.6.html`

## v0.3 content summary

Disambiguated the utilization measure from Phase 1's: renamed to **% Animals in Care Fostered** (Fostered Animals ÷ **Animals in Care**, the full active population that month) to avoid colliding with Phase 1's `% Animals in Foster` (÷ **Intake Animals**, new arrivals that month). Reconstructs an effective-dated history from Source (intake), Status, and Shelter Location (kennel) histories to derive Effective Centre, Region, and a normalized Animal Type Family. Real worked examples (3 animals, real IDs/timestamps) and real profiling stats throughout (99.93% status-to-source link rate, 11.8% of episodes span more than one centre, <0.1% true animal-family changes). Section 10 (schema/SCD mechanics) explicitly reframed as non-binding input to Semantic Model Design, not a settled BRD requirement — good RDLC boundary discipline.

## Review findings (2026-11-18) — not yet actioned

1. **Dangling citation, high priority.** Appendix D cites *"Foster Phase 1 BRD, Version 2.0, October 2026"* as the source for the `% Animals in Foster ÷ Intake Animals` measure. **No v2.0 exists anywhere in this repo** — only `BRD_Foster_Analysis_Phase1_Counters_v1_1.docx`, which *defers* that exact measure. See initiative 01's open thread — this is the same unresolved question showing up in a second document.
2. **Four more dangling references** — cited as real source documents, not present in the repo: `Foster_Episode_Definition_v0_1.md`, `Care Episode, Centre and Animal Type Definition v0.1` (the claimed basis for all of Appendix F), `Fostered_Animals_Counting_Logic_v0_1.docx`, `Foster_Placement_Count_Counting_Logic_v0_1.docx`.
3. **Structural duplication.** Section 7.2/7.4 and Appendix F.3–F.5 describe the same care-episode/centre/type rules twice, at business and technical depth. Appendix A and Appendix F.8 are the *same three worked examples* (Animals 620790, 605768, 607033) repeated almost verbatim. Risk: if one copy is corrected later, the other can silently drift out of sync.
4. **Undocumented window gap.** Reporting window is stated as "January 2006 onward," but all profiling evidence is sourced "2005 to Sep 2026" — never explained why 2006 is the cutoff if 2005 data exists.
5. **Missing a caveat Phase 1 already learned.** Phase 1's own BRD flags the multi-decade historical window as a real risk ("a materially longer range than assumed, increasing the chance of an undocumented structural break") and requires a stability check before trusting it. This BRD reconstructs history over the same range from the same raw histories but doesn't carry that caveat forward.

**Resolved, not a problem:** `BRD_AnimalFlow_LiveCapacity_Reporting_v1.0.pdf` (previously an unexplained file in this project) is accounted for — Appendix D names it as the template this BRD's governance section was borrowed from.

## Open

`CATU-SEASON-01` register entry (in the Analysis Design) is still unresolved/unconfirmed — not mocked, deferred.

## Next

Resolve the Phase 1 BRD v2.0 question (shared with initiative 01) before anything else — it affects whether Section 08's disambiguation callout itself needs correcting. Then: decide whether to collapse the Appendix A/F duplication, add the missing reference documents (or remove the citations), explain the 2005/2006 window gap, and consider adding the structural-break caveat. Resolve `CATU-SEASON-01`, get the v0.6 mock formally signed off, then move to `rdlc-coach-semantic-model`.
