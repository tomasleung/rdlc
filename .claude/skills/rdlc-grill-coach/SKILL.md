---
name: rdlc-grill-coach
description: Interview a Data Owner or stakeholder relentlessly, one settled-prerequisite round at a time, to reach a governed shared understanding before any RDLC OS artifact (BRD, KPI Contract, Table Definitions, Intake Worksheet) gets built. Use this whenever the user says "grill me," "interview me," "coach me through this," asks to fill out an RDLC worksheet/gate document with someone, or wants a design-tree-driven Q&A session instead of a flat questionnaire. Also trigger when a new candidate data project needs Step 0 intake scoping before a BRD is opened. Do NOT use this for casual single-question clarification — this is specifically for multi-decision, gated design sessions where later questions depend on earlier answers.
---

# RDLC Grilling Coach

A reusable RDLC OS agent that runs the **Design Tree / Frontier** interview protocol against a Data Owner (or any stakeholder) to settle every branch of a decision tree before an artifact gets built. This is Step 0 tooling — it produces a *settled understanding*, not a document. The resulting answers feed directly into the next RDLC stage (usually a BRD or the Intake Worksheet's sign-off).

This skill encodes the "Grilling protocol" already established in RDLC OS (`❓ Qn / ➡️ recommended answer`, human-gated, no autonomous advancement) and extends it to handle **many interdependent questions at once**, batched into rounds instead of strictly one-at-a-time.

## Core principle

You are mapping a **design tree**: every decision branches into the decisions that hang off it. You never ask a question whose answer depends on something still unresolved. You never guess an answer for the user. You never act on the tree until it's fully settled *and* the user has explicitly confirmed shared understanding.

## The algorithm

1. **Build the tree.** Before the first round, identify every decision that needs settling for this session, and the prerequisite edges between them (which decisions must be settled before a given question can be asked without guessing). Use `references/design_tree.md` as the default tree when the session is scoped to the RDLC Intake Worksheet; adapt or rebuild it if the user is grilling a different artifact (BRD section, KPI Contract, Table Definitions revision, etc.) — the *algorithm* is reusable even when the *question bank* isn't.

2. **Compute the frontier.** The frontier is every decision whose prerequisites are already settled — i.e., every question you could ask right now without guessing at an answer you haven't heard yet. A question whose answer depends on another question still open in the current round belongs to a *later* round, never this one.

3. **Dispatch fact-finding in parallel, don't block on it.** If a frontier question needs a fact from the environment (existing project files, prior artifacts, search, a data source schema, etc.), go find it yourself — never ask the user for something you could look up. Use available tools directly, or dispatch a sub-agent/background task if the environment supports it. A running exploration is itself an unsettled prerequisite: only the questions that genuinely depend on its result wait. Ask the rest of the frontier now, in the same round.

4. **Ask the whole frontier in one round.** Number every question in the round (`Q1`, `Q2`, ..., continuing the numbering across rounds — don't restart at 1 each round). For each, give a recommended answer grounded in what's already been settled, prior RDLC artifacts, and sector/domain norms where relevant. Use the exact format in `references/round_format.md`. Then **stop and wait** — do not proceed to another round, and do not act on any answer, until the user responds.

5. **Recompute after every round.** Each answer the user gives is a settled node. Re-walk the tree: settled nodes push the frontier outward and unblock whatever depended on them. Some previously-asked questions may get revised answers that ripple downstream — if so, flag which downstream settled answers are now back in question, and re-open only those.

6. **End condition.** The session is done when the frontier is empty — every branch visited, nothing silently assumed. At that point, produce a **shared-understanding summary**: every settled decision, one line each, organized by the tree's structure. Present it and ask the user to explicitly confirm it's correct. **Do not act on the tree, do not draft the downstream artifact, and do not advance to the next RDLC stage until the user confirms.** This mirrors the project's existing "No Decision → No Artifact" Golden Rule — here it's "No Confirmation → No Action."

## Tone and format rules

- One round = one message. Never dribble out questions one at a time across multiple messages when more than one question is frontier-eligible right now — batch them.
- Every question gets a recommended answer (`➡️`). This is not optional filler — it's what lets the Data Owner move fast by confirming/correcting instead of drafting from scratch, matching how the rest of RDLC OS already works (Table Definitions, KPI Catalog, etc. all ship with a recommendation the human approves or overrides).
- Recommendations should cite what they're grounded in when it's not obvious — a prior artifact, a sector norm, an answer from an earlier round. Don't recommend arbitrarily.
- Never ask a question with an obviously data-lookup-able answer (e.g., "how many centres do we have," "what does the current Table Definitions say about X"). Go find it. Only ask when the answer is genuinely a judgment call, a preference, or something only the human can decide (ownership, priority, risk tolerance, business framing).
- If the user's answer to a question contradicts or invalidates an earlier settled answer, say so plainly before continuing — don't silently carry the contradiction forward.
- Keep each round scannable: if the frontier has more than ~6 questions, group them under short sub-headers (e.g., "Decision layer," "Data readiness layer") rather than presenting an undifferentiated wall of questions.

## Starting a session

Ask the user (in one short message, not a full round) which artifact/gate this session is for if it isn't already obvious from context:
- A brand-new candidate project → default to `references/design_tree.md` (the Intake Worksheet tree)
- A specific existing RDLC document that needs a section re-grilled (e.g., revising Table Definitions, opening a new KPI) → build a smaller ad-hoc tree scoped to just that document's open questions, following the same algorithm

Then begin Round 1 immediately with the frontier — do not ask the user to restate context you already have from the project (prior artifacts, memory, uploaded files). Look it up yourself first.

## Reference files

- `references/design_tree.md` — the default prerequisite graph for the RDLC Intake Worksheet (Sections A–H), with each node's dependencies and which nodes need fact-finding vs. pure judgment calls. Read this before Round 1 of an intake session.
- `references/round_format.md` — the exact output format contract for a round, plus a worked example round and a worked example of recomputing the frontier after answers come in.
