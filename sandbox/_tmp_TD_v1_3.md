**TABLE DEFINITIONS --- FOSTER EPISODE ANALYTICS v1.3**

Animal Flow --- Foster Capacity Analysis (Category 2)

*Semantic Model --- Schema Confirmed Against Real MS Fabric Gold Table*

  -------------------------------------------------------------------------
  **Field**               **Value**
  ----------------------- -------------------------------------------------
  Document Type           Semantic Model / Table Definitions

  RDLC Phase              Category 2 --- Capacity & Support (new subject
                          area, separate from Phase 1 Fact_AnimalIntake)

  Capability              Animal Flow --- Foster Analysis

  Backend Platform        Microsoft Fabric

  Version                 1.3

  Status                  Draft --- all 3 prior CONFIRM items resolved;
                          ready for handoff gate review

  Supersedes              TABLE_DEFINITIONS_FOSTER_EPISODE_ANALYTICS_v1.2

  Related Documents       Foster_KPI_Catalog_Category2_Scoped_v0.1.xlsx
                          (KPI definitions this table supports)

  Author                  Tomas Leung
  -------------------------------------------------------------------------

> *Change from v1.2: All 3 CONFIRM items resolved --- see Section 8a
> (Resolved). Column nullability verified directly against the real MS
> Fabric gold table (gold_foster.fact_foster_placement) and corrected
> below. This document defines schema/data definitions only --- the live
> Fabric warehouse is the source-of-truth reference for schema shape,
> but the actual Phase 1 build still runs against mock CSV data,
> consistent with the main Foster Analysis project.*

**8. Semantic Model Configuration Standards**

**8.1 Business Naming Standard**

The Power BI Semantic Model must expose business-friendly names and hide
technical implementation details.

**Fact_FosterPlacement**

  --------------------------------------------------------------------------
  **Technical Name**            **Business Name**              **Visible**
  ----------------------------- ------------------------------ -------------
  PlacementId                   Foster Placement ID            Hidden

  AnimalId                      SB Animal ID                   Hidden

  ShelterLocationName           Foster Location                Visible

  StartDateKey                  Foster Start Date Key          Hidden

  EndDateKey                    Foster End Date Key            Hidden

  DurationDays                  Foster Duration (Days)         Visible

  IsCurrent                     Currently In Foster            Visible
  --------------------------------------------------------------------------

**Fact_FosterAnimalMonth**

  ------------------------------------------------------------------------
  **Technical Name**         **Business Name**               **Visible**
  -------------------------- ------------------------------- -------------
  PlacementId                Foster Placement ID             Hidden

  AnimalId                   SB Animal ID                    Hidden

  MonthDateKey               Month Date Key                  Hidden
  ------------------------------------------------------------------------

**Dim_Date**

  -----------------------------------------------------------------------
  **Technical Name**                    **Business Name**
  ------------------------------------- ---------------------------------
  DateKey                               Date Key

  Date                                  Date

  Year                                  Year

  Quarter                               Quarter

  Month                                 Month

  MonthName                             Month Name

  MonthShortName                        Month (Short)

  YearMonth                             Year-Month

  Week                                  Week

  Season                                Season
  -----------------------------------------------------------------------

------------------------------------------------------------------------

**8.2 Foster Animal Month Reporting Guidance**

Fact_FosterAnimalMonth is a monthly snapshot table.

MonthDateKey always represents:

Plain Text

1

First Day Of Month (YYYYMM01)

2

 

Show more lines

and exists only to provide a relationship to the shared Date dimension.

**Supported Analysis**

Plain Text

1

Year

2

Quarter

3

Month

4

Year-Month

Show more lines

**Not Supported**

Plain Text

1

Day

2

Week

Show more lines

Reason:

Plain Text

1

The table stores one row per Animal per Month touched by Foster.

2

It does not represent daily activity.

Show more lines

------------------------------------------------------------------------

**8.3 Semantic Model Table Visibility**

**Visible Tables**

Plain Text

1

Date

2

Foster Placements

Show more lines

**Hidden Tables**

Plain Text

1

Fact_FosterAnimalMonth

Show more lines

Reason:

Plain Text

1

Fact_FosterAnimalMonth exists solely to support the Fostered Animals KPI
and should not be used directly by report authors.

Show more lines

------------------------------------------------------------------------

**8.4 Measure Documentation Standard**

All published measures must include:

**Description**

Plain Text

1

Business definition.

Show more lines

**Grain**

Plain Text

1

Calculation grain.

Show more lines

**Usage Guidance**

Plain Text

1

Known reporting limitations.

Show more lines

------------------------------------------------------------------------

**Foster Placement Count**

Description:

Plain Text

1

Number of separate foster placements.

Show more lines

Grain:

Plain Text

1

Placement

Show more lines

Formula:

DAX

1

DISTINCTCOUNT ( \'Foster Placements\'\[Foster Placement ID\] )

Show more lines

------------------------------------------------------------------------

**Fostered Animals**

Description:

Plain Text

1

Distinct animals that were in foster during the selected month or year.

Show more lines

Grain:

Plain Text

1

Animal per Month

Show more lines

Usage Guidance:

Plain Text

1

Should be analyzed at Month, Quarter or Year level only.

2

Not intended for day-level reporting.

3

\`\`

Show more lines

Formula:

DAX

1

DISTINCTCOUNT ( Fact_FosterAnimalMonth\[SB Animal ID\] )

Show more lines

------------------------------------------------------------------------

**Current Foster Animals**

Description:

Plain Text

1

Distinct animals currently in foster.

Show more lines

Grain:

Plain Text

1

Animal

Show more lines

Formula:

DAX

1

CALCULATE(

2

DISTINCTCOUNT(\'Foster Placements\'\[SB Animal ID\]),

3

\'Foster Placements\'\[Currently In Foster\] = 1

4

)

Show more lines

------------------------------------------------------------------------

I would absolutely add this section because once we start building the
TMDL files, this document becomes the blueprint for the semantic model
configuration, not just the warehouse tables. That will make it much
easier to auto-generate the model later.

# **1. Purpose**

Define the semantic model required to support the two confirmed Category
2 (Capacity & Support) KPIs for this scope:

- Foster Placement Count --- how many separate foster stays occurred

- Fostered Animals --- how many distinct animals were in foster, by
  month/year

**Two additional columns (Duration, Current status) are included in the
physical design because they fall out naturally from the source data ---
but they are NOT presented as confirmed KPIs in this version. See
Section 5 and Known Gaps.**

# **2. Source & Business Logic**

**Source: slv.kennel_history_scd2 WHERE Is_Foster = 1 --- the
bronze-to-silver flag already built in the Fabric notebook pipeline (see
Fabric_Notebook_Function_Sharing_SOP_v0.1).**

> *RESOLVED (v1.3): the prior .isin(\'foster to adopt\') exact-match
> check missed the real variant \"Foster - To Adopt\" (confirmed against
> SB_Incoming_Mapping.xlsx). Fix: replace with a pattern match --- NOT
> ShelterLocation_Name.lower().contains(\'to adopt\') --- so all three
> known real spelling variants are excluded consistently. Confirmed by
> the human orchestrator: the fact table should include foster records
> only, which this fix is what actually achieves in practice. This
> should be fixed at the source (the shared notebook function), not
> worked around downstream.*

Business Logic (plain language): a foster placement begins when an
animal\'s Shelter Location changes to a value containing "foster" ---
but not the "Foster To Adoption" program (tracked separately, out of
scope here). It ends when the location changes away, or is still ongoing
(IsCurrent = 1).

> *Timezone handling (author\'s own addition, retained): Start/End Date
> Keys must be computed from the Vancouver local date, not the raw
> source timestamp\'s original timezone. A placement logged near
> midnight could otherwise be attributed to the wrong calendar day or
> month once localized. This applies to both fact tables below.*

# **3. Dimensional Model**

## **3.1 Date (technical: Dim_Date --- EXTENDS the existing conformed table)**

**This reuses the existing Dim_Date already built for Fact_AnimalIntake
(conformed dimension) --- not a new table. Two columns are added to
support this subject area\'s reporting needs.**

  ---------------------------------------------------------------------------------------
  **Technical Name **Business   **Type**     **Description**             **Build Notes**
  (Fabric)**       Name (Power                                           
                   BI)**                                                 
  ---------------- ------------ ------------ --------------------------- ----------------
  DateKey          Date Key     Int64        Existing --- surrogate/join Unchanged
                                (YYYYMMDD)   key                         

  Date             Date         DateTime     Existing --- calendar date  Unchanged

  Year             Year         Int64        Existing --- calendar year  Unchanged

  Quarter          Quarter      String       Existing --- calendar       Unchanged
                                             quarter                     

  Month            Month        Int64        Existing --- calendar month Unchanged
                                             number                      

  MonthName        Month Name   String       Existing --- full month     Unchanged
                                             name                        

  MonthShortName   Month        String       NEW --- 3-letter month      sortByColumn:
                   (Short)                   abbreviation (Jan--Dec),    Month
                                             for compact axis labels     

  YearMonth        Year-Month   String       NEW --- "YYYY-MM" format,   sortByColumn:
                                             for a clean, sortable       Date Key
                                             single trend axis across    
                                             years                       

  Week             Week         String       Existing --- ISO week label Unchanged

  Season           Season       String       Existing ---                Unchanged
                                             Winter/Spring/Summer/Fall   
  ---------------------------------------------------------------------------------------

> *RESOLVED (v1.3): Confirmed --- this extends the existing
> global/shared Dim_Date, not a separate table.*

# **4. Fact Table 1: Foster Placement**

## **Foster Placement (technical: Fact_FosterPlacement)**

**Grain: One row per foster placement (a stay --- one continuous trip to
foster, start to end).**

Description: The primary placement-level fact table. Supports Foster
Placement Count directly, and provides the source rows that
Fact_FosterAnimalMonth (Section 5) is derived from.

### **Foreign Keys**

- Start Date Key → Date

- End Date Key → Date

*Two relationships to the same conformed Date table --- one active, one
inactive by default (Power BI requires marking a second relationship to
the same dimension as inactive; activate the relevant one per measure
via USERELATIONSHIP as needed).*

### **Columns**

  --------------------------------------------------------------------------------------------------
  **Technical Name      **Business   **Type**   **Description**                     **Build Notes**
  (Fabric)**            Name (Power                                                 
                        BI)**                                                       
  --------------------- ------------ ---------- ----------------------------------- ----------------
  PlacementId           Placement ID Int64,     Unique foster placement identifier. Hidden --- key
                                     NULL       Generated --- not a native          column
                                                ShelterBuddy field. Nullable in the 
                                                real gold table --- see Section 8a  
                                                note.                               

  AnimalId              Animal ID    Int64,     Animal identifier. CONFIRMED        Hidden --- key
                                     NULL       against real                        column
                                                gold_foster.fact_foster_placement   
                                                schema --- Int64 is correct.        

  ShelterLocationName   Shelter      String,    Raw Shelter Location value that     
                        Location     NULL       triggered this placement ---        
                                                retained for QA/traceability back   
                                                to source.                          

  StartDateKey          Start Date   Int64      → Date. Vancouver local date.       Hidden --- FK
                        Key          (FK), NULL                                     column

  EndDateKey            End Date Key Int64      → Date. Vancouver local date. Null  Hidden --- FK
                                     (FK), NULL if placement is still ongoing.      column

  DurationDays          Duration     Int64,     Placement length in days. Column    Hidden --- not
                        (Days)       NULL       present; NOT a confirmed KPI in     yet promoted to
                                                this version --- see Known Gaps.    a measure

  IsCurrent             Is Current   Int64      1 if this placement is still active Hidden --- not
                                     (0/1),     (no End Date). Column present; NOT  yet promoted to
                                     NULL       a confirmed KPI in this version --- a measure
                                                see Known Gaps.                     
  --------------------------------------------------------------------------------------------------

# **5. Fact Table 2: Foster Animal Month**

## **Foster Animal Month (technical: Fact_FosterAnimalMonth)**

**Grain: One row per animal, per month touched by a foster placement.**

**Description: An internal, supporting table only --- not intended for
direct use by report authors. Its sole purpose is to correctly compute
Fostered Animals by month/year, since the same animal can have multiple,
non-overlapping placements that must still count as one animal per
month. Derived from Foster Placement (Section 4), not an independent
source.**

### **Foreign Keys**

- Month Date Key → Date

### **Columns**

  --------------------------------------------------------------------------------
  **Technical    **Business   **Type**   **Description**          **Build Notes**
  Name           Name (Power                                      
  (Fabric)**     BI)**                                            
  -------------- ------------ ---------- ------------------------ ----------------
  PlacementId    Placement ID Int64      Source placement this    Hidden ---
                                         row was derived from.    internal bridge
                                                                  table, not for
                                                                  direct report
                                                                  use

  AnimalId       Animal ID    Int64      Animal identifier.       Hidden ---
                                                                  internal bridge
                                                                  table, not for
                                                                  direct report
                                                                  use

  MonthDateKey   Month Date   Int64 (FK) → Date. First day of the Hidden --- FK
                 Key                     month this row           column
                                         represents.              
  --------------------------------------------------------------------------------

> *This entire table should be marked hidden in the semantic model (all
> columns, and ideally the table itself excluded from the default field
> list view) --- it exists purely to make the Fostered Animals measure
> compute correctly, not for ad-hoc exploration.*

# **6. KPI Definitions**

### **KPI: Foster Placement Count**

- Definition: Number of separate foster stays.

- Grain: One count per placement

- Formula: DISTINCTCOUNT(\'Foster Placement\'\[Placement ID\])

### **KPI: Fostered Animals**

- Definition: Distinct animals in foster at any point during the period
  (month or year).

- Grain: Distinct animal, per period

- Formula: DISTINCTCOUNT(\'Foster Animal Month\'\[Animal ID\])

- Computed from Foster Animal Month, not Foster Placement --- this is
  why the internal bridge table exists.

> *Not yet confirmed KPIs (columns exist, measures not yet built):
> Average Foster Duration (AVERAGE(\'Foster Placement\'\[Duration
> (Days)\])) and Current Foster Animals (DISTINCTCOUNT(Animal ID) WHERE
> Is Current = 1). Both are one step away from being built once formally
> confirmed as in-scope --- see Known Gaps.*

# **7. Relationship Summary**

- Foster Placement\[Start Date Key\] → Date\[Date Key\] (active by
  default)

- Foster Placement\[End Date Key\] → Date\[Date Key\] (inactive by
  default --- same conformed Date table)

- Foster Animal Month\[Month Date Key\] → Date\[Date Key\]

*No relationship declared to Fact_AnimalIntake in this version ---
Animal ID is a shared natural key across all three fact tables, but a
formal cross-fact relationship is deferred.*

# **8. Known Gaps & Deferred Items**

## **8a. Resolved since v1.2**

- Is_Foster flag logic --- RESOLVED. Fix confirmed: pattern match
  (contains \'to adopt\'), not exact match. Needs applying at the source
  notebook.

- Dim_Date extension --- RESOLVED. Confirmed: extends the existing
  global/shared Dim_Date, not a separate table.

- AnimalId as Int64 --- RESOLVED. Confirmed directly against the real
  gold_foster.fact_foster_placement schema in MS Fabric --- Int64 is
  correct.

- Column nullability --- CONFIRMED against the real gold table: all 7
  columns (PlacementId, AnimalId, ShelterLocationName, StartDateKey,
  EndDateKey, DurationDays, IsCurrent) are nullable in the live backend.
  Noted for awareness --- PlacementId being nullable is unusual for a
  grain key, but this document defines schema/data definitions only; the
  Phase 1 build runs against mock CSV data, not the live backend
  directly, so this is not a blocker for this version.

## **8b. Still Open**

- Average Foster Duration and Current Foster Animals --- columns exist
  (Duration (Days), Is Current) but are not confirmed KPIs in this
  version. Promote once formally agreed with the Data Owner.

- No relationship to Fact_AnimalIntake yet --- needed before cross-fact
  analysis (e.g., "which intake type drives the most foster placements")
  is possible.

- Foster Home / volunteer identity is not tracked anywhere in
  ShelterBuddy --- cannot answer "how many foster families were used."

- This Category 2 work has not gone through a formal BRD/Discovery
  Framework update --- the existing BRD_Foster_Analysis_v1.0 and
  Foster_Phase1_Discovery_Framework explicitly exclude Foster Duration
  Analysis and Concurrent Animals in Foster (i.e., exactly this subject
  area). This was a deliberate, direct-Data-Owner-feedback path, not an
  oversight --- but it should be recorded as a known process deviation,
  or reconciled with a formal addendum, before this is treated as fully
  governed.

# **9. Revision History**

  --------------------------------------------------------------------------
  **Version**   **Date**     **Change**
  ------------- ------------ -----------------------------------------------
  1.1           (prior)      Author\'s initial draft ---
                             PlacementId/AnimalId as String; no IsCurrent
                             column.

  1.2           2026-08-24   PlacementId and AnimalId changed to Int64
                             (AnimalId pending verification against real SB
                             data). Added IsCurrent to Foster Placement.
                             Added Current Foster Animals KPI (not yet
                             confirmed in scope). Start/End Date Keys use
                             Vancouver local date. Reformatted to house
                             style (Technical/Business Name split, Build
                             Notes). Corrected Is_Foster exclusion logic gap
                             (misses "Foster - To Adopt"). Confirmed scope:
                             only Foster Placement Count and Fostered
                             Animals are active KPIs this version; Duration
                             and Current Foster Animals are present as
                             columns, not yet promoted to measures.

  1.3           2026-08-24   All 3 v1.2 CONFIRM items resolved: Is_Foster
                             fix confirmed (pattern match), Dim_Date
                             confirmed as global/shared extension, AnimalId
                             confirmed as Int64 against the real MS Fabric
                             gold table (gold_foster.fact_foster_placement).
                             Column nullability updated to match the real
                             gold table exactly (all 7 columns nullable).
                             Noted this document defines schema/data
                             definitions only --- Phase 1 build runs on mock
                             CSV data, not the live backend. Flagged as a
                             still-open item: this Category 2 work has not
                             yet gone through formal BRD/Discovery Framework
                             reconciliation.
  --------------------------------------------------------------------------
