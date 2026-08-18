# TMDL Syntax Guide

Exact syntax rules for hand-authoring Tabular Model Definition Language (TMDL) files, distilled for RDLC OS's file-based (no-MCP-primary) authoring workflow.

## Source Provenance
| Source | URL | Last Verified |
|---|---|---|
| MS skills-for-fabric — semantic-model-authoring (tmdl-guidelines.md, direct-lake-guidelines.md, pbip.md) | https://github.com/microsoft/skills-for-fabric/tree/main/plugins/powerbi-authoring/skills/semantic-model-authoring | 2026-08-17 |

If asked to update this file against current MS guidance, re-fetch the URL above, diff against this content, and report changes before editing.

---

## 1. Core Syntax Rules

- **Indentation**: one consistent style per file (tabs recommended, matching Power BI tooling).
- **Object declaration**: `<type> <name>`, e.g., `table Customer`, `column ProductId`, `measure 'Total Sales'`.
- **Quoting**: names with spaces or special characters (`.`, `=`, `:`, `'`) must be wrapped in single quotes — `column 'Order Date'`.
- **Descriptions use `///` above the object — NOT a `description:` property.** This is a common and easy mistake:
  ```tmdl
  /// Revenue by product category
  measure 'Total Sales' = SUM(Sales[Amount])
  ```
- **CRITICAL: a `///` comment block must be immediately, directly adjacent to the object it documents — zero blank lines, zero gap.** `///` is parsed as "accumulate description text for the next object declaration." A `///` block followed by a blank line before reaching a table/column/measure/hierarchy declaration causes a hard parse failure: `TMDL Format Error: InvalidLineType — Unexpected line type: Empty!`. This means **standalone/floating explanatory comments are not valid TMDL** — every `///` block must end with an object declaration on the very next line, never with a blank line. If you need to explain a design rationale that doesn't belong to one specific object, put that explanation in a reference doc (e.g., this file, or `modeling-and-ai-readiness-standards.md`), not as a freestanding comment inside the `.tmdl` file itself.
- **`//` line comments are NOT supported in TMDL.** Comments are only valid inside M or DAX code blocks.
- **`lineageTag`**: omit when authoring new objects — the engine assigns GUIDs on first save. Never hand-author one.
- **Multi-line DAX**: wrap in triple backticks:
  ```tmdl
  measure 'Profit Margin' = ```
          DIVIDE([Total Revenue] - [Total Cost], [Total Revenue])
          ```
      formatString: 0.00%
  ```
- **Order within a table**: measures before columns.
- **`formatString` is required on every measure.**

## 2. File Layout

```
definition.pbism                        <- Semantic model connection settings
definition/database.tmdl                <- Database properties (compatibility level)
definition/model.tmdl                   <- Model properties, table/role/culture refs
definition/relationships.tmdl           <- Named/inactive relationships
definition/functions.tmdl               <- DAX user-defined functions
definition/tables/<TableName>.tmdl      <- Tables: columns, measures, partitions, hierarchies
definition/roles/<RoleName>.tmdl        <- Security roles (out of scope for Phase 1 builds)
definition/cultures/<locale>.tmdl       <- Translations (out of scope for Phase 1 builds)
definition/perspectives/<name>.tmdl     <- Perspectives (out of scope for Phase 1 builds)
```

### database.tmdl — must start with a `database` declaration
```tmdl
database <guid-or-name>
	compatibilityLevel: 1702
	compatibilityMode: powerBI
	language: 1033
```
**Critical**: a bare `compatibilityLevel:` without the `database` declaration line causes `InvalidLineType: Property!`.

### model.tmdl — declares refs so the engine discovers table/role/culture files
```tmdl
model Model
	culture: en-US
	defaultPowerBIDataSourceVersion: powerBI_V3
	sourceQueryCulture: en-US

ref table Sales
ref table Date
```
**Note**: `defaultPowerBIDataSourceVersion: powerBI_V3` is required for Import-mode models — omitting it causes `Import from JSON supported for V3 models only`.

## 3. Table Examples by Storage Mode

### Import (Prototype/CSV mode — RDLC OS default for mock-data builds)
```tmdl
table Customer

	/// Total number of customers
	measure '# Customers' = COUNTROWS(Customer)
		formatString: #,##0

	column CustomerId
		dataType: int64
		isHidden
		summarizeBy: none
		sourceColumn: CustomerId

	column 'Customer Name'
		dataType: string
		sourceColumn: CustomerName

	partition Customer = m
		mode: import
		source =
			let
				Source = Csv.Document(File.Contents("<local-path>\Customer.csv"), [Delimiter=",", Columns=2, Encoding=65001, QuoteStyle=QuoteStyle.None]),
				PromotedHeaders = Table.PromoteHeaders(Source, [PromoteAllScalars=true])
			in
				PromotedHeaders
```

### Direct Lake (Production mode — RDLC OS default once real Fabric tables exist)
```tmdl
expression DL_Lakehouse =
	let
		Source = AzureStorage.DataLake("https://onelake.dfs.fabric.microsoft.com/<WorkspaceId>/<LakehouseId>", [HierarchicalNavigation=true])
	in
		Source

table Sales

	/// Total revenue
	measure 'Total Sales' = SUMX(Sales, Sales[Quantity] * Sales[UnitPrice])
		formatString: $ #,##0.00

	column SalesKey
		dataType: int64
		isHidden
		summarizeBy: none
		sourceColumn: sales_key

	partition Sales = entity
		mode: directLake
		source
			entityName: Sales
			schemaName: dbo
			expressionSource: DL_Lakehouse
```

**Direct Lake specifics**:
- Every Direct Lake table needs a shared named `expression` targeting the OneLake source via `AzureStorage.DataLake` — never `Sql.Database` unless explicitly required.
- Partition source type is `EntityPartitionSource` (declared as `= entity`), never `MPartitionSource` — no Power Query for Direct Lake tables.
- `binary` dataType columns are unsupported in Direct Lake — exclude them if present in source.
- Migration process: create the named expression → analyze OneLake table schema → create tables mapping `sourceColumn` directly to OneLake column names, using `EntityPartitionSource` → deploy to a dev workspace to test if available.

## 4. Relationships

**CRITICAL: relationships do NOT support a `description` property or a `///` comment above them — unlike tables, columns, measures, and hierarchies, which all do.** Putting `///` above a `relationship` declaration causes a hard parse failure: `Property 'description' is unknown and is not expected in the situation it appears.` If a relationship needs explaining, put that explanation in the `///` description of the fact table itself (e.g., "Date analysis is bound to Intake Date, not Foster Date" belongs on the fact table's own description, not floating above the relationship).

```tmdl
relationship 'Sales to Date'
	fromColumn: Sales.'Order Date'
	toColumn: Date.Date
```

- Create relationships **before** any measure that depends on them.
- `fromColumn` = many-side (fact); `toColumn` = one-side (dimension).
- Default `crossFilteringBehavior: oneDirection` — only add `bothDirections` when explicitly required.
- Both sides must share the same `dataType`.
- Prefer integer keys over string keys for relationship columns.
- **No composite keys — not supported.** Single surrogate/natural key column only.
- **No surrogate key on fact tables** — use the natural/source key.
- Hide foreign key columns (`isHidden`).

## 5. Format String Reference

| Type | Format String | Example |
|---|---|---|
| Currency | `$#,##0.00` | $1,234.56 |
| Percentage | `0.00%` | 45.67% |
| Integer | `#,##0` | 1,234 |
| Decimal | `#,##0.00` | 1,234.56 |

## 6. Date/Calendar Table

- Prefer an existing date table from the source over a DAX-generated one.
- Ensure a contiguous date range — no gaps.
- Set `dataCategory: Time` on the date table.
- Configure `sortByColumn` on any text month/day-name column (sort by the numeric month/day, not alphabetically).
- Disable Power BI's auto-date tables when a proper calendar table exists.

## 7. Hierarchies (only if named in the confirmed Table Definitions doc)
```tmdl
hierarchy 'Geography Hierarchy'
	level Continent
		column: Continent
	level Country
		column: Country
```
- Declared inside the table, after columns.
- Levels ordered coarsest to finest.
- Never create a single-level hierarchy.

## 8. Calculation Groups (out of scope for Phase 1 RDLC OS builds — noted for completeness only)
Not used in Phase 1 models per RDLC OS scope discipline (no time-intelligence variation explosion expected yet). If a future phase needs YTD/LY/PY variations across many measures, revisit this section.

## 9. Annotations
- Never hand-author `PBI_*`-prefixed annotations (`PBI_FormatHint`, `PBI_ResultType`, etc.) — these are Power BI-internal and auto-managed.
- Custom annotations are fine for internal documentation/tooling metadata if genuinely needed.

## 10. Metadata Discovery (for Verify Mode only, via live DAX query — not static authoring)
When Verify Mode is active, use `INFO.VIEW.*` DAX functions for read-only metadata checks rather than guessing:
- `INFO.VIEW.TABLES()`, `INFO.VIEW.COLUMNS()`, `INFO.VIEW.MEASURES()`, `INFO.VIEW.RELATIONSHIPS()`
- To check for measure errors: `EVALUATE INFO.VIEW.MEASURES()` and inspect for an error/`ErrorMessage`-style column.
- Always run a scope-estimation query first (row counts of tables/columns/measures) before deep discovery, to avoid flooding context with unbounded metadata.
