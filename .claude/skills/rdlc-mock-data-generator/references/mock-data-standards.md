# Mock Data Standards — Reasoning Anchors

Reasoning anchors for generating useful, realistic synthetic data. Apply contextually — not a mechanical checklist. The goal is data that meaningfully exercises the real model, not just data that satisfies the schema.

---

## 1. Referential Integrity

Every foreign key in a generated fact row must point to a row that actually exists in the corresponding dimension CSV. Generate dimensions first, always. When generating the fact table, sample foreign keys *only* from the already-generated dimension key sets — never generate a plausible-looking key value independently (e.g., never invent `C099` for Centre ID unless a dimension row with that exact key was actually written).

## 2. Structural Fidelity Over Data Fidelity

The schema (columns, order, types) must be an exact match to the confirmed Table Definitions document — this is non-negotiable per `SKILL.md`. The *values* are where creative, realistic judgment belongs. A prototype model built on this data should behave structurally identically to the eventual production model; only the numbers and names differ.

## 3. Realistic Distributions, Not Naive Uniform-Random

Uniform-random values across every row make for weak prototypes — they don't exercise seasonality, skew, or edge cases the real model needs to handle well. Apply known real-world patterns when they're statable:

- **Seasonal/time-based skew**: if the domain has a known seasonal pattern (e.g., a "kitten season" spike in spring/summer intakes for an animal welfare model), reflect that in fact-table date distribution rather than spreading rows evenly across all dates.
- **Categorical skew**: dimension categories are rarely evenly used in reality (e.g., a handful of intake types or centres typically account for a disproportionate share of activity). Bias fact-table foreign key sampling accordingly rather than sampling uniformly across every dimension row.
- **Rate-like flags**: a flag measure (e.g., a 0/1 indicator) should reflect a plausible real-world rate, not a 50/50 split, unless the human has stated a specific rate to test. State the rate you chose and the reasoning.
- **Correlated columns**: when two columns are known to be related in the real world (e.g., an age-group category correlating with a seasonal intake pattern), let the mock data reflect that correlation rather than generating each column fully independently — this is what makes a prototype actually useful for testing driver-analysis style reports.

## 4. Volume Calibration by Purpose

Two different defaults depending on stated purpose:

- **CI/CD test fixture**: favor small, fast, deterministic volumes — enough rows to exercise every relationship and every category at least once, not enough to slow down a pipeline run. Typically dozens to low hundreds of fact rows, not thousands.
- **Prototype / stakeholder demo**: favor larger, more realistic-looking volumes — enough that charts, trends, and driver comparisons look meaningful rather than sparse. Still bounded (thousands, not millions) since this is a prototype, not a load test.

If the human hasn't stated which purpose applies, ask before recommending a default — the right volume differs materially between the two.

## 5. Reproducibility

When generating data programmatically, use and state a fixed random seed. This lets a generation run be reproduced exactly on request (e.g., "regenerate the same fixtures, add one more centre") rather than producing a different random dataset every time.

## 6. Synthetic-Data Safety

Never use, approximate, or reference real source-system records. Mock data should be obviously synthetic — plausible in shape and pattern, but not an attempt to reconstruct or resemble actual real-world entities, names, or values from the source system the eventual production data will come from. This is a data-generation task, not a data-anonymization task — don't start from anything real and obscure it; generate from nothing.

## 7. CSV as an ETL-Facing Artifact, Not a Business-Facing One

Generated CSVs use Technical Names (matching the confirmed Table Definitions document's `sourceColumn`-mapped names), not Business Names. These files exist to be read by a Power BI M/Import partition query as a Prototype-mode data source — the Business Name / display-name layer is applied later, inside the semantic model itself, not in the CSV.

---

## Further Reading

- Kimball Group — dimensional modeling test data considerations (general reference, not mock-data-specific): https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/
- RFC 4180 — Common Format and MIME Type for CSV Files: https://www.rfc-editor.org/rfc/rfc4180
