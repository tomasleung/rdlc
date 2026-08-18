# Changelog — `rdlc-tmdl-build-agent`

Bug-fix log from real Power BI Desktop testing. Each entry: what broke, root cause, exact fix, and which permanent reference file changed as a result. This is the record of *why* the reference material looks the way it does — not just a list of edits.

---

## 2026-08-18 — Fix 1: Orphan `///` comment causes `InvalidLineType: Empty!`

**Symptom** (Power BI Desktop, on open):
```
TMDL Format Error: InvalidLineType — Unexpected line type: Empty!
Document - './tables/Animal Intakes'
Line Number - 22
```

**Root cause:** TMDL's `///` syntax is parsed as "accumulate description text for the next object declaration." A `///` comment block explaining a design rationale (why certain hidden columns use unspaced technical names) was written as a standalone block — not immediately attached to any table/column/measure — followed by a blank line before the next real object. The parser hit that blank line mid-accumulation and failed.

**Why it happened in two places (gold example AND the live Claude Code build):** The live build almost certainly used the gold example as its style/pattern reference and faithfully reproduced the same structure — including the bug. Confirmed by the error occurring at the *identical line number* in both files.

**Fix applied:**
- Removed the floating comment block; folded its explanation directly into the `///` descriptions of the two columns it was actually about (`IntakeCount`, `FosterFlag`)
- `references/tmdl-syntax-guide.md` — added an explicit CRITICAL rule: a `///` block must be immediately, directly adjacent to the object it documents, zero blank lines, ever
- `references/static-validation-checklist.md` — added as a Section A item, flagged as one of the two highest-value checks in the list (causes total load failure, not a lint warning)

---

## 2026-08-18 — Fix 2: `description` property on a relationship causes `DataModelLoadFailed`

**Symptom** (Power BI Desktop, on open):
```
Property 'description' is unknown and is not expected in the
situation it appears.
InnerException1.Stack Trace: ...SingleColumnRelationship.ReadMetadataProperties...
```

**Root cause:** Unlike tables, columns, measures, and hierarchies, **relationships do not support a `description` property in the Tabular Object Model.** `relationships.tmdl` had a `///` comment directly above each `relationship` declaration — valid syntax for other object types, invalid for this one.

**Fix applied:**
- Removed all four `///` lines from `relationships.tmdl` — relationship intent is expressed via naming and `fromColumn`/`toColumn`; any broader rationale (e.g., "bound to Intake Date, not Foster Date") lives in the fact table's own description instead
- `references/tmdl-syntax-guide.md` — added a CRITICAL note under Section 4 (Relationships) stating this restriction explicitly
- `references/static-validation-checklist.md` — added a matching Section A item

---

## What these two fixes have in common

Both were the same underlying mistake (misapplying `///`/description) on two different object types. Two data points isn't a pattern requiring a structural rethink, but it's exactly why `static-validation-checklist.md`'s Section A now explicitly checks for this class of error on every object type it applies to — not just the two that already broke.

## Outstanding — not yet fixed/decided

- **Bounded MCP retry rule** for Verify Mode — discussed and agreed in principle (max 1 corrected retry, then stop and report), not yet written into `SKILL.md`. See `SOP.md` for the interim manual guidance.
- No live model has yet been checked against `modeling-and-ai-readiness-standards.md`'s best-practice rules via Verify Mode — only static validation and a successful Desktop open + KPI verification have happened so far (see `TDD-VERIFICATION.md`).
