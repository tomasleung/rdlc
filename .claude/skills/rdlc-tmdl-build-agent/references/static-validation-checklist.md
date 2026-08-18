# Static Validation Checklist

Run this checklist against every authored TMDL file set before declaring a build complete, per `SKILL.md` rule 3. Every item here is checkable from the TMDL text itself (plus, where noted, the source CSV files) — **no live engine or MCP connection required.** Items that genuinely need a processed/live model (cardinality, row counts, actual compression stats) are explicitly excluded and listed at the bottom as Verify-Mode-only.

## Source Provenance
| Source | URL | Last Verified |
|---|---|---|
| Tabular Editor / Microsoft Analysis-Services — Best Practice Analyzer Rules | https://github.com/microsoft/Analysis-Services/blob/master/BestPracticeRules/BPARules.json | 2026-08-17 |

If asked to update this file, re-fetch the URL above against the current rule set, diff, and report changes before editing.

---

## A. Structural / Error Prevention

- [ ] Every `///` comment block is immediately, directly followed by a table/column/measure/hierarchy declaration — zero blank lines in between. A floating `///` block followed by a blank line causes a hard parse failure (`InvalidLineType: Empty!`) when opened in Power BI Desktop. This is one of the two highest-value checks in this entire list — it causes total load failure, not just a lint warning.
- [ ] **No `///` comment (or `description:` property) appears directly above any `relationship` declaration.** Relationships do not support a description property in TOM — this causes a hard parse failure (`Property 'description' is unknown and is not expected in the situation it appears`). Relationship rationale belongs in the fact table's own description instead.
- [ ] Every data column has a `sourceColumn` mapping to the partition source.
- [ ] Every object requiring an expression (measure, calculated column, calculated table partition) actually has one.
- [ ] Both sides of every relationship share the same `dataType`.
- [ ] No object name contains invalid characters (emojis, tabs, line breaks).
- [ ] No description contains invalid characters.
- [ ] No object name starts or ends with a space.
- [ ] No composite keys anywhere — every relationship uses a single column on each side.
- [ ] No surrogate primary key column on any fact table.

## B. Naming

- [ ] No `CamelCase`, `snake_case`, or `UPPER_CASE` on any visible table, column, or measure name.
- [ ] No `Fact_`/`Dim_`/`FACT_`/`DIM_`/`STG_` prefix on any visible table name.
- [ ] Fact table business names are plural; dimension table business names are singular.
- [ ] First letter of every visible object name is capitalized.
- [ ] Partition name matches its table name for single-partition tables.
- [ ] No trailing/leading whitespace on any object name.

## C. Columns

- [ ] No column uses `Double` — `Decimal` or `Int64` used instead.
- [ ] All key and foreign key columns are hidden (`isHidden`).
- [ ] Every column feeding a measure's aggregation is hidden.
- [ ] `SummarizeBy: None` set on every non-aggregatable numeric (IDs, postal codes, year, month number).
- [ ] `dataCategory` set on geographic columns (city/country/continent/postal code) and lat/long pairs.
- [ ] Relationship columns use `Int64` where possible, not `String`.
- [ ] `isAvailableInMdx: false` set on hidden columns not used in sort-by/hierarchy/variation.
- [ ] Month-name (or similar) text columns have `sortByColumn` configured — never left to sort alphabetically.
- [ ] **Do NOT set `isKey = true` on dimension primary keys** — documented MS-vs-BPA conflict, MS governs (see `modeling-and-ai-readiness-standards.md`).

## D. Measures

- [ ] Every measure has a `formatString`.
- [ ] Every measure has a `///` description.
- [ ] No two measures share identical DAX (duplicate check).
- [ ] No measure is a direct, bare reference to another measure.
- [ ] No measure uses `EVALUATEANDLOG`.
- [ ] No measure uses `IFERROR` (prefer `DIVIDE`/conditional logic).
- [ ] No measure uses `1-(x/y)` or `1+(x/y)` syntax.
- [ ] Every division uses `DIVIDE()`, not raw `/`.
- [ ] Any `INTERSECT()` for a virtual relationship should be `TREATAS()` instead.
- [ ] DAX column references are table-qualified (`'Table'[Column]`); DAX measure references are NOT table-qualified (`[Measure]`).
- [ ] Measures are distributed across their relevant tables, not concentrated in one catch-all table.

## E. Relationships

- [ ] Every table has at least one relationship (except a genuine utility/calculation-group table).
- [ ] No bi-directional or many-to-many relationship exists unless explicitly required by the confirmed Table Definitions document.
- [ ] Bi-directional + many-to-many relationships combined are under 30% of total relationship count.
- [ ] No inactive relationship exists without a corresponding `USERELATIONSHIP()` reference in at least one measure.
- [ ] No two fact tables relate to the same dimension through different, non-conformed key columns.

## F. Date Table

- [ ] A table with `dataCategory: Time` exists in the model.
- [ ] The date table's date range is contiguous — no missing days.
- [ ] No leftover auto-generated `LocalDateTable_*` or `DateTableTemplate_*` tables — auto-date disabled.
- [ ] Month-name column (if present) is a `sortByColumn` on the numeric month.

## G. Descriptions & Documentation

- [ ] Every visible table, column, and measure has a `///` description (never a `description:` property).
- [ ] No description merely restates the object's name.
- [ ] AI/Copilot-facing descriptions front-load the most important disambiguating detail within the first 200 characters.

## H. Maintenance / Hygiene

- [ ] No hidden column exists that isn't referenced by any measure, relationship, hierarchy, or sort-by.
- [ ] No hidden measure exists that isn't referenced by any other DAX expression.
- [ ] No data source (named expression) is left unreferenced by any partition.
- [ ] No empty perspective (if perspectives are used — out of scope for Phase 1).

## I. Referential Integrity (Prototype/CSV mode — checkable directly against source files)

- [ ] Every foreign key value in the fact table's source CSV resolves to an existing row in the corresponding dimension CSV — zero orphans. (Re-use the same validation pattern as `rdlc-mock-data-generator`'s own handoff gate.)

---

## Excluded — Verify Mode / Live Engine Only (do NOT attempt statically)

These require a processed, live model and its VertiPaq storage annotations — meaningless against static TMDL text:

- Bi-directional relationships against high-cardinality columns (`Vertipaq_Cardinality` annotation)
- Long-length high-cardinality text column detection (`LongLengthRowCount` annotation)
- Large-table partitioning threshold (`Vertipaq_RowCount` annotation)
- DateTime-with-time-component detection (`DateTimeWithHourMinSec` annotation — needs actual loaded data)
- Actual measure evaluation errors (`INFO.VIEW.MEASURES()` error column — needs a live DAX query)

If any of these matter for a specific build, invoke Verify Mode (Trigger A in `SKILL.md`) rather than guessing.
