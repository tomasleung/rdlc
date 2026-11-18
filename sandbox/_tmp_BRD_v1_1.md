**Business Requirements Document**

*RDLC Phase-1 Artifact --- Animal Flow: Foster Pathway Analytics
(Counter Scope)*

**Document Metadata**

  ------------------------ ----------------------------------------------
  **Field**                **Value**

  RDLC Document Type       BRD

  RDLC Phase               Phase 1 Business Requirement --- Counter Scope

  RDLC Stage               Design

  Capability Name          Animal Flow --- Foster Analysis

  Capability Domain        Operations / Animal Flow

  Department               Animal Flow

  Author                   Tomas Leung

  Version                  1.1

  Status                   Draft

  Supersedes               BRD_Foster_Analysis_v1_0 --- KPI Contract
                           (§7.2) only; all other sections carried
                           forward

  Created Date             2026-07-06

  Last Update              2026-08-31

  Decision Support         Understand how many distinct animals
                           experience foster care, and how much foster
                           placement activity occurs, as the foundational
                           baseline before rate-based and
                           program-segmented KPIs are introduced.
  ------------------------ ----------------------------------------------

**Scope note --- why this document exists**

This is a narrowed revision of BRD_Foster_Analysis_v1.0, scoped
specifically to what Phase 1 will actually deliver first: two
foundational counters --- Fostered Animals and Foster Placement Count
--- built on Fact_FosterEpisode, a fact table discovered to be necessary
once real ShelterBuddy foster-history data (silver.kennel_history_scd2)
was reviewed. Foster Placement Rate % and driver-variance analysis, both
named in the original BRD\'s Phase 1 scope, are deferred until this
two-counter foundation is validated against real data --- see §5 and §9
for the explicit deferral record.

**SECTION 01 --- SUMMARY**

Enable the Animal Flow team to answer two foundational questions with a
governed, trusted number: how many distinct animals experienced foster
care, and how much foster placement activity occurred --- both broken
down by month and year. This is the baseline layer the rest of Foster
Pathway Analytics builds on.

The solution will follow a phased approach. This document narrows Phase
1 to exactly two KPIs --- Fostered Animals and Foster Placement Count
--- rather than the broader Phase 1 scope in the original BRD, because
building the semantic model correctly for even these two numbers
required discovering and modeling a new fact table (Fact_FosterEpisode)
that the original design did not anticipate.

By standardizing how these two numbers are counted --- with explicit,
worked-example-verified definitions --- this solution replaces
inconsistent manual counts with one governed answer, extendable to the
fuller KPI set once this foundation is proven.

**SECTION 02 --- BUSINESS PROBLEM**

Current ShelterBuddy reporting does not support accurate foster
tracking. The original BRD identified this at the status-field level;
direct review of the underlying source data (silver.kennel_history_scd2)
confirmed a more specific structural problem: an animal\'s foster
history is not a single flag, it is a sequence of separate location-stay
episodes, and an animal can move in and out of foster more than once
during a single care journey.

**Key Challenges:**

> • Over-reliance on non-granular or inconsistent status fields
>
> • Inability to:
>
> ◦ Accurately count distinct animals in foster, by month/year
>
> ◦ Count foster placement activity (episodes), independent of animal
> identity
>
> ◦ Distinguish genuine foster placements from \"%foster adoption%\"
> locations, a related but distinct program
>
> • Manual extraction and transformation processes
>
> • Lack of standardized, worked-example-verified definitions across
> teams

**Result:**

> • Limited visibility into how many animals actually use foster care
>
> • Risk of double-counting or under-counting animals with multiple
> foster episodes
>
> • No governed distinction between \"how many animals\" and \"how much
> placement activity\" --- two different, both useful, numbers

**SECTION 03 --- DECISION SUPPORTED**

This solution supports progressive decision-making across three phases;
this document narrows only the Phase 1 deliverable, not the overall
roadmap.

**Phase 1 Decision**

Are we effectively using foster as part of the animal journey after
intake --- starting from a trustworthy count of how many animals and how
much placement activity, before layering in rate and driver analysis?

**Phase 2 Decision**

Which programs (e.g., EAB, LSAI) are effectively leveraging foster
placements?

**Phase 3 Decision**

How can foster placements be optimized to improve outcomes and reduce
time in care?

**Decision Owner:**

Animal Flow Manager (named individual on file; withheld from this
artifact for privacy --- confirm via Section 15 sign-off).

**Frequency:**

> • Phase 1: Weekly / Monthly monitoring
>
> • Phase 2+: Program and operational reviews

**SECTION 04 --- CURRENT PROCESS (AS-IS)**

Current workflow relies on manual extraction and interpretation of data
from ShelterBuddy.

**Process:**

> • Users extract reports from ShelterBuddy
>
> • Data is manually cleaned and transformed
>
> • Foster identification is inconsistent
>
> • Metrics are calculated outside the system

**Newly confirmed:**

> • Raw foster-history data does exist in a queryable form ---
> \[silver\].\[kennel_history_scd2\] in the Fabric silver layer, sourced
> from ShelterBuddy via API --- but is not yet loaded into the
> Fact_AnimalIntake star schema, and no Fact_FosterEpisode table exists
> yet

**Limitations:**

> • No standardized, worked-example-verified definitions
>
> • No single source of truth
>
> • High manual effort
>
> • Inconsistent reporting across users
>
> • No existing fact table at the correct grain (foster episode, not
> intake) to answer either target KPI correctly

**SECTION 05 --- DESIRED CAPABILITY (TO-BE)**

A governed analytics system that evolves from two foundational counters
to full lifecycle pathway tracking with program and outcome insights.

**Phase 1 (this document\'s scope) --- Two Counters Only**

> • Fostered Animals --- distinct animals (by SB Animal_Id) with an
> active foster placement in the selected period
>
> • Foster Placement Count --- count of foster placement episodes that
> started in the selected period (transaction count)

**Explicitly deferred from Phase 1 (see §9)**

> • Foster Placement Rate % --- named in the original BRD\'s Phase 1
> scope; deferred pending confirmation that Fact_FosterEpisode\'s
> denominator logic (rate against what population?) is settled with the
> Data Owner
>
> • Placement Rate variance by species/region/intake type --- deferred
> to the same review

**Phase 2**

> • Ability to segment foster usage by Program (EAB, LSAI) and Intake
> type

**Phase 3**

> • Full pathway tracking: Intake → Program → Foster → Outcome
>
> • Placement-level metrics: Duration, Effort, Outcome effectiveness
>
> • Foster segmentation: Staff vs Volunteer, Foster-to-Adopt logic

**SECTION 06 --- BUSINESS QUESTIONS**

**Phase 1 --- Baseline (In Scope for this document)**

> • How many distinct animals experienced foster care in a given month
> or year?
>
> • How much foster placement activity (episode volume) occurred in a
> given month or year?
>
> • Do these two numbers move together, or does placement activity grow
> faster/slower than distinct-animal count --- an early signal of churn
> vs. genuine demand growth?

**Deferred --- Phase 1 Extension (pending Data Owner review, see §9)**

> • What percentage of intake animals were fostered?
>
> • How does foster placement vary by species, region, and intake type?

**Phase 2 --- Program Analysis**

> • What % of EAB animals are placed in foster?
>
> • What % of LSAI animals are placed in foster?
>
> • How does foster usage vary across programs?

**Phase 3 --- Pathway & Outcomes**

> • How long do animals stay in foster?
>
> • What outcomes result after foster placement?
>
> • Are foster placements improving outcomes?
>
> • Where are bottlenecks in the foster process?

**SECTION 07 --- REQUIRED INFORMATION (KEY SIGNALS + KPI CONTRACT)**

Signals are sourced from \[silver\].\[kennel_history_scd2\] (Fabric
silver layer, via ShelterBuddy API) and Fact_AnimalIntake --- not the
Program/Placement-Date/Outcome/Region signal set named in the original
BRD, which assumed intake-grain foster tracking.

**7.1 Core Signals**

  -----------------------------------------------------------------------
  **Signal**               **Description**
  ------------------------ ----------------------------------------------
  Animal_Id                ShelterBuddy animal identifier --- links
                           foster episodes to the same animal

  IntakeDate /             The intake event that began this animal\'s
  LinkedIntakeID           current care journey (FK to Fact_AnimalIntake)

  ShelterLocation_Name     Raw location name from kennel history --- used
                           to identify foster segments and exclude
                           \"%foster adoption%\"

  Is_Foster                0/1 flag on each location-stay segment ---
                           whether that segment is a foster placement

  FosterStartDate          Segment start date --- drives both KPI
                           calculations

  FosterEndDate            Segment end date (nullable --- ongoing
                           placement)

  IsCurrent                SCD2 flag --- whether this segment is the
                           animal\'s current status
  -----------------------------------------------------------------------

**7.2 KPI Contract (Phase 1 --- this document\'s scope)**

**KPI: Fostered Animals**

  ------------------ ----------------------------------------------------
  **Attribute**      **Definition**

  Definition         Distinct animals (by Animal_Id) with a foster
                     placement active on any day within the selected
                     period. An animal counts once per period even if it
                     has multiple foster episodes touching that period;
                     the same animal can appear in more than one period
                     (month/year) if its placement(s) span or repeat
                     across periods.

  Grain              Animal, evaluated per reporting period (month or
                     year)

  Owner              Animal Flow Manager

  Filter             Is_Foster = 1, excluding locations matching
                     \"%foster adoption%\"

  Source             Fact_FosterEpisode (new --- derived from
                     silver.kennel_history_scd2)

  Type               Operational KPI --- period-window distinct count

  Reference          Fostered_Animals_Counting_Logic_v0_1.docx --- worked
                     example (Animal A, two episodes across 2025--2026)
  ------------------ ----------------------------------------------------

**KPI: Foster Placement Count**

  ------------------ ----------------------------------------------------
  **Attribute**      **Definition**

  Definition         Count of foster placement episodes (segments where
                     Is_Foster = 1) whose start date falls within the
                     selected period --- a transaction count, not
                     deduplicated by animal. An animal placed twice in
                     the same period contributes 2, not 1.

  Grain              Foster placement episode, attributed to its
                     FosterStartDate

  Owner              Animal Flow Manager

  Filter             Is_Foster = 1, excluding locations matching
                     \"%foster adoption%\"; counted by FosterStartDate
                     only --- a placement that continues into a later
                     period contributes nothing to that later period\'s
                     count

  Source             Fact_FosterEpisode (new --- derived from
                     silver.kennel_history_scd2)

  Type               Operational KPI --- transaction count

  Reference          Foster_Placement_Count_Counting_Logic_v0_1.docx ---
                     worked example, including side-by-side contrast with
                     Fostered Animals
  ------------------ ----------------------------------------------------

**7.3 Governance --- Ownership & Change Control**

> • Foster episode identification logic (Is_Foster flag, \"%foster
> adoption%\" exclusion) must be confirmed and approved by the Data
> Owner before go-live
>
> • Both KPI definitions above must be approved by the Data Owner,
> matching the worked examples in their reference documents exactly ---
> no numeric or logic changes without a versioned update to both this
> contract and the reference document
>
> • Any changes must follow RDLC change control

**7.4 Data Contract (Phase 1)**

Minimum required:

> • Animal_Id
>
> • ShelterLocation_Name (or pre-computed Is_Foster flag)
>
> • FosterStartDate, FosterEndDate
>
> • LinkedIntakeID (traceability to Fact_AnimalIntake)

**SECTION 08 --- DELIVERY PHASES**

**Phase 1 (this document)**

Build Fact_FosterEpisode; deliver Fostered Animals and Foster Placement
Count, by month and year, with worked-example-verified definitions.

**Phase 1 Extension (pending Data Owner review)**

Foster Placement Rate % and driver-variance analysis
(species/region/intake type), once the denominator logic against
Fact_FosterEpisode is confirmed.

**Phase 2 --- Program Analysis**

Segment foster usage by EAB / LSAI.

**Phase 3 --- Full Pathway**

Placement duration, outcomes, and optimization.

**SECTION 09 --- SCOPE**

This document\'s scope is intentionally narrower than the original
BRD\'s Phase 1: exactly two KPIs, both counters, both grounded in a new
fact table discovered necessary during design review of real
ShelterBuddy foster-history data.

**Explicitly in scope:**

> • Fostered Animals (distinct-animal, period-window count)
>
> • Foster Placement Count (transaction count, start-date attributed)
>
> • Fact_FosterEpisode design and build

**Explicitly deferred, not dropped:**

> • Foster Placement Rate % --- needs Data Owner confirmation of
> denominator logic against the new fact table before re-entering Phase
> 1 scope
>
> • Driver variance (species/region/intake/age) --- Phase 1 stretch per
> the EDA Specification, not blocked by this document but not delivered
> by it either
>
> • Foster caregiver/home identity, capacity, duration, outcome ---
> unchanged from original BRD, still out of scope (Phase 2/3)

**SECTION 10 --- DATA SOURCES**

  ----------------------------------------------------------------------------------------------
  **Data Domain**     **Source**                           **Frequency**     **Purpose**
  ------------------- ------------------------------------ ----------------- -------------------
  Animal Intake Data  Shelter Buddy                        Daily             Core intake record;
                                                                             links foster
                                                                             episodes to a care
                                                                             journey

  Foster Episode      \[silver\].\[kennel_history_scd2\]   Daily / SCD2      Identify foster
  History             (Fabric silver, via ShelterBuddy     (change-driven)   placement episodes
                      API)                                                   ---
                                                                             start/end/current
                                                                             status

  Location Data       Shelter Buddy                        Static            Distinguishes
                                                                             foster vs.
                                                                             foster-adoption vs.
                                                                             other locations
  ----------------------------------------------------------------------------------------------

**SECTION 11 --- ASSUMPTIONS & CONSTRAINTS**

**Assumptions**

> • Is_Foster, as computed upstream in the silver layer, correctly
> excludes \"%foster adoption%\" locations --- to be confirmed, not
> assumed, before go-live (§7.3)
>
> • Animal_Id is stable and consistent between Fact_AnimalIntake and
> kennel_history_scd2
>
> • Data is consistently updated

**Constraints**

> • Dependent on ShelterBuddy / Fabric silver-layer data quality
>
> • Historical window spans 2005--present (Data Owner-confirmed) --- a
> materially longer range than the 5-year window assumed in the EDA
> Specification, increasing the chance of an undocumented structural
> break (system migration, policy change); the EDA Specification\'s
> stability check becomes load-bearing, not optional, before this window
> is trusted for any future threshold baseline
>
> • Data Quality Memo not yet run --- readiness is Amber, not Green,
> until it completes and is benchmarked against last year\'s known
> figures (per Data Owner-confirmed QA method)

**SECTION 12 --- SUCCESS CRITERIA**

Clear, governed visibility into how many animals used foster care and
how much placement activity occurred --- the trusted foundation the rest
of the roadmap builds on.

> • Fostered Animals and Foster Placement Count match their respective
> worked-example reference documents exactly
>
> • Both KPIs\' definitions are Data-Owner approved before any report
> page goes live
>
> • Users can correctly explain why the two numbers can legitimately
> disagree for the same period (see reference docs\' side-by-side
> comparison)

**SECTION 13 --- DATA VALIDATION & QA**

> • Fact_FosterEpisode counts reconciled against last year\'s known
> benchmark figures (Data Owner-confirmed method)
>
> • No animal double-counted within a single period for Fostered Animals
>
> • Foster Placement Count ties to distinct FosterEpisodeID records,
> correctly attributed by start date
>
> • \"%foster adoption%\" locations confirmed excluded from both KPIs
>
> • No discrepancies across filters (month, year, combined ranges)

**SECTION 14 --- ACCEPTANCE CRITERIA**

> • Data Owner approval of both KPI definitions (§7.2) and the Is_Foster
> exclusion logic (§7.3)
>
> • Fact_FosterEpisode built and validated against the Animal A worked
> example in both reference documents
>
> • Both counters operational and trusted by stakeholders
>
> • Deferred items (§9) logged for explicit follow-up --- not silently
> dropped

**SECTION 15 --- APPROVALS**

  ------------------------------------------------------------------------
  **Role**                    **Name**                      **Approval
                                                            Status**
  --------------------------- ----------------------------- --------------
  Product Owner               Maggie Stewart                Pending

  Data Owner                  Cynthia Boulter               Pending

  Data Architecture           Tomas Leung                   Pending

  Decision Owner (Animal Flow On file --- withheld from     Pending
  Manager)                    this artifact                 
  ------------------------------------------------------------------------
