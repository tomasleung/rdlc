# RDLC OS Skill Conventions

What `rdlc-gate-monitor`'s discovery protocol looks for when scanning `.claude/skills/*/SKILL.md`. This is the documented version of the convention — if the convention changes, update this file, not the agent's private understanding of it.

Confirmed present, identically, in all RDLC OS skills as of this writing (`rdlc-coach-semantic-model`, `rdlc-mock-data-generator`, `rdlc-tmdl-build-agent`, `rdlc-grill-coach`).

## Frontmatter fields

| Field | Purpose | Required for discovery? |
|---|---|---|
| `name` | Skill identifier, matches folder name | Yes |
| `description` | Trigger-matching text | Yes (not used for gate logic, only invocation) |
| `framework: RDLC OS` | Marks this skill as part of the pipeline gate-monitor should discover | Yes — skills without this marker are skipped |
| `metadata.lifecycle_phase` | Where this stage sits in the pipeline, in prose | Yes — used to place the stage in order |
| `metadata.upstream_agent` | What produces this stage's required input | Yes — used to derive pipeline edges |
| `metadata.downstream_agent` | What consumes this stage's output | Yes — used to derive pipeline edges |
| `metadata.author`, `metadata.version`, `metadata.role_pairing` | Informational | No |

Note: `rdlc-grill-coach` doesn't currently carry this `metadata:` block in the same shape as the other three (it predates the convention being made explicit). Treat a discovered RDLC OS skill missing these fields as a finding to report ("this skill doesn't declare lifecycle_phase/upstream_agent/downstream_agent, pipeline position inferred from its description instead"), not as a reason to skip it.

## Body tags

| Tag | Purpose | Always present? |
|---|---|---|
| `<role>` | Scope/boundary statement | Yes |
| `<non_negotiable_rules>` | Hard rules — always a source of gate conditions | Yes |
| `<input_contract>` | What this stage requires before starting — used for the input-contract cross-check | Yes |
| `<handoff_gate>` | Explicit completion criteria for this stage | Not always — e.g. `rdlc-tmdl-build-agent` uses `<mode_selection>` instead, since its "gate" is about which authoring mode is active rather than a document-completeness checklist |
| `<mode_selection>` | Where present, defines trigger-gated sub-modes (e.g. Authoring vs. Verify/Setup) — treat its trigger conditions as gate-equivalent when no `<handoff_gate>` tag exists | No |
| `<validation_guardrails>` | Must / Prefer / Avoid lists — the "Avoid" list is the source for mechanical spot-checks | Yes |
| `<reference_files_definition>` | Points to the stage's own standards/format docs — useful context for understanding *why* a gate condition exists, not itself a gate source | Usually |
| `<degraded_mode_instructions>` | What the stage does if its own references are unavailable — not relevant to gate-monitor's own checks | Usually |

## What counts as a gate condition

A gate condition is anything in `<handoff_gate>` (or its `<mode_selection>` equivalent) or `<non_negotiable_rules>` phrased as a requirement the artifact must satisfy before being treated as complete/confirmed — not every sentence in a skill's body. When in doubt, a line containing "must," "never," "refuse," "halt," or an explicit before/after condition ("before X is done, Y must be true") is a gate condition; descriptive or stylistic preference lines ("prefer distributing measures across tables") are not gate conditions, they're the `<validation_guardrails>` "Prefer" tier — worth noting if violated, but not a compliance failure.
