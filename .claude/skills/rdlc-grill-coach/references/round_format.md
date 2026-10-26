# Round Format Contract

Every round follows this exact shape. Question numbering follows the convention in `SKILL.md`'s Round format section — read that first if you haven't; the short version is: preserve a node's native tree ID for the default Intake Worksheet tree (even out of order), or number sequentially for an ad-hoc tree. Never restart numbering in a later round.

```
❓ **Q<n>** - **<question title>**: <question body, may be multiple paragraphs, may include multiple-choice options>

➡️ **Recommended:** <standalone proposed answer>

**Basis:** <what grounds it — a settled answer, a retrieved fact with its source, a prior artifact, a governance rule, or an explicitly named domain norm>

**Affects:** <the downstream design choices this answer unlocks or constrains, when non-obvious>

---

❓ **Q<n+1>** - **<question title>**: <question body>

➡️ **Recommended:** ...

**Basis:** ...
```

- `**Affects:**` is optional when the downstream impact is obvious from the question itself; `**Basis:**` is not optional — every recommendation needs one, even if the basis is "no prior source found, this is domain-norm judgment."
- If a recommendation rests on an assumption rather than a confirmed fact, say so in the Basis line explicitly (e.g., "Basis: assumed — no documentation confirms this; flagging per the Assumption rule") rather than stating it with the same confidence as a sourced fact.
- No content after the last question's recommendation — the round ends there. Wait for the user.
- If the round is closing the session (frontier empty), append a **Shared Understanding Summary** after the last question instead of another `❓`, and ask for explicit confirmation before anything gets acted on.

## Worked example — Round 1 (root question only)

```
❓ **Q1** - **What's the problem?**: What's happening right now that made you think this needs a data project — describe the situation, not the solution you're imagining. What's unclear, inconsistent, or hard to act on today?

➡️ I don't have this yet — this is the one question nothing else in the tree can be inferred ahead of, so there's no recommendation to offer. Tell me in your own words and I'll take it from there.
```

(Q1 is the one legitimate case where "no recommendation" is honest — everything downstream of it genuinely can't be pre-guessed. Every other node should carry a real `Basis`.)

## Worked example — Round 3 (frontier expands after Q3 lands, node ID out of numeric order)

Assume the user's Round 2 answer settled Q3 as: *"Decide whether to open a new evening-intake triage shift at high-volume centres."*

```
❓ **Q4** - **Who owns this decision?**: Based on "whether to open a new evening-intake triage shift," this reads as an operations staffing call, not a clinical or executive one.

➡️ **Recommended:** The **Centre Operations Manager** (or regional equivalent).

**Basis:** They're the one who'd actually authorize a shift change — inferred from the decision framing in Q3, no prior artifact named an owner for this specific decision.

**Affects:** Who Q12 (escalation owner) names if the signal goes red.

---

❓ **Q5** - **How often does this decision get revisited?**: Staffing decisions like this are usually reviewed on a fixed operational cadence rather than continuously.

➡️ **Recommended:** Quarterly, tied to typical staffing/budget review cycles.

**Basis:** Domain norm for staffing decisions — no seasonal driver has been named yet that would argue for a tighter check.

---

❓ **Q6** - **What's the one Signal number?**: The number that would tell the Ops Manager whether the current shift structure is working.

➡️ **Recommended:** After-hours intake volume as a % of total daily intake.

**Basis:** Directly answers "is enough volume happening outside current staffed hours to justify a shift change" — maps straight back to the Q3 decision.

**Affects:** Q7/Q8 fact-finding (does this number already exist, where does the underlying data live) will be dispatched against this exact definition next round.

---

❓ **Q12** - **What happens if this signal goes red, and who's responsible?**: This only needed the Decision Owner (Q4) settled, not the Signal/Threshold branch — so it's eligible now, in the same round as Q4–Q6, rather than waiting for Q7–Q11 to close. Note it keeps its tree ID (Q12), not "Q7" — see the numbering convention in SKILL.md.

➡️ **Recommended:** If after-hours volume sustains above threshold for 2+ review cycles, the Centre Operations Manager escalates a staffing proposal to Regional leadership for the next budget cycle — not an automatic shift change, since staffing has real cost implications that need a second sign-off.

**Basis:** Standard RDLC action-owner pattern — a signal breach triggers an escalation, not an automatic system action, when the response has budget/staffing consequences.
```

## Worked example — reopened branch

If in the round above the user corrects Q4 to name a *Regional Director* instead of a Centre Operations Manager:

```
**Reopened branch**

**New answer:** Decision Owner is the Regional Director.

**Conflicts with:** Q4's settled answer (Centre Operations Manager).

**Affected:** Q12's escalation path — it currently reads "escalates *to* Regional leadership," which no longer makes sense if the Regional Director *is* the owner. Updating Q12's answer to route the escalation directly, not upward.

**Unchanged:** Q5 and Q6 don't reference the Decision Owner's identity, so they stand as settled.
```

Then proceed to the next legitimately-unblocked round as normal. Don't silently re-ask settled questions — call out what changed and move forward.

## Worked example — blocked fact

```
❓ **Q7** - **Does that number already exist somewhere today?**: Checking whether "after-hours intake volume as a % of total daily intake" is already tracked anywhere.

➡️ **Recommended:** I couldn't confirm this — I searched the existing KPI Catalog and prior Table Definitions and found intake volume by day, but nothing broken out by time-of-day or shift window.

**Basis:** Blocked fact — the source that would answer this (ShelterBuddy's intake-timestamp data) isn't accessible from here to query directly. Marking Q7 blocked rather than guessing; it doesn't gate Q9's readiness read, since "not found in existing catalog" is itself a usable answer for that.

I'm not asking you to recall this by hand — if you happen to know offhand that it's tracked somewhere I haven't checked, say so; otherwise I'll carry this forward as a blocked fact and Q9's readiness rating will reflect "doesn't exist yet, needs to be built."
```

## Worked example — partial answer handling

If the user answers only Q4 and Q6 from the Round 3 example above, and leaves Q5 and Q12 unanswered:

```
Settling Q4 (Decision Owner: Centre Operations Manager) and Q6 (Signal: after-hours intake volume %) from your answer.

Q5 and Q12 are still open — I won't treat the silence on those as agreement with the recommendations. Since Q12 only needed Q4 (now settled), it's still eligible; re-asking it below alongside whatever else is now unblocked, rather than carrying it forward unchanged.

❓ **Q5** - **How often does this decision get revisited?**: [as before]

➡️ **Recommended:** Quarterly, tied to typical staffing/budget review cycles.

**Basis:** Domain norm — unchanged from last round, repeating since it wasn't answered.

---

❓ **Q12** - **What happens if this signal goes red, and who's responsible?**: [as before, repeated since not yet answered]

➡️ **Recommended:** Centre Operations Manager escalates to Regional leadership after 2+ review cycles above threshold.

**Basis:** Unchanged from last round.
```

## Worked example — scope split

```
This session has grown into two genuinely separate decision trees — I don't want to force them through one frontier:

1. **Evening-intake staffing** (Q3–Q6, Q12) — settled or near-settled, operations-owned.
2. **Centre capacity expansion** (introduced by your Q6 follow-up about whether high-volume centres need physical space, not just staffing) — a distinct decision with its own owner, likely Facilities or Regional Director, and its own signal.

Recommend finishing the staffing tree first since it's closer to settled, then opening a separate grilling session for capacity expansion rather than interleaving both frontiers in one set of rounds. Want to proceed that way, or prioritize differently?
```

## Closing round shape

```
❓ **Q13** - **Sign-off**: That settles every branch of the tree. Here's the shared understanding for confirmation:

**Problem:** ...

**Decision:** [settled outcome]
**Rationale:** [why]
**Basis:** [fact / artifact / rule / judgment]
**Impact:** [downstream consequence]
**Owner:** [only if material]
**Status:** Settled

... (one block per material node) ...

### Exclusions
[what's explicitly out of scope]

### Open or accepted limitations
[anything unresolved the user chose to carry forward, e.g. Q7's blocked fact — "after-hours split doesn't exist yet; accepted as a Red readiness rating, not a blocker to opening the BRD"]

### Artifact readiness
This understanding is sufficient to support opening a BRD. It is not yet sufficient for a KPI Contract, since the Signal's exact source table (Q8) remains blocked.

➡️ Do you confirm that this shared understanding is correct and complete for the stated scope? ("Confirmed" / "Yes, this is correct and complete" / "Proceed based on this understanding" — not a mid-discussion "sounds good.")
```

Note `Q13` here is an assigned sequential number for the Sign-off step, which `design_tree.md`'s node table deliberately leaves unnumbered (it lists it as "Sign-off," not a `Q#`, since it's always the final round regardless of how many numbered questions preceded it). The Priority rating likewise isn't its own `❓` — per `design_tree.md`, it's a computed read from settled nodes, presented for confirmation inside this same closing block, not asked as a new open question.
