**TABLE DEFINITIONS --- FOSTER EPISODE ANALYTICS v1.2**

Animal Flow --- Foster Capacity Analysis (Category 2)

*Semantic Model --- Reformatted to House Style, Content Reviewed and
Corrected*

  -------------------------------------------------------------------------
  **Field**               **Value**
  ----------------------- -------------------------------------------------
  Document Type           Semantic Model / Table Definitions

  RDLC Phase              Category 2 --- Capacity & Support (new subject
                          area, separate from Phase 1 Fact_AnimalIntake)

  Capability              Animal Flow --- Foster Analysis

  Backend Platform        Microsoft Fabric

  Version                 1.2

  Status                  Draft --- reformatted to house style; 3 items
                          pending confirmation (see Known Gaps)

  Supersedes              TABLE_DEFINITIONS_FOSTER_EPISODE_ANALYTICS_v1.1
                          (author\'s own draft lineage --- separate from
                          the main Foster Analysis Table Definitions,
                          currently at v1.4)

  Related Documents       Foster_KPI_Catalog_Category2_Scoped_v0.1.xlsx
                          (KPI definitions this table supports)

  Author                  Tomas Leung
  -------------------------------------------------------------------------

> *Reformatted from the author\'s own v1.2 draft into house style
> (Technical/Business Name split, Build Notes column, note-box
> convention). Structure and column design are the author\'s own --- a
> genuinely good pattern (two purpose-built fact tables rather than one
> table serving two grains). Content corrections applied during this
> pass are called out explicitly below, not silently merged in.*

**1. Purpose**

Define the semantic model required to support the two confirmed Category
2 (Capacity & Support) KPIs for this scope:

- Foster Placement Count --- how many separate foster stays occurred

- Fostered Animals --- how many distinct animals were in foster, by
  month/year

**Two additional columns (Duration, Current status) are included in the
physical design because they fall out naturally from the source data ---
but they are NOT presented as confirmed KPIs in this version. See
Section 5 and Known Gaps.**

**2. Source & Business Logic**

**Source: slv.kennel_history_scd2 WHERE Is_Foster = 1 --- the
bronze-to-silver flag already built in the Fabric notebook pipeline (see
Fabric_Notebook_Function_Sharing_SOP_v0.1).**

> *CORRECTION to the existing Is_Foster flag logic: the current notebook
> implementation uses .isin(\'foster to adopt\') --- an exact-match
> check after lowercasing. Verified against real ShelterBuddy Shelter
> Location data (SB_Incoming_Mapping.xlsx review), this MISSES the real
> variant \"Foster - To Adopt\" (the extra \" - \" means it never equals
> the exact string \"foster to adopt\"). It correctly excludes \"Foster
> to Adopt\" and \"Foster To Adopt\" by coincidence, but not the third
> variant. Recommended fix: replace the exact-match check with a pattern
> match --- e.g., NOT ShelterLocation_Name.lower().contains(\'to
> adopt\') --- so all three known real spelling variants are excluded
> consistently. This should be fixed at the source (the shared notebook
> function), not worked around downstream.*

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

**3. Dimensional Model**

**3.1 Date (technical: Dim_Date --- EXTENDS the existing conformed
table)**

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

> *Open item: confirm this is meant to extend the shared Dim_Date rather
> than create a separate table for this subject area. Applied as the
> Kimball-recommended default (conformed dimension reuse) pending
> explicit confirmation --- see Known Gaps.*

**4. Fact Table 1: Foster Placement**

**Foster Placement (technical: Fact_FosterPlacement)**

**Grain: One row per foster placement (a stay --- one continuous trip to
foster, start to end).**

Description: The primary placement-level fact table. Supports Foster
Placement Count directly, and provides the source rows that
Fact_FosterAnimalMonth (Section 5) is derived from.

**Foreign Keys**

- Start Date Key → Date

- End Date Key → Date

*Two relationships to the same conformed Date table --- one active, one
inactive by default (Power BI requires marking a second relationship to
the same dimension as inactive; activate the relevant one per measure
via USERELATIONSHIP as needed).*

**Columns**

  ----------------------------------------------------------------------------------------
  **Technical Name      **Business   **Type**    **Description**          **Build Notes**
  (Fabric)**            Name (Power                                       
                        BI)**                                             
  --------------------- ------------ ----------- ------------------------ ----------------
  PlacementId           Placement ID Int64       Unique foster placement  Hidden --- key
                                                 identifier. Generated    column
                                                 --- not a native         
                                                 ShelterBuddy field.      

  AnimalId              Animal ID    Int64       Animal identifier.       Hidden --- key
                                                                          column. VERIFY
                                                                          against real SB
                                                                          data before
                                                                          final build ---
                                                                          see Known Gaps.

  ShelterLocationName   Shelter      String      Raw Shelter Location     
                        Location                 value that triggered     
                                                 this placement ---       
                                                 retained for             
                                                 QA/traceability back to  
                                                 source.                  

  StartDateKey          Start Date   Int64 (FK)  → Date. Vancouver local  Hidden --- FK
                        Key                      date.                    column

  EndDateKey            End Date Key Int64 (FK,  → Date. Vancouver local  Hidden --- FK
                                     nullable)   date. Null if placement  column
                                                 is still ongoing.        

  DurationDays          Duration     Int64       Placement length in      Hidden --- not
                        (Days)                   days. Column present;    yet promoted to
                                                 NOT a confirmed KPI in   a measure
                                                 this version --- see     
                                                 Known Gaps.              

  IsCurrent             Is Current   Int64 (0/1) 1 if this placement is   Hidden --- not
                                                 still active (no End     yet promoted to
                                                 Date). Column present;   a measure
                                                 NOT a confirmed KPI in   
                                                 this version --- see     
                                                 Known Gaps.              
  ----------------------------------------------------------------------------------------

**5. Fact Table 2: Foster Animal Month**

**Foster Animal Month (technical: Fact_FosterAnimalMonth)**

**Grain: One row per animal, per month touched by a foster placement.**

**Description: An internal, supporting table only --- not intended for
direct use by report authors. Its sole purpose is to correctly compute
Fostered Animals by month/year, since the same animal can have multiple,
non-overlapping placements that must still count as one animal per
month. Derived from Foster Placement (Section 4), not an independent
source.**

**Foreign Keys**

- Month Date Key → Date

**Columns**

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

**6. KPI Definitions**

**KPI: Foster Placement Count**

- Definition: Number of separate foster stays.

- Grain: One count per placement

- Formula: DISTINCTCOUNT(\'Foster Placement\'\[Placement ID\])

**KPI: Fostered Animals**

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

**7. Relationship Summary**

- Foster Placement\[Start Date Key\] → Date\[Date Key\] (active by
  default)

- Foster Placement\[End Date Key\] → Date\[Date Key\] (inactive by
  default --- same conformed Date table)

- Foster Animal Month\[Month Date Key\] → Date\[Date Key\]

*No relationship declared to Fact_AnimalIntake in this version ---
Animal ID is a shared natural key across all three fact tables, but a
formal cross-fact relationship is deferred.*

**8. Known Gaps & Deferred Items**

- CONFIRM: Is_Foster flag logic in the shared notebook needs the
  exact-match fix described in Section 2 --- currently misses the
  "Foster - To Adopt" spelling variant.

- CONFIRM: Dim_Date extension (MonthShortName, YearMonth) --- applied as
  an extension of the existing shared table by default; confirm this is
  correct rather than a separate table.

- CONFIRM: AnimalId as Int64 --- verify against real ShelterBuddy data
  that Animal ID is a pure integer with no leading zeros or alphanumeric
  values before final build. If not purely numeric, this must revert to
  String.

- Average Foster Duration and Current Foster Animals --- columns exist
  (Duration (Days), Is Current) but are not confirmed KPIs in this
  version. Promote once formally agreed with the Data Owner.

- No relationship to Fact_AnimalIntake yet --- needed before cross-fact
  analysis (e.g., "which intake type drives the most foster placements")
  is possible.

- Foster Home / volunteer identity is not tracked anywhere in
  ShelterBuddy --- cannot answer "how many foster families were used."

**9. Revision History**

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
  --------------------------------------------------------------------------
