# TDD-Style Verification — Methodology

A reusable verification pattern for any `rdlc-tmdl-build-agent` build: **predict expected values before opening Power BI Desktop, then confirm the live model produces them.** This is the acceptance-test layer, sitting above static validation — it proves the built model is not just syntactically valid, but actually computing correctly end-to-end.

This file holds the *methodology* only — reusable for any project. The actual filled-in prediction/result for Foster Analysis lives in `05-validation/` at the project root, not here.

---

## Why this matters — what static validation alone cannot prove

`static-validation-checklist.md` catches structural/syntactic issues from reading TMDL text: missing descriptions, forbidden data types, orphan comment blocks, referential integrity in the source CSVs. None of that proves the *live* model, once opened, actually computes the right numbers — a relationship could point the wrong direction, a measure could reference the wrong column, an M query could silently drop rows. Only opening the real model and checking real values proves the whole chain works.

## The method

1. **Before opening Desktop**, from the known mock data generation parameters (row counts, seeded distributions), state the expected value for each core KPI/measure. Write these down *before* looking at the live model — a prediction made after seeing the result isn't a real test.
2. **Open the model** in Power BI Desktop. Build a simple table/card visual with the core measures.
3. **Compare** — record actual vs. predicted for each measure.
4. **Any mismatch is a real signal**, not necessarily a bug in the model — check in this order: (a) was the prediction itself wrong (bad math from the mock data parameters)? (b) is a relationship pointing the wrong way or using the wrong columns? (c) is a measure's DAX referencing the wrong base column? A mismatch after a build has passed static validation usually means something in the semantic layer, not the syntax layer.

## What counts as a good prediction target

Pick measures/values where:
- The expected value is derivable from known mock data generation parameters (not guessed)
- Getting it right requires the *whole* chain to work — CSV → M query → relationships → measure DAX — not just one link
- A wrong value would be immediately, obviously wrong (not something that could plausibly be "close enough")

Ratio/derived measures (like a placement rate) are especially good targets — they can only compute correctly if both underlying additive measures are already correct AND the DIVIDE logic is right.

## Template for recording a result

```markdown
## [Project Name] — Build Verification, [date]

| Measure | Predicted | Actual | Match |
|---|---|---|---|
| [measure 1] | [value, with reasoning for how it was derived] | [value from live model] | ✓ / ✗ |
| ... | | | |

**Build source:** [confirmed Table Definitions doc version]
**Data source:** [mock data folder / generation parameters, e.g. seed used]
**Conclusion:** [pass/fail, and if fail, what was investigated]
```

See `05-validation/` for the first completed instance of this template.
