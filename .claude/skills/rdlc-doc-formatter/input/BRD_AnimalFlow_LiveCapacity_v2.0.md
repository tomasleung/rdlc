Business Requirements Document

RDLC Phase‑1 Artifact — Animal Flow: Live Capacity Dashboard
# Document Metadata


| **Field** | **Value** |
| --- | --- |
| RDLC Document Type | BRD |
| RDLC Phase | Phase 1 Business Requirement |
| RDLC Stage | Design |
| Capability Name | Animal Flow — Live Capacity Management |
| Capability Domain | Operations / Animal Flow |
| Department | Animal Flow |
| Author | Tomas Leung |
| Version | 2.0 |
| Status | Draft |
| Created Date | 2026-07-07 |
| Last Update | 2026-07-07 |
| Decision Support | Support real-time capacity management and animal placement decisions by providing accurate space availability, centre confirmations, floor plan visibility, and Shelter Buddy assignment validation across all Community Animal Centres. |
# SECTION 01 — SUMMARY
This initiative provides BC SPCA Community Animal Centres (CACs) and the Animal Flow team with a centralized platform for managing, validating, and monitoring live animal housing capacity.

Unlike the original dashboard concept, the solution now combines:

- Floor plan management

- Capacity configuration

- ShelterBuddy occupancy validation

- Data quality monitoring

- Centre-level capacity confirmation

- Real-time intake support

The platform allows Centre Managers to manage and validate capacity information while enabling Animal Flow to make faster and more reliable animal placement decisions based on accurate, up-to-date operational data.

The focus is on operational accuracy, decision confidence, and data quality while remaining easy to use across all centres.
# SECTION 02 — BUSINESS PROBLEM
Historically, Animal Flow relied on phone calls, emails, text messages, and local knowledge to determine available capacity within Community Animal Centres.

**Key challenges included:**

- No centralized view of DOG and CAT availability

- No floor plan visibility

- No verification of Shelter Buddy kennel assignments

- Inconsistent understanding of available spaces

- Limited visibility into stale or incomplete data

- Manual effort required to validate capacity

**As a result:**

- Intake decisions take longer than necessary

- Animals may be routed to centres with inaccurate capacity information

- Staff spend significant time verifying information

- Data quality issues remain hidden until operational problems occur

**Process Flow:**

| **Before**  | **Now** |  |
| --- | --- | --- |
| Need Placement        ↓Contact Centres        ↓Gather Capacity Information        ↓Make Decision | **Centre Manager**Review Floor Plan        ↓Manage Open / Hold / Unavailable Spaces        ↓Review Shelter Buddy Assignments        ↓Correct Data Issues        ↓Confirm Capacity | **Animal Flow**Need Placement        ↓View Live Capacity        ↓Review Available Spaces        ↓Review Data Freshness        ↓Select Best CAC        ↓Route Animal |
# SECTION 03 — DECISION SUPPORTED
The platform supports two primary decision groups.

**Decision A — Animal Placement Decision**

**Decision to be Made**

Which CAC should receive an incoming animal based on available DOG or CAT capacity and current operational status?

**Decision Owner**

Animal Flow Team

**Frequency**

Multiple times per day.

**Why Timing Matters**

Animals require timely placement to avoid delays, transportation inefficiencies, and animal welfare risks.

**Decision B — Capacity Validation Decision**

**Decision to be Made**

Does the current floor plan, kennel assignment, and space availability accurately reflect the true operational state of the centre?

**Decision C — Occupancy Investigation**

**Decision to be Made**

Are the animals currently assigned within Shelter Buddy accurately represented within the configured capacity model?

**Decision Owner**

Centre Manager

**Frequency**

Daily and as changes occur.

**Why Timing Matters**

Incorrect or missing assignments reduce confidence in live capacity decisions and create operational risk.
# SECTION 04 — CURRENT PROCESS (AS‑IS)
**Current operational process:**

1. Centre capacity information is maintained locally.

2. Animal assignments occur in Shelter Buddy.

3. Capacity validation is primarily manual.

4. Animal Flow verifies space through calls, emails, or local knowledge.

5. Intake decisions are made using best available information.

**Limitations:**

- No unified operational platform

- No visual floor plan

- No automated validation of occupancy

- Difficult to identify missing kennel assignments

- High dependency on manual communication
# SECTION 05 — DESIRED CAPABILITY (TO‑BE)
The future state provides a centralized capacity management platform that combines master facility configuration, Shelter Buddy animal occupancy, and operational validation.

The platform provides:

**For Centre Managers**

- Interactive CAT and DOG floor plans

- Open / Hold / Unavailable space management

- Shelter Buddy assignment validation

- Missing kennel assignment identification

- Emergency closure controls

- Shift confirmation process

**For Animal Flow**

- Real-time space availability

- Capacity utilization monitoring

- Centre comparison

- Data freshness indicators

- Intake support decisions

**Animal Occupancy Validation**

The platform shall provide visibility into the individual animals contributing to space utilization.

Users must be able to:

- View animals occupying configured spaces

- Review Shelter Buddy assignment details

- Identify animals missing kennel assignments

- Investigate occupancy discrepancies

- Navigate directly to Shelter Buddy animal records for further review

**Benefits include:**

- Faster placement decisions

- Increased trust in capacity data

- Reduced manual validation effort

- Improved Shelter Buddy data quality

- Better operational visibility across all CACs
# SECTION 06 — BUSINESS QUESTIONS
**Capacity Management (In Scope)**

- Which centres have available space right now?

- Which centres are approaching capacity limits?

- Which centres cannot receive additional animals?

- How fresh is the centre capacity information?

- Has the centre confirmed its capacity status?

**Data Quality (In Scope)**

- Are all animals assigned to valid kennels?

- Are any animals missing kennel assignments?

- Are Shelter Buddy assignments aligned with configured floor plans?

- Are configured spaces being used correctly?

**Future Analytics (Out of Scope)**

- Seasonal capacity trends

- Capacity forecasting

- Historical utilization analysis

- Predictive placement modeling
# SECTION 07 — REQUIRED INFORMATION (KEY SIGNALS)
**Capacity Signals**

| **Signal** | **Description** |
| --- | --- |
| Total Spaces | Total configured DOG/CAT spaces |
| Open Spaces | Available for animal placement |
| In Use Spaces | Occupied spaces based on ShelterBuddy |
| Hold Spaces | Temporarily unavailable for intake |
| Unavailable Spaces | Permanently or operationally unavailable |
| Utilization Rate | Percentage of occupied spaces |
| Emergency Closure Status | Centre-level closure flag |
**Data Quality Signals**

| **Signal** | **Description** |
| --- | --- |
| Missing Kennel Assignment Count | Animals assigned to shelter location without kennel |
| Assignment Accuracy | Percentage of valid space assignments |
| Shelter Buddy Sync Time | Last successful synchronization |
| Capacity Confirmation Status | Centre manager sign-off status |
**Governance**

Master space configuration, kennel definitions, and capacity rules must be approved by Operations and maintained through formal change control.
# SECTION 08 — DATA SOURCES SharePoint List


| **Data Domain** | **Source System** | **Purpose** |
| :---: | :---: | :---: |
| Dog Capacity Configuration | DOG Master | Defines all dog spaces |
| Cat Capacity Configuration | CAT Master | Defines all cat spaces |
| Centre Configuration | CENTER Master | Defines all Centre spec |
| Shelter Occupancy | Shelter Buddy | Animal occupancy source |
| Capacity Confirmation | Power App | Centre sign-off |
| Space Status Management | Power App | Open/Hold/Unavailable control |
| Capacity Rollup | Live Capacity Platform | Operational dashboard |
| Historical Archive | Daily Snapshot | Future analytics |
# SECTION 09 — SCOPE
**In Scope**

- DOG floor plan management

- CAT floor plan management

- Space configuration management

- Open/Hold/Unavailable management

- Shelter Buddy occupancy validation

- Missing kennel assignment detection

- Capacity confirmation workflow

- Live capacity reporting

Intake support decision making

**Out of Scope**

- Predictive capacity forecasting

- Automated routing recommendations

- Historical trend reporting

- APS automation

- Advanced capacity optimization analytics
# SECTION 10 — ASSUMPTIONS & CONSTRAINTS
**Assumptions**

- Shelter Buddy remains the authoritative source for animal occupancy.

- DOG and CAT Master configurations accurately represent physical spaces.

- Centre Managers maintain space status information.

- Animal Flow relies on the platform for intake decisions.

**Constraints**

- Dependent on Shelter Buddy data quality.

- Kennel assignment remains optional in Shelter Buddy.

- Centres may experience connectivity limitations.

- Platform represents current operational state, not future reservations or animals in transit.
# SECTION 11 — SUCCESS CRITERIA
**Capacity Management**

- 90%+ of centres maintain current capacity information.

- Animal Flow can identify intake locations without direct centre contact.

**Data Quality**

- Missing kennel assignments reduced by 80%.

- Assignment validation is reviewed by Centre Managers.

**Operational Efficiency**

- Intake placement decisions occur 30% faster.

- Reduction in manual communication between centres and Animal Flow.

- Occupancy discrepancies can be investigated within one workflow.

**Adoption**

- Live Capacity Management becomes the primary operational capacity tool for all CACs.
# SECTION 12 — DATA VALIDATION & QA
- Occupancy counts must reconcile with Shelter Buddy. 

- Capacity totals must reconcile with configured floor plans. 

- Missing assignments must be surfaced automatically. 

- Data freshness indicators must be accurate. 

- Confirmation audit history must be retained.

**Note**: This BRD defines decision-making and governance contracts only. All system design, data modeling, automation, and analytics implementation details will be defined in the Technical Requirement Document (TRD).
# SECTION 13 — ACCEPTANCE CRITERIA
**Governance & Accountability**

- Product Owner approval obtained.

- Data Owner approval obtained.

- Capacity governance rules documented.

**Operational Readiness**

- Floor plans accurately reflect configured spaces.

- Open/Hold/Unavailable statuses function correctly.

- Capacity confirmation process operates successfully.

**Data Integrity**

- Shelter Buddy occupancy reconciles with platform counts.

- Missing kennel assignments are identified and reported.

- Capacity calculations are validated across pilot centres.
# SECTION 14 — APPROVALS


| **Role** | **Name** | **Approval Status** |
| --- | --- | --- |
| Product Owner | Cynthia Boulter |  |
| Data Owner | Kahlee Demers |  |
| Data Architecture | Tomas Leung | Pending |

