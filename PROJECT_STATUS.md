# Project Status — Initiative Index

**Purpose:** any new chat session in this repo should read this file first — it's the short index. Each row below has objective + current step + a link to the full detail file in `initiatives/`. Read the detail file before resuming that specific initiative's work; don't rely on this index alone.

**Workspace objective:** build a portfolio of RDLC OS-governed, decision-first data products for BC SPCA. Each initiative below goes through the same pipeline (`rdlc-grill-coach` → confirmed BRD → `rdlc-data-analysis` → `rdlc-mock-report-design` → semantic model → build) independently — they are separate artifacts, not one monolithic project, even though they share this repo and its RDLC skills.

**Naming note:** "Phase 1," "Phase 2," etc. are already claimed by specific initiatives below — check this table before reusing a phase number or KPI name for a new initiative. This has already collided twice: "Phase 2" claimed by both CATU and (briefly) Foster Volunteer Engagement; "% Animals in Foster" used for two different denominators before being disambiguated.

**Before trusting any "confirmed"/"current state" claim anywhere in this project** (including in this file), verify it against the real files on disk — see `SOP_New_Chat_Session.md`. This project has real, repeated drift between documented state and what's actually built.

---

| # | Initiative | Data source | Objective (current, may still be under discovery) | Stage | Status | Detail file |
|---|---|---|---|---|---|---|
| 01 | Foster Pathway Analytics, Phase 1 | ShelterBuddy | Two counters (Fostered Animals, Foster Placement Count); rate measure disputed | BRD v1.1, all approvals Pending | **CONTESTED** | `initiatives/01-phase1-shelterbuddy-counters.md` |
| 02 | Centre & Animal Type Utilization (CATU) — this repo's real "Phase 2" | ShelterBuddy | % Animals in Care Fostered, by effective Centre/Region/Animal Type Family | BRD v0.3, reviewed, fixes needed | Draft for business review | `initiatives/02-catu-centre-animal-type.md` |
| 03 | SB Status & Source Operational Pathway | ShelterBuddy | Diagnostic evidence for a Status/Source taxonomy consolidation decision (not ours to decide) | BRD v0.1, grill session not formally closed | Draft | `initiatives/03-sb-status-source-pathway.md` |
| 04 | Self-Service Data Platform | Internal (IT/Data Team) | Define the repeatable Self-Service product contract (scope: Bronze/Lakehouse access only) | Epic v1.0; grill session mid-flight at Q3/Q7 | In progress | `initiatives/04-self-service-data-platform.md` |
| 05 | Triage Dashboard | Excel trackers (pre-intake) | Rank the most vulnerable pre-intake service requests across sources | Minimal grill, Q1/Q2 awaiting confirmation | Early | `initiatives/05-triage-dashboard.md` |
| 06 | Foster Volunteer Engagement | Better Impact + ShelterBuddy | Objective still under discovery — which KPI(s) become the confirmed core measure | Source docs saved, pre-grill-coach | Early | `initiatives/06-foster-volunteer-engagement/README.md` |
| 07 | Reference documents | — | Informational only, not governed artifacts | N/A | N/A | `initiatives/07-reference-documents.md` |

---

## Standing process notes

- When something in private AI memory would help *the project* (not just working-style preferences with Tomas), promote it to a repo file and remove the private duplicate — don't let the same fact live in two places.
- Update this index and the relevant `initiatives/` file whenever an initiative materially advances. A session that advances work without updating these recreates the exact staleness problem that made the old `CLAUDE.md` wrong about Phase 1.
