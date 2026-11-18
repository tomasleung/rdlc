# SOP — Starting a New Chat Session on This Project

**Purpose:** If this chat's history is lost (new window, new machine, context reset), this is the procedure to pick the work back up without re-deriving it from scratch and without silently trusting stale claims. Follow it in order.

---

## Step 1 — Read, in this order

1. **`CLAUDE.md`** — operational rules, pipeline order, which skill applies to what. Auto-loaded every session, but treat its "Current State" section as a *claim to verify*, not a fact — see Step 3.
2. **`PROJECT_STATUS.md`** — the initiative tracker. This tells you what's in flight, what stage each one is at, and what's open. This is the real "what happened" record, more current than `CLAUDE.md` or `README.md`.
3. **`README.md`** — narrative background on the RDLC OS agent chain. Useful for understanding *why* the pipeline is shaped the way it is, but as of 2026-11-18 it only describes 3 of the 8 skills and an old file-naming convention — don't treat it as current-state truth either.

## Step 2 — Figure out which initiative you're actually being asked about

`PROJECT_STATUS.md` lists every initiative this repo currently holds (Phase 1, CATU, SB Status/Source, Self-Service Data Platform, Triage Dashboard, Foster Volunteer Engagement, plus informational docs). Match the user's request to one of these sections before doing anything else — don't assume "Foster" means Phase 1 specifically; this repo hosts several unrelated Foster-adjacent initiatives at once.

If the request doesn't match any existing section, it's a new initiative — check the naming note at the top of `PROJECT_STATUS.md` before picking a name or phase number for it (phase numbers have already collided once in this project).

## Step 3 — Before repeating any "confirmed" or "current state" claim, verify it against real files

This is the single most important step, and the one that was skipped for weeks before this SOP existed. On 2026-11-18, `CLAUDE.md`'s "Current State" section turned out to describe a core measure, a table name, and a file path that didn't match what was actually built — because the project had moved on and nobody updated the summary.

Before telling the user something is confirmed, built, or current:
- **For a BRD/measure claim**: open the actual `.docx`/`.html` file (convert `.docx` via `pandoc -t markdown` if needed) and check its own Status field and Approvals table — don't rely on a prior summary's characterization of it.
- **For a "what's built" claim**: `ls` the actual folder (`03-mock-data/`, `04-powerbi-project/`, etc.) rather than quoting `CLAUDE.md`'s description of it.
- **For a cross-document reference** (e.g., "see BRD vX.X" or "per the approved Y document"): confirm that file actually exists in the repo before treating its contents as settled. Several real citations in this project turned out to point at documents that don't exist anywhere on disk.

If what you find contradicts `CLAUDE.md`/`PROJECT_STATUS.md`, say so plainly to the user and update the file — don't silently pick one version and move on, and don't silently patch `CLAUDE.md` without flagging what changed and why.

## Step 4 — Resuming a grill-coach session already in progress

`PROJECT_STATUS.md` records exactly which questions are settled and which are still open (by number) for any mid-flight `rdlc-grill-coach` session. Resume from there — re-ask only the open ones, don't restart the tree, and don't treat a past recommendation as confirmed just because it was presented once.

## Step 5 — When you finish a piece of work, update `PROJECT_STATUS.md` before ending the turn

A session that advances an initiative (settles a question, confirms a BRD, builds an artifact) and doesn't update `PROJECT_STATUS.md` recreates the exact problem this SOP exists to prevent. Treat the update as part of finishing the task, not an optional follow-up.

## Step 6 — Naming discipline

Before introducing a new phase number, KPI name, or measure name: check `PROJECT_STATUS.md` and the relevant BRDs for an existing claim on that name. This project has already had two real collisions — "Phase 2" claimed independently by CATU and (briefly) by Foster Volunteer Engagement, and "% Animals in Foster" used for two different denominators across Phase 1 and CATU before being disambiguated to "% Animals in Care Fostered."

---

## Quick checklist for the start of any new session

- [ ] Read `CLAUDE.md`, `PROJECT_STATUS.md`
- [ ] Identified which initiative(s) this request touches
- [ ] Did not repeat a "confirmed"/"current state" claim without checking the real file first
- [ ] If anything contradicted `PROJECT_STATUS.md`, flagged it to the user rather than silently resolving it
- [ ] Updated `PROJECT_STATUS.md` before ending the session if anything materially advanced
