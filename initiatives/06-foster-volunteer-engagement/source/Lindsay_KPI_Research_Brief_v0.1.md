# BC SPCA Foster Volunteer KPI Research — Master Context & Governance Brief

**Status:** [RESEARCH] Phase — KPI Definition & Business Rules (Pre-Schema / Pre-Design)
**Data Owner:** Lindsay (Foster Volunteer Member Support)
**Technical Architect:** Tomas Leung (Senior BI Developer & Data Architect, BC SPCA)
**Strategic Alignment:** Director's primary objective is to **reduce unnecessary days in care (Length of Stay)** across BC SPCA's ~33 animal centres.

---

## 1. Operational Reality & Hard Data Constraints (Ground Truth)
* **Decentralized Execution:** All ~33 BC SPCA centres operate independently when placing animals into foster care.
* **High-Risk ("Noise") Fields:** Branches do **not** reliably log soft workflow milestones (e.g., `Foster_Eligible_Date`) and do **not** update volunteer `Status` (`Active` vs. `Archived`) in real time. "Archiving" (changing a profile from Active to Closed/Inactive without deleting history) often happens in delayed bulk administrative cleanups months or years after a volunteer stops fostering.
* **Guaranteed ("Signal") Fields:** Every centre is operationally forced to create a placement record in **ShelterBuddy (SB)** linking **`AnimalID` + `PersonID`** (with placement start/end timestamps) to track animal custody. **All core KPIs must be 100% anchored on verified `AnimalID`–`PersonID` links.**

---

## 2. Starting Baseline: Data Owner's Initial Draft Definitions
Lindsay initially proposed treating foster volunteer support like a traditional membership model:
1. **Retention (Draft):** `# of fosters that stay (i.e. remain active) with BC SPCA > 1 yr`
2. **Attrition (Draft):** `# of fosters lost (i.e. # archived) every year/quarter/etc.`

### Why Pure Membership Definitions Fail Operational Decisions (Audit Findings)
* **The "Ghost Volunteer" Illusion:** A volunteer who fostered once in 2024 and ignored emails all of 2025 remains `Active` until manually archived—inflating `> 1 yr` retention.
* **Lagging Administrative Spikes:** Counting `# archived` per quarter measures *when staff clean up the database*, not *when volunteers actually stopped taking animals*.
* **Disconnected from Director's Goal:** Counting un-archived profiles tells leadership nothing about whether fosters are actively relieving physical shelter kennels or reducing unnecessary days in care.

---

## 3. Recommended KPI Architecture (Decision-First Signal Layer)
Keep Lindsay's two status counts strictly as **Layer 1: Admin Roster Hygiene**, and establish **Layer 2: Operational Foster Signals** built exclusively from `AnimalID` + `PersonID` placement records (excluding Foster-to-Adopt / FTA placements from volunteer churn):

| Operational Decision (WHY) | Recommended KPI (SIGNAL) | Exact `AnimalID` + `PersonID` Logic | Connection to Lindsay & Director |
| :--- | :--- | :--- | :--- |
| **1. Do we have real surge capacity today?** | **Active Foster Pool & Utilization %** | `PersonID`s with ≥ 1 `AnimalID` linked in the **last 90 days** ÷ Total `PersonID`s linked in the **last 12 months**. | Replaces "un-archived roster size" with verified working foster homes. |
| **2. Are new volunteers supported well enough to return?** | **1st-to-2nd Placement Conversion %** | % of new `PersonID`s (first-ever `AnimalID` link) who link a **2nd `AnimalID` within 180 days**. | Directly inside Lindsay's ownership; catches early burnout without waiting for branch `Archive` dates. |
| **3. Is our core volunteer base loyal year-over-year?** | **12-Month Repeat Retention %** *(Upgraded Retention)* | % of `PersonID`s with ≥ 1 `AnimalID` in Year T-1 **AND** ≥ 1 `AnimalID` in Year T0 (or active placement span > 365 days). | Upgrades Lindsay's `> 1 yr` rule from passive mailing-list tenure to active repeat care. |
| **4. When are we actually losing foster homes?** | **Effective Behavioral Attrition** *(Upgraded Attrition)* | `PersonID`s crossing **365 consecutive days with zero `AnimalID` links** OR formal `Archive_Date` (whichever happens **first**). | Smooths out late branch database cleanups into an honest quarterly/annual churn curve. |
| **5. How much kennel relief did foster deliver?** | **Shelter Days Saved (In-Foster Care Days)** | Sum of `(Placement_End - Placement_Start)` across all linked `AnimalID`–`PersonID` pairs (separating **In-Shelter LOS** from **Foster Care Days**). | Proves foster's direct contribution to the Director's #1 goal (*reducing unnecessary in-shelter days*). |

---

## 4. Open Research Questions & Guardrails for Next AI Session
* **Current Stage:** Still in **KPI Definition & Business Rule Validation** with Lindsay (do **not** jump to SQL/DAX/schema design unless explicitly instructed).
* **Framework Rules:** Apply **Decision-Driven BI** principles—every metric must map to a concrete operational action, distinguish Signal vs. Noise, and maintain Human-in-the-Loop governance.
* **Next Topics to Explore:**
  1. Edge-case boundary rules (e.g., how to classify **Foster-to-Adopt (FTA)**, **seasonal summer-only kitten fosters**, and **approved applicants who never received a 1st animal**).
  2. Preparing concrete numerical examples (e.g., 5 sample volunteer timelines) to walk Lindsay through the gap between `Archive` status and `AnimalID`–`PersonID` activity.

---
*Pasted into chat and saved to this repo 2026-11-18, per the project's new rule that initiative source material must exist as a real file, not only as chat history.*
