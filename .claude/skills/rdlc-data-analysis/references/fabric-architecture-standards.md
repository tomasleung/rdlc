# Microsoft Fabric Architecture Standards

> Consolidated 2026-10-26 from *Enterprise Analytics Standards & Governance Playbook* Part 3 (unique to that source — the other two framework docs don't cover Fabric/Lakehouse architecture at all).
>
> **Scope note:** this content describes the *build-time* data platform standards (Bronze/Silver/Gold, Direct Lake, Delta Lake maintenance) that apply once a semantic model is actually being constructed. It is downstream of this skill's own job (Analysis Design, which runs before Table Definitions exist) — kept here as context so this skill's output stays compatible with it, but it's primarily load-bearing for `rdlc-coach-semantic-model` and `rdlc-tmdl-build-agent`, not for this stage's own execution workflow.

## Architecture principles

1. **OneLake is the single source of truth.** Avoid duplicated datasets, redundant storage, departmental copies unless a specific business requirement exists.
2. **Business logic moves upstream.** Preferred flow: `Bronze → Silver → Gold → Semantic Model → Power BI`. Avoid putting significant business logic in Power BI measures, calculated columns, or report-level transformations when it can live further upstream.
3. **Gold is the analytics contract.** Business users consume Gold, never Bronze or Silver directly. Gold represents approved, business-ready data.

## Medallion architecture

| Layer | Purpose | Standards |
|---|---|---|
| **Bronze** | Raw data preservation, source ingestion, audit/lineage | Minimal transformation, append-only, near-source structure. Store source IDs, load timestamps, batch IDs, raw values. Never apply business rules, delete source columns, or modify source meaning here. |
| **Silver** | Cleansing, conformance, standardization | Deduplication, data quality enforcement, business keys, schema validation, survivorship logic. SCD Type 1 for corrections (e.g. misspelled names); SCD Type 2 for historical tracking (e.g. program assignment, location changes). |
| **Gold** | Business consumption, reporting, forecasting | Fact tables, dimensions, aggregations, business metrics — designed for Direct Lake / Power BI with minimal further transformation. Avoid row-level raw data, source-system complexity, or technical metadata in Gold. |

## Fact table standards

Define the grain before building anything. If the grain can't be stated in one clear sentence, don't build the fact table yet. Examples from this project: `Fact Foster Placements` (one row = one foster placement), `Fact Foster Animal Month` (one row = one animal per month).

## Dimension standards

Use conformed (shared) dimensions wherever possible — Date, Location, Program, Animal — for consistency across reports. Prefer integer surrogate keys over natural/business keys for performance, historical tracking, and data integrity.

## Direct Lake optimization

Keep relationships simple (star schema; avoid complex many-to-many models). Control cardinality — keep high-cardinality columns (transaction IDs, GUIDs, free text) in fact tables, not dimensions. Reduce model complexity by pushing logic into Gold rather than complex report-level DAX.

**Delta Lake maintenance:** `OPTIMIZE` for file compaction (reduces small-file problems); `VACUUM` per documented retention/recovery/compliance requirements; `V-Order` enabled for analytics workloads and Direct Lake consumption.

## Data engineering standards

Pipelines should be idempotent, restartable, auditable, observable. Log pipeline name, execution time, rows processed, success/failure status. Support retry logic, failure alerts, exception logging. Prefer incremental loads (watermarks, CDC, merge operations) over full reloads for large datasets.

## Governance and security

Document, per Fabric solution: Data Sources (source systems, refresh frequency, ownership), Data Lineage (`Source → Bronze → Silver → Gold → Semantic Model → Report`), Refresh Strategy (schedule, dependencies, failure process). Access follows least-privilege by role, business need, and data sensitivity; document classification/retention/access requirements for sensitive data.

## Architecture review checklist

Before production: raw data preserved in Bronze? Data quality enforced in Silver? Gold business-ready? Direct Lake optimized? Documented and reproducible?
