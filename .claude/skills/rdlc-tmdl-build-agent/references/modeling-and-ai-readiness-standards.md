# Modeling & AI-Readiness Standards

Synthesized from Microsoft's official semantic-model-authoring guidance and Tabular Editor's Best Practice Analyzer (BPA) rule set. Where the two sources conflict, **Microsoft's guidance governs** — it reflects current, Fabric-specific platform behavior; Tabular Editor's classic SSAS/AS conventions predate Direct Lake. Conflicts are stated explicitly below, never silently resolved.

## Source Provenance
| Source | URL | Last Verified |
|---|---|---|
| MS skills-for-fabric — semantic-model-authoring (modeling-guidelines.md, naming-conventions.md, semantic-model-ai-readiness.md, direct-lake-guidelines.md) | https://github.com/microsoft/skills-for-fabric/tree/main/plugins/powerbi-authoring/skills/semantic-model-authoring | 2026-08-17 |
| Tabular Editor / Microsoft Analysis-Services — Best Practice Analyzer Rules (71 rules, 6 categories) | https://github.com/microsoft/Analysis-Services/blob/master/BestPracticeRules/BPARules.json | 2026-08-17 |

If asked to update this file, re-fetch both URLs, diff against this content, and report changes before editing.

---

## ⚠️ Documented Conflict — MS Wins

**Dimension primary key `isKey` property.**
- Tabular Editor BPA (`MARK_PRIMARY_KEYS`): set `IsKey = true` on the primary key column of a dimension table.
- MS `modeling-guidelines.md`: explicitly lists "Set `isKey = true` on the primary key column of dimension tables" under **DON'T**.
- **Resolution: follow MS — do not set `isKey`.** If Verify Mode surfaces a BPA-style lint warning on this specific rule, treat it as expected and documented, not a real defect.
- **Parked open question (2026-08-18):** `modeling-guidelines.md` states the DON'T with no stated reason, unlike its neighboring rules. An earlier hypothesis that `isKey` is deprecated legacy was checked and found incorrect — the deprecated property found was a different, older OLAP-mining-model `IsKey`, not Tabular's `Column.IsKey`. A separate question — whether `isKey`/Power BI's "Key Column" UI property benefits Copilot/AI-agent entity recognition — remains genuinely unresolved; MS's own `semantic-model-ai-readiness.md` names `isDefaultLabel` for that exact goal and does not mention `isKey` at all, which argues against `isKey` being the relevant AI-readiness lever, but this has not been independently confirmed. **Do not treat "MS wins" as backed by a verified rationale for this specific rule** — it's followed on the strength of it being MS's current, explicit instruction, not because we understand or have confirmed the underlying reasoning. Revisit if AI-readiness work is prioritized later — start with `isDefaultLabel` (see §10), not `isKey`.

No other conflicts were found between the two sources across all 71 BPA rules reviewed; the rest are complementary reinforcement of the same principles (star schema, no floating point, hide FKs, `DIVIDE` over `/`, etc.).

---

## 1. Core Modeling Principles

- **Consistency over perfection** — when extending an existing model, match its established patterns first.
- **Star schema, always** — single fact table, denormalized dimensions, one-column relationship keys, one-to-many. Deviate only for a strong, explicit requirement. Avoid snowflaking.
- **Explicit measures, never implicit aggregation** — every aggregated base column gets a corresponding named DAX measure; the base column is then hidden. Recommend enabling the model property `discourageImplicitMeasures`.
- **Lean models** — include only what's needed for analysis; every column costs memory whether used or not. High-cardinality columns (GUIDs, raw transaction IDs, unsplit DateTime, composite string keys) are the biggest memory cost.

## 2. Table Rules

**DO**: create the partition/source query first; manually create all columns with correct types matching the source; default to M/Import mode unless Direct Lake is the confirmed target; add a table-level `description` documenting purpose and grain; ensure every table has at least one relationship (except genuine utility tables).

**DON'T**: use Power Query M for Direct Lake tables; create per-table shared named expressions (share one); leave a table orphaned from the model without explicit reason.

| Partition source type | Storage mode | Use |
|---|---|---|
| `MPartitionSource` (`= m`) | Import/DirectQuery | Default; CSV/Prototype mode uses this |
| `EntityPartitionSource` (`= entity`) | Direct Lake | Production mode, OneLake-backed |
| `CalculatedPartitionSource` (`= calculated`) | Import | Calculated tables only (e.g., a measures-only utility table) |

## 3. Column Rules

**DO**:
- `sourceColumn` required on every data column, mapping to the partition source.
- `dataType` required (except calculated columns, inferred from DAX).
- Efficient types: `Int64` for keys/identifiers, `Decimal` for currency/precise numbers (4 decimal digit limit), `String`, `DateTime`, `Boolean`.
- **Never `Double`** — roundoff errors, worse compression. Use `Decimal` or `Int64`.
- Hide technical columns: keys, foreign keys, system columns, and any column aggregated by a measure.
- **Set `isAvailableInMdx: false` on every hidden column** — unless it's the source of another column's `sortByColumn` (e.g., a numeric `Month` column backing a `Month Name` sort), in a hierarchy, or in a variation. Confirmed via live MCP best-practice review (2026-08-18) and independently by Tabular Editor BPA as a real, actionable gap when omitted — applies to nearly every hidden key/FK/base-measure column in a typical build. Note: this only affects MDX query tools (e.g., Excel PivotTables, legacy SSRS) — it does not affect visibility to Power BI report authors, which is controlled separately by `isHidden`.
- Split combined DateTime into separate Date/Time columns — a DateTime with time precision creates near-unique cardinality.
- `SummarizeBy: None` on non-aggregatable numerics — IDs, phone numbers, postal codes, year, month number, day of week.
- `dataCategory` for geographic columns (`City`, `Country`, `Continent`, `PostalCode`) and lat/long pairs.
- `sortByColumn` on any text column needing non-alphabetical order (month names sorted by month number).
- Always reference columns table-qualified: `'Table Name'[Column Name]`.

**DON'T**: leave a column that doesn't map to the partition source; use `Double`; leave a foreign key visible; keep a column unreferenced by any measure/relationship/hierarchy/sort/visual; exceed ~30 visible columns per table (signals a denormalization problem — split into proper dimensions instead).

## 4. Calculated Columns — use sparingly

Prefer measures wherever the logic is expressible as an aggregation. If a calculated column is genuinely needed, prefer pushing the computation upstream to the data source instead. Never use a calculated column for something that changes with filter context (that's a measure's job), and avoid `RELATED()`-based calculated columns specifically — they compress worse than data columns.

## 5. Measures & DAX

**DO**: create an explicit measure for every aggregatable numeric column, hiding the base column; distribute measures across their relevant tables rather than one giant "Measures" table; always set `formatString`; add a `description` explaining business logic for anything non-obvious; use `displayFolder` to group related measures.

**DON'T**: put every measure in one dedicated table; leave a measure untested (Verify Mode only — see below); set a `dataType` on a measure (inferred at runtime); create two measures with identical DAX; create a measure that's just a direct reference to another measure (`[MeasureB] := [MeasureA]`).

**DAX-specific rules** (apply when authoring any measure expression):
- `DIVIDE()` over `/` for safe divide-by-zero handling.
- `TREATAS()` over `INTERSECT()` for virtual relationships.
- `REMOVEFILTERS()` over `ALL()` when the intent is "remove filters," for clarity.
- Never `1-(x/y)` or `1+(x/y)` syntax — use `VAR` + `DIVIDE()` on the full expression instead.
- Avoid `IFERROR` inside iterators — prefer `DIVIDE`/conditional logic.
- Never `EVALUATEANDLOG` in a production model.
- Column filter predicates directly in `CALCULATE`, not wrapped in `FILTER(Table, ...)`, when the predicate is a simple column comparison.

## 6. Relationships

**DO**: prefer integer keys; matching `dataType` on both sides; hide FK columns on the many side.

**DON'T**: composite keys (unsupported); surrogate keys on fact tables; bi-directional/many-to-many unless strictly required (BPA flags a model if over 30% of relationships are bi-di/many-to-many); leave an inactive relationship with no `USERELATIONSHIP()` reference anywhere (orphaned, signals incomplete modeling); multiple fact tables relating to the same dimension through different key columns without a shared conformed dimension.

**Documented exemption — String relationship keys.** `Centre ID` and `Intake Type ID` are `String`, not `Int64`, in the Foster Analysis build — a deliberate, accepted exception to the integer-key preference, not an oversight. These are ShelterBuddy's natural source-system keys; converting them to a surrogate integer would require introducing new surrogate mapping logic the confirmed Table Definitions document doesn't call for, and doing so is a grain/key redesign decision outside `rdlc-tmdl-build-agent`'s boundary — it belongs to `rdlc-coach-semantic-model`, if ever revisited. Both live MCP Verify Mode and Tabular Editor BPA will correctly flag this every time (confirmed 2026-08-18) — expected, not a defect.

## 7. Date/Calendar Table

Prefer a real source date table over a DAX-generated one. Contiguous range, no gaps. `dataCategory: Time`. Standard attributes: Year, Quarter, Month, Day, Week (optional: Day of Week, Month Name with `sortByColumn`). Disable Power BI's auto-date tables once a proper date table exists — auto-date creates a hidden `LocalDateTable_*` per date column and bloats memory.

## 8. Naming Conventions

- Readable casing with spaces — no `CamelCase`/`snake_case`/`UPPER_CASE`.
- No technical prefixes on visible names (`Fact`, `Dim`, `FACT_`, `DIM_`, `STG_`).
- Fact tables plural (`Sales`), dimension tables singular (`Customer`).
- A dimension's primary descriptive column should match the table name (`Product` inside table `Product`, not `Product Name`) — except when the model is Copilot/Data-Agent-consumed, where table-qualified names avoid ambiguity across tables.
- Measures: clear descriptive names, `#` prefix for counts (`# Orders`). Never programming-style abbreviations (`NetSls`, `TotDelCst`).
- Measure variation pattern: `[Base Name] [Period] ([Unit])` — e.g., `Total Sales (ytd)`, `Gross Margin (%)`.
- No emojis, tabs, line breaks in any object name; no leading/trailing whitespace (BPA: `TRIM_OBJECT_NAMES`, `OBJECTS_SHOULD_NOT_START_OR_END_WITH_A_SPACE`).
- First letter of visible object names capitalized (BPA: `FIRST_LETTER_OF_OBJECTS_MUST_BE_CAPITALIZED`).

**Explicit exemption — hidden base columns colliding with their own measure name.** Both MS's `naming-conventions.md` and Tabular Editor's BPA state casing rules without an explicit hidden-object carve-out, but MS's own `modeling-guidelines.md` separately requires that a measure's aggregated base column be hidden *and not share the measure's name*. When a business-friendly spaced name would collide with its own measure (e.g., a measure named `Intake Count` and a hidden base column that would otherwise also be named `Intake Count`), the hidden base column's Technical Name (unspaced, e.g. `IntakeCount`) may be used as-is for that column, rather than forcing a spaced Business Name into collision. This is a deliberate, reasoned exemption — confirmed via a live MCP best-practice review (2026-08-18) that this casing choice does not register as a defect once the underlying collision-avoidance rationale is understood, but it is not explicitly stated as an exemption in either upstream source. State the rationale in the column's `///` description when applying this exemption, so it reads as intentional, not an oversight.

## 9. Descriptions — Human vs. AI Audience

Every visible table, column, and measure needs a `description` (via TMDL `///` syntax — see `tmdl-syntax-guide.md`). But the *content* differs by audience:

- **Human-oriented**: explain business meaning, don't restate the name.
- **AI/Copilot-oriented**: **only the first 200 characters are read by Copilot** — front-load preferred usage, disambiguation against similar fields, expected grain, and units. Literal interpretation applies: `AMT` is not `Sales Amount` to a language model — spell it out.

## 10. AI-Readiness — What's TMDL-Editable vs. UI-Only

This distinction is critical to state accurately to the human — do not claim to have applied something you can't.

| Layer | Examples | Editable by this agent? |
|---|---|---|
| TOM model metadata | names, descriptions, relationships, measures, hidden flags, data types, `SummarizeBy`, `isDefaultLabel` | **Yes** — direct file edit |
| AI-specific artifacts | AI instructions, AI Data Schema selection & synonyms, Verified Answers | **No** — configured only in Power BI's "Prep data for AI" UI |

For the non-editable layer: this agent may **draft suggested content** (e.g., a paragraph of candidate AI instructions, a list of candidate synonyms) for the human to paste into the UI — but must state clearly that these are suggestions requiring manual application, never claim they've been "applied."

**Consumption-mode scoping**: ask the human whether the model needs to support reports/ad-hoc exploration only (standard star schema + explicit measures suffice) or also conversational BI/Copilot (add business-friendly naming, synonyms, AI-optimized descriptions, `isDefaultLabel` on each dimension's natural-name column, predefined measures for commonly-asked questions).

**Foundation-first principle**: a weak model cannot be rescued by AI instructions — star schema, explicit measures, and correct data types are prerequisites, not optional polish. Don't skip ahead to AI-layer suggestions if the foundational model isn't sound yet.

## 11. Static Maintenance Hygiene (checkable without a live engine)

- No unused/unreferenced hidden columns or measures.

**Documented exemption — intentionally retained "unreferenced" natural keys.** `Animal ID` and `Source Intake ID` are hidden, natural/source-system keys not referenced by any current relationship or measure — they would otherwise trip this rule. Both are deliberately retained per the confirmed Table Definitions document: `Source Intake ID` for backend/QA reconciliation against ShelterBuddy, `Animal ID` to support a future Phase 2 `DISTINCTCOUNT()`-based "# Animals" measure distinct from the event-level Intake Count. Neither BPA nor a static text scan can know this context — treat a flag on these two specific columns as expected, not a defect, unless the confirmed Table Definitions document changes to drop them.
- No duplicate measures (identical DAX under different names).
- No inactive relationship without a `USERELATIONSHIP()` reference.
- No orphaned data sources (referenced by zero partitions).
- No empty perspectives.
- Every visible object has a description.
- Referential integrity: no orphan foreign key values (checkable against the source CSVs directly in Prototype mode).

## 12. Explicitly Out of Scope for RDLC OS Phase 1 Builds

Row-Level Security, calculation groups, perspectives, translations/cultures, and DAX user-defined functions are all valid TMDL features but are **not** part of Phase 1 RDLC OS builds unless a confirmed Table Definitions document explicitly calls for one. Do not add these speculatively — same "structure only where evidence demands it" principle used throughout RDLC OS.
