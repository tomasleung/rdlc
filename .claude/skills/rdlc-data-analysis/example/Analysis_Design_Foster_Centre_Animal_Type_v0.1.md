# Analysis Design — Foster Centre and Animal Type Utilization

**Status:** Draft — demonstration run. Not confirmed. Handoff gate not met (see bottom).
**Input BRD:** `BRD_Foster_Centre_Animal_Type_Utilization_v0.2.html`, Version 0.2, Draft for business review (Approvals §19: Technical owner signed; Business decision owner, Business definition owner, Source-system authority all **Pending**).
**Produced by:** `rdlc-data-analysis` (RDLC OS), first run of the rebuilt skill.
**ID prefix:** `CATU-` (Centre & Animal Type Utilization) — derived from this BRD's capability name, not reused from any prior example's prefix (non-negotiable rule 4/8).

---

## Part 1 — Analysis Design Brief

### Primary Business Questions
Restated verbatim from BRD §06, not reinvented:

1. How many animals are in care by month, Centre, Region, and Animal Type?
2. How many of those animals experience qualifying foster care?
3. What share of Animals in Care experience foster care?
4. Which centres and regions use foster most?
5. Which animal-type families use foster most?
6. Where are changes occurring over time?
7. How often do animals relate to multiple centres in one month?
8. What data-quality gaps affect interpretation?

### Analytical Approach
Each Animal-in-Care-month is evaluated once per Centre × Animal Type Family group: did this animal experience a qualifying foster stay (Phase 1's unmodified foster-definition logic) overlapping at least one day of the month? Organization-wide totals are computed independently from dimensional subtotals, never by summing them, because an animal can validly belong to more than one Centre or Animal Type group within the same month (BRD §7.1).

### Data Used
Reporting Month, Animal ID, Care Episode ID, Effective Centre, Effective Region, Source Animal Type ID/Label, Normalized Animal Type Family, Is Animal in Care, Is Fostered Animal, Foster Days, Unmapped Centre Flag, Unknown Animal Type Flag, Data Conflict Flag — all per BRD §07.1/§09, none added.

### Drivers
Only three — the BRD's confirmed in-scope dimensions. **Not** Species-level, Age Group, or Municipality: all three are explicitly out of scope or deferred per BRD §11 and CLAUDE.md's Known Gaps.

**Driver 1 — Time.** Story: where are changes occurring over time (BQ6)? Views: monthly trend from Jan 2006, YoY percentage-point change, current-quarter vs. YTD. Method: YoY comparison (Must tier).

**Driver 2 — Centre & Region.** Story: which centres/regions use foster most, and is that difference real or just a different animal-type mix (BQ1, BQ4, BQ7, BQ8)? Views: Centre/Region volume & utilization ranking, Centre × Type mix-adjustment, multiple-centre attribution, Unmapped Centre data-quality view.

**Driver 3 — Animal Type Family.** Story: which families rely most on foster care, and does that explain apparent Centre differences (BQ5)? Views: Type volume & utilization, Centre × Type intersection.

### Open question — flagged, not decided here
BRD §06 has no business question resembling Phase 1's "is there seasonality" (Phase 1 Q3). Per rule 8 (never silently copy a prior report's content forward), I have **not** added a seasonal-utilization view by default. This needs a decision from the Foster Manager / Animal Flow Manager: was seasonality intentionally dropped from Phase 2's scope, or is it an oversight in the BRD that should be added back? See `CATU-SEASON-01` below — left open pending that answer.

---

## Part 2 — Insight Specification Register

| ID | Business Question | Primary Measure | Comparison | Diagnostic Method (tier) | Permitted Conclusion | Prohibited Conclusion | Min. Volume | Status |
|---|---|---|---|---|---|---|---|---|
| `CATU-BASE-01` Provincial baseline | BQ1–3 | Animals in Care, Fostered Animals, % Animals in Care Fostered | Full history, Jan 2006–latest (annual grain) | — | Describe level/direction of org-wide change over the full history, not just the current period | Imply performance, capacity, or target | n/a (org-wide) | Draft — revised to require the full-history annual view, not a YTD-only snapshot |
| `CATU-TREND-01` Recent trend | BQ6 | All three core KPIs, monthly | YoY | YoY comparison (**Must**) | Describe direction/magnitude of change | Attribute cause without further decomposition | n/a | Draft |
| `CATU-SEASON-01` Seasonal utilization | *not in BRD §06* | — | — | Multi-year seasonal baseline (**Must/Prefer**) | — | — | — | **OPEN — confirm with business owner before drafting** |
| `CATU-CENTRE-01` Centre volume & utilization | BQ1, BQ4 | 3 core KPIs + Foster Days, by Centre | Prior year, top-N share | Top-N / Pareto share (**Must**) | Describe which centres show comparatively higher/lower volume & rate, within volume safeguards | Label a centre best/worst; infer capacity or performance (BRD §13.1) | BRD §8.1 tiers (30+/10–29/<10) | Draft |
| `CATU-CENTRE-TREND-01` Centre long-term trajectory (filtered) | Extends BQ4 + BQ6 | 3 core KPIs, annual grain, one selected Centre | Full available history for that Centre, then recent 5-year zoom | n/a for the long view; YoY (**Must**) for the 5-year zoom | Describe how one centre's own trajectory moved over its available history and recent years | Compare one centre's trajectory shape directly against another's (that's `CATU-CENTRE-01`'s job); treat a centre's earliest years as representative of current operations if boundaries changed | BRD §8.1 tiers, per year-cell | Draft — see data-quality requirement below |
| `CATU-REGION-01` Region volume & utilization | BQ4 | Same, rolled to Region | Prior year | Top-N / Pareto share (**Must**) | Same as above, at Region grain | Same as above | BRD §8.1 tiers | Draft |
| `CATU-TYPE-01` Animal Type volume & utilization | BQ5 | 3 core KPIs by Normalized Animal Type Family | Prior year | Top-N / Pareto share (**Must**) | Describe which families show higher/lower utilization, within safeguards | Infer a family is "harder to place" or imply outcome/capacity | BRD §8.1 tiers | Draft |
| `CATU-CENTRETYPE-MIX-01` Centre × Type composition | Extends BQ1, BQ4, BQ5 | % of Animals in Care by Animal Type Family, per Centre (composition, sums to 100%) | None (descriptive) | Composition / 100% stacked bar (**Must** — `analysis-insight-standard.md` §5.1) | Describe how animal-type composition varies by centre | Imply a centre's composition reflects intent, policy, or performance | BRD §8.1 tiers | Draft — new, foundational for `CATU-CENTRETYPE-01` |
| `CATU-TYPE-TREND-01` Animal Type long-term trend | Extends BQ5+BQ6 | % Animals in Care Fostered by Normalized Animal Type Family, annual grain | Full history, Jan 2006–latest, then recent 5-year zoom; all families shown together (small category count, no filter needed unlike Centre) | n/a for the long view; YoY (**Must**) for the 5-year zoom | Describe how each family's trajectory moved, and whether the relative gap between families has widened/narrowed/stayed stable | Imply causation for why a family's rate changed; treat a widening gap as evidence of outcome, capacity, or performance differences (out of scope) | BRD §8.1 tiers, per year-cell | Draft — new |
| `CATU-CENTRETYPE-01` Centre × Type drivers | BQ1, BQ4, BQ5 | Centre × Type intersection | Mix-adjusted vs. raw; cross-centre, single period — scatter of mix share vs. rate, bubble = volume | Kitagawa/Das Gupta rate decomposition (**Must**); direct standardization (**Must**, reliable cells) or indirect/SMR (**Prefer**, sparse cells) | State whether a Centre difference is mix-driven or a true within-type rate difference | Conclude a centre is better/worse at fostering without mix-adjustment; apply direct standardization below the volume floor; fit a trend/regression line implying a confirmed relationship | BRD §8.1 tiers, per cell | Draft — visual revised to add cross-centre scatter alongside the single-centre decomposition bar |
| `CATU-CENTRETYPE-TREND-01` Centre × Type split over time (filtered) | Extends BQ4+BQ5+BQ6 | Mix effect + rate effect, annual grain, one selected Centre | Full available history for that Centre, then recent 5-year zoom — same pattern as `CATU-CENTRE-TREND-01` | Kitagawa decomposition (**Must**), repeated per year | Describe whether a centre's gap from the provincial average is becoming more mix-driven or more rate-driven over its own history | Compare the trajectory's shape directly against another centre's; treat years before the data-start badge as zero or typical | BRD §8.1 tiers, per year-cell | Draft — new |
| `CATU-TRANSFER-01` Multiple-centre attribution | BQ7 | Animals attributed to >1 Centre/month | n/a (structural disclosure) | n/a — overlap count, not a statistical method | Disclose overlap count/share with non-additivity warning | Sum Centre-level distinct counts into a province-wide total | n/a | Draft |
| `CATU-TRANSFER-DIR-01` Directional transfer flow | Extends BQ7 | Transfer event count, by From-Centre → To-Centre pair | n/a (structural disclosure, like `CATU-TRANSFER-01`) | Sequential effective-dated-segment ordering + change-flag (**Must** — derived/DAX-buildable, not a statistical method; grounded in BRD §7.4 effective-dated Location History and the §19 "Centre transfer, mid-month Centre change" UAT case) | Describe transfer frequency and the most common From→To centre pairs | Imply a centre is a "source" or "destination by design"; infer capacity, performance, or cause for any transfer; double-count a transfer already disclosed by `CATU-TRANSFER-01`'s overlap warning as a second, separate event | n/a | Draft — new, confirms a from/to field is derivable from existing effective-dated Centre segments, no new source data required |
| `CATU-CHANGE-01` YoY change decomposition | BQ6 (Centre/Type level) | 3 core KPIs | YoY, by Centre/Type | Kitagawa decomposition (**Must**); naive contribution ratio / PVM bridge with explicit residual (**Must**) | State which Centre/Type groups contributed most to a change, residual disclosed | Treat the contribution split as a causal explanation | n/a | Draft |
| `CATU-DQ-01` Centre/Type completeness | BQ8 | % records with resolved Centre + Type | vs. BRD §14.1's 99% floor | n/a — direct rate vs. stated threshold | State completeness rate and whether it meets the 99% floor | Exclude Unmapped/Unknown from province-wide totals (BRD keeps them in) | n/a | Draft |
| `CATU-RESTATEMENT-01` Historical corrections | §14.3 | Snapshot-vs-snapshot diff, by month/measure | Current reprocess vs. prior published snapshot | Leave-one-out / snapshot robustness check (**Must**) | Disclose which months/measures changed and why | Restate without logging | n/a | Draft |

**`CATU-CENTRE-TREND-01` data-quality requirement:** each centre's long-term view must show a "data available from [year]" indicator rather than assuming full 2006 coverage — this is the concrete, checkable answer to whether Centre identity is stable/traceable that far back, replacing what would otherwise be a blanket blocked assumption. Phase 1's qualitative "lifecycle phase" bands (Rapid Growth / Transition / Stable / High Utilization) were a manual read of the shape, not a formal change-point algorithm — don't imply this view auto-detects phases unless that capability is separately built and confirmed.

---

## Handoff gate check (not met)

- [x] Every Driver traces to a BRD-confirmed in-scope dimension
- [x] Every register entry answers a BRD-confirmed business question (except the flagged open item)
- [x] Every diagnostic method is Must/Prefer tier
- [x] Every entry states permitted/prohibited conclusions
- [x] Every ranking/rate states its minimum-volume safeguard
- [x] No Green/Yellow/Red threshold language used — BRD §13.1 bans it, rule 6 applied
- [ ] **`CATU-SEASON-01` is open, not resolved**
- [ ] **Business/decision owner has not confirmed this Brief or Register — BRD itself is still Pending on 3 of 4 approvals**

Not ready to hand to `rdlc-coach-semantic-model`. Two things block it: the seasonality question above, and actual sign-off — on this artifact, and on the BRD it's built from.
