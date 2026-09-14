# Round Format Contract

Every round follows this exact shape. Question numbering is global across the whole session — never restart at Q1 in round 2.

```
❓ **Q<n>** - **<question title>**: <question body, may be multiple paragraphs, may include multiple-choice options>

➡️ <your recommended answer, grounded in fact-finding or prior settled answers where possible>

---

❓ **Q<n+1>** - **<question title>**: <question body>

➡️ <your recommended answer>
```

- No content after the last question's recommendation — the round ends there. Wait for the user.
- If the round is closing the session (frontier empty), append a **Shared Understanding Summary** after the last question instead of another `❓`, and ask for explicit confirmation before anything gets acted on.

## Worked example — Round 1 (root question only)

```
❓ **Q1** - **What's the problem?**: What's happening right now that made you think this needs a data project — describe the situation, not the solution you're imagining. What's unclear, inconsistent, or hard to act on today?

➡️ I don't have this yet — this is the one question nothing else in the tree can be inferred ahead of, so there's no recommendation to offer. Tell me in your own words and I'll take it from there.
```

(Note: Q1 is the one legitimate case where "no recommendation" is honest — everything downstream of it genuinely can't be pre-guessed. Every other node should carry a real recommendation.)

## Worked example — Round 3 (frontier expands after Q3 lands)

Assume the user's Round 2 answer settled Q3 as: *"Decide whether to open a new evening-intake triage shift at high-volume centres."*

```
❓ **Q4** - **Who owns this decision?**: Based on "whether to open a new evening-intake triage shift," this reads as an operations staffing call, not a clinical or executive one.

➡️ Recommend the **Centre Operations Manager** (or regional equivalent) as Decision Owner — they're the one who'd actually authorize a shift change. Confirm or correct.

---

❓ **Q5** - **How often does this decision get revisited?**: Staffing decisions like this are usually reviewed on a fixed operational cadence rather than continuously.

➡️ Recommend **quarterly**, tied to typical staffing/budget review cycles — unless there's a specific seasonal driver (e.g., kitten season volume) that would argue for a tighter monthly check during peak months.

---

❓ **Q6** - **What's the one Signal number?**: The number that would tell the Ops Manager whether the current shift structure is working.

➡️ Recommend **After-hours intake volume as a % of total daily intake** — it directly answers "is enough of our volume happening outside current staffed hours to justify a shift change," which maps straight back to the Q3 decision. Confirm or propose an alternative.

---

❓ **Q12** - **What happens if this signal goes Red, and who's responsible?**: This only needed the Decision Owner (Q4) settled, not the Signal/Threshold branch, so it's ready now rather than waiting for Q7–Q11.

➡️ Recommend: if after-hours volume sustains above threshold for 2+ review cycles, the Centre Operations Manager escalates a staffing proposal to Regional leadership for the next budget cycle — not an automatic shift change, since staffing has real cost implications that need a second sign-off.
```

## Worked example — recomputing the frontier after answers

If in the round above the user corrects Q4 to name a *Regional Director* instead of a Centre Operations Manager, that's a settled-node revision. Before asking the next round:

```
Noting: Q4 revision changes who Q12's escalation path names as the acting owner — updating Q12's answer to route directly to the Regional Director rather than escalating *to* them. No other downstream nodes are affected since Q5/Q6 didn't reference the Decision Owner's identity.
```

Then proceed to the next legitimately-unblocked round as normal. Don't silently re-ask settled questions — call out what changed and move forward.

## Closing round shape

```
❓ **Q13** - **Sign-off**: That settles every branch of the tree. Here's the shared understanding for confirmation:

**Problem:** ...
**Decision:** ... (Owner: ..., Cadence: ...)
**Signal:** ... (currently: exists / partial / new — Readiness: 🟢/🟡/🔴)
**Threshold approach:** Fixed Target / Baseline-Derived — ...
**Action if Red:** ... (Owner: ...)
**Priority read:** Decision Impact = ..., Data Readiness = ... → recommended next step: open a BRD / scope data work first / log and defer

➡️ If this all reads correctly, confirm and I'll treat this as settled — nothing gets drafted into a BRD or the Intake Worksheet until you say so.
```
