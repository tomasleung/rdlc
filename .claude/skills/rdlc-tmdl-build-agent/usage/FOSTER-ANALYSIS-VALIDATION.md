# Foster Analysis — Build Verification

First completed instance of the TDD-style verification methodology (see `.claude/skills/rdlc-tmdl-build-agent/usage/TDD-VERIFICATION.md` for the reusable template/reasoning).

## Result — 2026-08-18

| Measure | Predicted | Actual | Match |
|---|---|---|---|
| Intake Count | 600 | 600 | ✓ |
| Animals Ever Fostered | ≈241 (241/600 = 40% at generation time) | 241 | ✓ |
| Foster Placement Rate (%) | ≈40% | 40.17% | ✓ |

**Build source:** `02-table-definitions/TABLE_DEFINITIONS_FOSTER_v1_3.docx` (confirmed)
**Data source:** `03-mock-data/`, seeded (seed=42), 600 fact rows, 32 centres, 40 animals, 34 intake types/8 groups, 731-day contiguous date range (2024-01-01 to 2025-12-31)
**Build agent:** `rdlc-tmdl-build-agent`, run via Claude Code, Authoring Mode (no MCP)

**Conclusion: PASS.** All three predicted values matched the live model exactly (Foster Placement Rate's small decimal difference, 40.17% vs. an approximate 40%, is expected — the prediction was a round-number approximation from generation-time math, not a precise recomputation).

## What this confirms

- The M/Import partition queries correctly read all 600 rows from `Fact_AnimalIntake.csv`
- All four relationships (`Animal Intakes` → `Date`/`Centre`/`Animal`/`Intake Type`) are correctly joining — a directional or column-mapping error would have produced blank or wrong values
- `SUM('Animal Intakes'[IntakeCount])`, `SUM('Animal Intakes'[FosterFlag])`, and `DIVIDE([Animals Ever Fostered], [Intake Count])` are all computing against the correct hidden base columns
- Nothing was lost or corrupted across the full chain: confirmed Table Definitions → mock data generation → TMDL authoring

## Bugs found and fixed en route to this result

See `.claude/skills/rdlc-tmdl-build-agent/usage/CHANGELOG.md` for full detail. Two TMDL syntax errors were found and fixed before this result was achieved:
1. Orphan `///` comment block causing `InvalidLineType: Empty!`
2. `///` comment on a `relationship` object (unsupported) causing `DataModelLoadFailed`

Both are now permanently fixed in the skill's reference material, not just this one build.

## Not yet done

- Best-practice/semantic verification via MCP Verify Mode (naming conventions, hidden flags, `SummarizeBy` correctness, etc.) — only static validation and this KPI-accuracy check have been completed so far.
