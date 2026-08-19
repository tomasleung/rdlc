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

---

## 2026-08-19 — Finding: First live Verify Mode pass (dual independent check) surfaces 5 real reference-material gaps

**Not a bug in the built model** — this is a different kind of entry from Fix 1/2 above. The model opened and worked correctly throughout; what changed is our *reference material's* completeness, confirmed via two independent live checks run back-to-back:

1. **Live MCP review** — Claude Code invoked Microsoft's real `powerbi-authoring:semantic-model-authoring` skill directly (not `rdlc-tmdl-build-agent`), connected via `powerbi-modeling-mcp` to the actual running Foster Analysis model, and ran its Workflow: Analyze Best Practices (read-only, `ConnectFolder` → `List`/`Get` operations → `Disconnect`).
2. **Manual Tabular Editor BPA** — the human operator independently ran the full 71-rule Best Practice Analyzer rule set directly in Tabular Editor against the same live model.

**Findings, triaged:**

| # | Finding | Disposition |
|---|---|---|
| 1 | `isAvailableInMdx: false` missing on 11 hidden columns | **Real gap — fixed.** Applied to the live model via `rdlc-tmdl-build-agent` (Authoring Mode); confirmed by both checks independently. |
| 2 | Hidden-column casing (`IntakeCount`/`FosterFlag`) reads as a naming violation under MS's `naming-conventions.md` taken literally | **Documentation gap, not a model defect.** The design (avoiding a measure/base-column name collision) was already correct; the *reasoning* wasn't explicitly written into our own reference material. Now is. |
| 3 | `isKey` not set on dimension primary keys (BPA `MARK_PRIMARY_KEYS`) | **Expected — confirms an existing decision.** Exactly the documented MS-wins conflict, firing as predicted. No action. Separately, a real open question about whether `isKey`/"Key Column" has AI-readiness value was surfaced and explicitly **parked, not resolved** — see `modeling-and-ai-readiness-standards.md`. |
| 4 | `Centre ID`/`Intake Type ID` relationship columns are `String`, not `Int64` | **Already-known, accepted tradeoff** (ShelterBuddy native keys) — now explicitly documented as an exemption for the first time. |
| 5 | `Animal ID`/`Source Intake ID` flagged as unreferenced hidden columns | **Already-known, accepted** (retained for Phase 2 `DISTINCTCOUNT`/QA use) — now explicitly documented as an exemption for the first time. |

**Fixes applied:**
- `references/modeling-and-ai-readiness-standards.md` — 5 edits: `isAvailableInMdx` guidance, hidden-column casing exemption (§8), `isKey` parked-question note (§Documented Conflict), String relationship key exemption (§6), retained-column exemption (§11)
- `references/static-validation-checklist.md` — added `isAvailableInMdx` item; added exemption pointers to the relationship-column and unreferenced-hidden-column checks so future runs don't flag #4/#5 as new defects
- **Live model**: `rdlc-tmdl-build-agent` applied `isAvailableInMdx: false` to all 11 columns directly via Authoring Mode file edit; static checklist re-run clean afterward

**Also corrected on the record, not silently dropped:** an earlier claim in this session that `isKey` is a "deprecated legacy property" was checked against real sources and found wrong — the deprecated property found was a different, older OLAP-mining-model `IsKey`, not Tabular's `Column.IsKey`. The actual reason for MS's DON'T guidance remains genuinely unstated in their source document.

**Why this matters beyond the specific findings:** this is the first time in the project that live, independent third-party review (not our own static reasoning) fed back into the reference material with a full, honest triage — separating real gaps from already-known accepted tradeoffs from genuinely unresolved open questions, rather than treating every flag as either "fix it" or "ignore it."

## Outstanding — not yet fixed/decided

- **Bounded MCP retry rule** for Verify Mode — discussed and agreed in principle (max 1 corrected retry, then stop and report), not yet written into `SKILL.md`. See `SOP.md` for the interim manual guidance.
- **`isKey`/AI-readiness open question** — parked 2026-08-19, not resolved. Revisit if Copilot/AI-agent readiness becomes a real priority; start from `isDefaultLabel` (§10 of `modeling-and-ai-readiness-standards.md`), not `isKey`.
- **Date table marking / format string discrepancies** (BPA findings #3/#4 from the same review pass) — investigated partially: confirmed the Date table *is* correctly marked in a live, data-loaded Desktop session (the earlier flag was likely a metadata-only MCP connection limitation, not a real gap). Format string application not yet independently re-checked. Low priority, parked.
