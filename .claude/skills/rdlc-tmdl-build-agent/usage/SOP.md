# SOP — Running `rdlc-tmdl-build-agent` in Claude Code

Standard operating procedure for a human operator kicking off, monitoring, and troubleshooting a build session. This is human-facing — it does not change how the agent itself behaves (that's `SKILL.md`'s job).

---

## Before opening Claude Code

Confirm on disk:
- Target `04-powerbi-project/` contains a genuinely empty Power BI project (created fresh in Power BI Desktop, saved as PBIP)
- `.claude/skills/rdlc-tmdl-build-agent/` is present with `SKILL.md` + `references/`
- `CLAUDE.md` and `README.md` are at the project root
- The confirmed Table Definitions doc you intend to build from has passed `rdlc-coach-semantic-model`'s handoff gate (no unresolved "Assumed — pending Data Owner" items, or an explicit override)

## Step 1 — Start the session

```
cd path/to/your-project
claude
```

**Do NOT run `/init`.** That generates a `CLAUDE.md` by scanning the codebase — if you already have a hand-written one, `/init` risks overwriting it with a generic auto-summary.

## Step 2 — Verify context loaded (don't skip this)

First prompt should confirm context, not start the real task:

```
Confirm you've loaded this project's CLAUDE.md and can see the
rdlc-tmdl-build-agent skill. Summarize the project structure and
tell me what's currently in 04-powerbi-project/.
```

If this comes back wrong (skill not found, folder structure misread), fix that before proceeding — don't discover a context problem mid-build.

## Step 3 — Invoke the skill explicitly

Name the skill and the mode directly. Do not rely on Claude Code inferring intent from a vague request.

```
Use the rdlc-tmdl-build-agent skill in Authoring Mode (file-based,
no MCP — do not connect to powerbi-modeling-mcp for this build).
Build the semantic model into the empty PBIP project at
04-powerbi-project/, using [confirmed Table Definitions doc path]
as the spec and [mock data folder path] as the local CSV source
(Prototype mode). Run the static validation checklist before
reporting the build complete.
```

## Step 4 — Review the build report

Before opening Power BI Desktop, confirm the report explicitly states:
- Every table/measure/relationship built, matching the confirmed spec
- The static validation checklist was actually run (not just claimed)
- Any items flagged rather than silently resolved (e.g., a spec detail the agent didn't feel authorized to change)

## Step 5 — Open in Power BI Desktop — the real test

Static validation cannot catch everything a live TOM/AS engine will. **This step is not optional** — a build is not confirmed working until it has actually opened.

If Desktop throws a `TMDL Format Error` or similar:
1. Copy the **full error text**, including the stack trace — not a paraphrase. Precise error text lets root cause be found immediately rather than guessed at.
2. Bring it to a coaching session (see `CHANGELOG.md` for how this has worked so far) or directly to Claude Code with the diagnosis attached, per whichever is more efficient for the specific error (see note below).
3. Once fixed, re-verify: don't just confirm "it opens" — confirm the actual measure/KPI values match what was predicted before the build (see `TDD-VERIFICATION.md`).

## When to debug via chat vs. directly in Claude Code

Both are legitimate; pick based on the situation:

- **Bring the error to a chat coaching session first** when the root cause is unclear — a detailed diagnosis derived once can be reused as a precise instruction, avoiding Claude Code re-deriving the same root cause from scratch (a real, avoidable cost).
- **Fix directly in Claude Code (or by hand)** once the root cause and fix are already known — for a single-line, already-diagnosed fix, relaying through chat first adds a step for no benefit.
- **After any fix**, always update the skill's `references/` files (not just the one broken build) so the same class of error can't recur on a future build. This is what `CHANGELOG.md` tracks.

## Verify Mode (MCP) — bounded, not automatic-and-unlimited

If invoking Verify Mode for best-practice/semantic checking (beyond what static validation covers):
- One corrected retry maximum per failed validation call
- If the retry also fails, stop and report the exact error — do not loop
- A repeated failure on the same object usually signals a real misunderstanding, not a fixable typo — more automated attempts won't resolve it

## Checking usage/cost

Run `/status` inside Claude Code for your real, current session and weekly usage — not an external estimate. Claude Code and Claude.ai chat share one usage pool on most plans.
