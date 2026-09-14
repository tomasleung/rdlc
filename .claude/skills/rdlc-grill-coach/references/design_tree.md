# Design Tree — RDLC OS Intake Worksheet

Default prerequisite graph for grilling a Data Owner through `RDLC_OS_Data_Project_Intake_Worksheet_v0_1.docx`. Node IDs match the worksheet's question numbers. Use this as-is for a new candidate project; adapt it if the worksheet itself has been revised.

Legend: `[F]` = this node's recommended answer should be grounded in fact-finding (search project files, prior artifacts, source systems) before it's asked, not guessed from priors alone. `[J]` = pure judgment/ownership call — never dispatch fact-finding for these, just ask.

## Nodes and prerequisites

| Node | Question | Prereqs | Type | Notes |
|---|---|---|---|---|
| Q1 | What's the problem? | — (root) | J | Always Round 1. Nothing gates this. |
| Q2 | Is this new, or does it duplicate an existing tracked problem? | Q1 | F | Once Q1's answer names the problem domain, search existing project artifacts / prior RDLC BRDs for overlap before asking — arrive with a recommendation ("this looks distinct from Foster Analysis Phase 1/2/3" or "this overlaps with X, here's how"), don't make the user check. |
| Q3 | What decision would this data help make? | Q1 | J | Needs the problem framed (Q1) but not Q2 — can run same round as Q2. |
| Q4 | Who is the Decision Owner? | Q3 | J | Can't meaningfully ask "who owns this decision" before the decision itself is named. |
| Q5 | Decision cadence (how often revisited)? | Q3 | J | Same gate as Q4 — both unblock together once Q3 lands. |
| Q6 | What's the one Signal number? | Q3 | J | Also gated on Q3, not Q4/Q5 — can run in the same round as Q4/Q5. |
| Q7 | Does that number already exist somewhere today? | Q6 | F | Once the candidate Signal is named, search for it (source systems, existing reports, prior KPI Catalog entries) before asking — arrive with a readiness read, let the user confirm/correct. |
| Q8 | Where does the underlying data live? | Q6 | F | Same fact-finding pass as Q7 — batch together. |
| Q9 | Data readiness rating (Green/Amber/Red) | Q7, Q8 | J | This is the human's call, but only after Q7/Q8 facts are in — present your researched read as the recommendation. |
| Q10 | Do we know what "normal" looks like yet (baseline)? | Q9 | J | Needs readiness settled first — if data doesn't exist yet (Red), this question's honest answer is usually "no," which is fine and expected. |
| Q11 | Fixed Target vs. Baseline-Derived threshold? | Q10 | J | Direct continuation of Q10. |
| Q12 | Action + Action Owner if the Signal goes Red? | Q4 | J | Only truly needs the Decision Owner known (Q4) — doesn't need Q6–Q11 settled, so it can surface as soon as Q4 lands, even in an earlier round than Q9–Q11. Don't hold it hostage to the Threshold branch. |
| Priority | Decision Impact + Data Readiness ratings for the intake log | Q3, Q9 | — | Not a question to the user — the coach computes a recommended rating from settled Q3 (impact) and Q9 (readiness) and presents it for confirmation in the closing round, not as a new open question. |
| Sign-off | Data Owner / Decision Owner names + approval | ALL of the above | J | Always the final round. Nothing else may still be open. |

## Round shape this typically produces

- **Round 1:** Q1 only (or Q1 + any zero-prereq housekeeping). Everything else depends on Q1's answer.
- **Round 2:** Q2 (after fact-finding dispatched on Q1's answer), Q3. These don't depend on each other, so both go in the same round.
- **Round 3:** Q4, Q5, Q6 — all unblocked the moment Q3 lands. Also Q12 if Q4 already resolved by this point (it usually will, since Q4 and Q6 unblock together).
- **Round 4:** Q7, Q8 — after fact-finding dispatched on Q6's answer.
- **Round 5:** Q9 (needs Q7+Q8), and Q10 if the tree happens to allow it (Q10 needs Q9, so usually not yet — check before batching).
- **Round 6:** Q10, Q11.
- **Round 7 (closing):** Priority ratings presented for confirmation + Sign-off block.

This is a *typical* shape, not a fixed script — if the user's Round 1 answer already resolves what would normally take two rounds (e.g., they name both the problem and the decision unprompted), collapse rounds accordingly. Never ask a question whose prerequisite the user already answered unprompted, even if it "belongs" to a later round on paper.

## Fact-finding dispatch notes

For `[F]` nodes, before including the question in a round:
1. Search project files / knowledge base for anything matching the stated problem or signal.
2. If a source system is named or implied (e.g., ShelterBuddy for BC SPCA work), check whether prior artifacts (Table Definitions, BRD) already document its data readiness for a related signal — reuse that finding rather than re-deriving it.
3. Arrive at the question with a specific, evidenced recommendation ("Green — this already exists as `FosterFlag` in `Fact_AnimalIntake`" rather than a generic "probably check your source system").
4. If fact-finding is inconclusive or the source is genuinely new/unknown, say so honestly in the recommendation rather than fabricating a confident answer — an honest "I couldn't confirm this, my best guess is X, please correct me" is a valid recommendation.
