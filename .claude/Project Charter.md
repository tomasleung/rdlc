# Decision-Driven Development Project Charter

## 1. Project Identity

**Project:** [Project Name]

**Project Owner:** [Business Owner]

**Delivery Owner:** [Technical / Delivery Owner]

**Version:** [Version]

**Status:** [Draft / Active / Completed]

---

## 2. Purpose

This project exists to **turn data into governed, decision-ready knowledge**.

The objective is not to build dashboards, reports, applications, or AI features for their own sake.

The objective is to improve a specific business decision by providing the right:

**Signal → Meaning → Action → Outcome**

---

# 3. Decision First

## Primary Decision

**What decision are we trying to improve?**

> [Describe the real business decision in one sentence.]

Example:

> Which shelter should receive an incoming animal?

## Decision Owner

**Who owns the decision?**

> [Person / Role]

## Decision Frequency

> [Real-time / Daily / Weekly / Event-driven]

## Decision Consequence

What happens if this decision is:

* Made correctly?
* Made incorrectly?
* Delayed?
* Made using poor or stale information?

---

# 4. Business Outcome

## Desired Outcome

The project must produce a measurable improvement in the decision.

**Primary outcome:**

> [Describe the business result.]

**Success measures:**

* [Measure 1]
* [Measure 2]
* [Measure 3]

### Benefit Gate

Before building functionality, confirm:

> **What business benefit does this capability create?**

If the benefit cannot be explained, the capability should not be prioritized.

**Priority order:**

**Benefit → Efficiency → Comfort**

---

# 5. Decision Signals

The project should identify the **minimum set of signals required to support the decision**.

| Signal   | Meaning            | Source   | Freshness     | Owner   |
| -------- | ------------------ | -------- | ------------- | ------- |
| [Signal] | [What it tells us] | [Source] | [Requirement] | [Owner] |
| [Signal] | [What it tells us] | [Source] | [Requirement] | [Owner] |

## Signal vs Noise

Every proposed metric, field, visualization, feature, or AI output must answer:

> **Does this help the decision owner make a better decision?**

If not, it is noise unless there is a documented secondary purpose.

---

# 6. Action

A decision-ready solution must make the next action clear.

**Decision:**
[What must be decided?]

**Signal:**
[What evidence supports the decision?]

**Interpretation:**
[What does the signal mean?]

**Action:**
[What should the user do?]

**Expected outcome:**
[What should improve?]

---

# 7. Scope

## In Scope

Only capabilities that directly support the defined decision and agreed outcomes.

* [Capability]
* [Capability]
* [Capability]

## Out of Scope

* Features without a defined decision or business benefit
* Unrequested reporting
* Unnecessary data collection
* Premature automation
* Technical complexity without measurable value
* AI functionality without a defined user/business purpose

Scope expansion requires explicit approval.

---

# 8. Governance

Every important element must have a clear owner.

| Area                   | Owner             |
| ---------------------- | ----------------- |
| Business Decision      | [Business Owner]  |
| Business Definition    | [Business Owner]  |
| Data                   | [Data Owner]      |
| Delivery               | [Delivery Owner]  |
| Technical Architecture | [Technical Owner] |
| Production Deployment  | [Owner]           |
| Adoption               | [Business Owner]  |

## Governance Principles

**Define** — What exactly does it mean?

**Own** — Who is accountable?

**Trace** — Where did the information come from?

**Trust** — How do we know it is reliable?

**Protect** — How is it secured?

**Place** — Where does it belong in the solution?

**Document** — Can another person understand and operate it?

---

# 9. Decision Contracts

The project must establish explicit contracts before implementation.

### Business Contract

What business decision and outcome are being supported?

### Access Contract

Who can access the information and perform the action?

### Technical Contract

What technical architecture and constraints must be followed?

### Audit Contract

What must be traceable, logged, or auditable?

### Artifact Contract

What deliverables must exist and what is their authoritative source?

---

# 10. Delivery Lifecycle

The project follows the Decision-Driven Development lifecycle:

**DECIDE → GOVERN → DEFINE SIGNALS → DESIGN → BUILD → ACT → LEARN**

### 1. DECIDE

Define the business decision and desired outcome.

### 2. GOVERN

Establish ownership, definitions, contracts, controls, and accountability.

### 3. DEFINE SIGNALS

Identify the minimum trustworthy signals required for the decision.

### 4. DESIGN

Design the user experience, data flow, architecture, and decision-support experience.

### 5. BUILD

Implement the smallest useful solution.

### 6. ACT

Enable the decision owner to interpret the signal and take action.

### 7. LEARN

Measure adoption, decision quality, outcomes, and feedback.

---

# 11. Vibe Coding / AI Development Rules

AI is a **delivery accelerator**, not the business decision owner.

The AI must follow the project charter before generating or modifying code.

### AI must:

1. Understand the decision before writing code.
2. Read the existing repository before changing it.
3. Preserve existing working functionality.
4. Prefer the smallest useful implementation.
5. Reuse existing patterns before introducing new ones.
6. Never invent business rules.
7. Never change the defined business meaning silently.
8. Identify assumptions explicitly.
9. Ask for clarification when an ambiguity could materially change the decision or architecture.
10. Validate the implementation against the defined acceptance criteria.
11. Keep documentation synchronized with meaningful changes.
12. Avoid building features simply because they are technically possible.

### AI must not:

* Optimize for feature count.
* Optimize for visual complexity.
* Create metrics without defined meaning.
* Create automation without an accountable owner.
* Replace human accountability for business decisions.
* Introduce architecture merely for future possibilities.
* Treat generated code as correct simply because it runs.

---

# 12. Human-in-the-Loop

AI can accelerate:

* Analysis
* Design
* Coding
* Testing
* Documentation
* Data exploration
* Pattern detection

Humans remain accountable for:

* Business meaning
* KPI definitions
* Thresholds
* Decision rules
* Risk
* Exceptions
* Final decisions
* Governance

**AI recommends / accelerates.
Humans define / govern / decide.**

---

# 13. Definition of Done

A capability is not complete merely because the code works.

It is complete when:

* [ ] The supported decision is clearly defined.
* [ ] Business owner is identified.
* [ ] Business definitions are agreed.
* [ ] Required signals are defined.
* [ ] Signal sources are known.
* [ ] Data quality is acceptable.
* [ ] The user can understand the signal.
* [ ] The required action is clear.
* [ ] Appropriate validation has passed.
* [ ] Existing functionality remains intact.
* [ ] Security/access requirements are satisfied.
* [ ] Documentation is updated.
* [ ] The business owner accepts the outcome.

### Final Gate

> **Does this solution improve the decision it was created to support?**

If the answer is no, the feature is not done.

---

# 14. Change Control

Any change that affects the following requires review:

* Primary business decision
* Decision owner
* Business outcome
* Business definition
* KPI / signal definition
* Decision rules
* Data source
* Security
* Architecture
* Scope

AI agents must not make material changes to these areas autonomously.

---

# 15. Current Project Priorities

**Primary Decision**

> [Decision]

**Current Business Outcome**

> [Outcome]

**Current Signals**

> [Signals]

**Current Milestone**

> [Milestone]

**Current WIP Limit**

> [1–2 major items]

**Next Decision / Milestone**

> [Next priority]

---

# 16. Project North Star

> **Start with the decision.
> Govern the meaning.
> Define the signals.
> Build only what creates value.
> Enable action.
> Measure the outcome.
> Learn and improve.**

---

PROJECT CHARTER
       │
       │  Why / North Star / Decision
       ▼
     IRD
       │
       │  Is the decision sufficiently defined?
       ▼
     DVC
       │
       │  Decision Validation Contract
       ▼
     DQE
       │
       │  Is the decision/data quality sufficient?
       ▼
     BRD
       │
       │  Business requirements
       ▼
     TRD
       │
       │  Technical implementation
       ▼
     BUILD
       │
       ▼
      QA
       │
       ▼
     DAL
       │
       │  Decision Adoption Layer
       ▼
     ACT → LEARN