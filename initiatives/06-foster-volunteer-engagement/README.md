# Initiative 06 — Foster Volunteer Engagement (Better Impact volunteer KPIs)

**Do not call this "Phase 2"** — that label already belongs to CATU (initiative 02). This naming collision was already caught and corrected once.

**Status:** Two source documents now saved to this repo (`source/`). No action taken on either yet. No BRD opened. Pre-grill-coach.

## Subject

Which KPI(s) become the confirmed "core measure" for foster volunteer engagement/retention — Lindsay's 2 draft membership-style metrics vs. Tomas's proposed 5-metric Layer 2 operational-signal set, all anchored on verified ShelterBuddy `AnimalID`+`PersonID` placement links rather than Better Impact's lagging `Active`/`Archived` status field.

## Source documents

1. **`source/Lindsay_KPI_Research_Brief_v0.1.md`** — the technical/analytical brief: ground-truth data constraints, Lindsay's 2 draft KPIs (Retention >1yr, Attrition = archived count) and why they're unreliable, Tomas's proposed 5-signal Layer 2 architecture (Active Foster Pool & Utilization %, 1st-to-2nd Placement Conversion %, 12-Month Repeat Retention %, Effective Behavioral Attrition, Shelter Days Saved).
2. **`source/Better_Impact_Access_Request_v0.1.md`** — the business-case/access-request email, naming Maggie Stewart as a collaborator and framing the objective as understanding the balance between **Need** (animals in care requiring foster) and **Capacity** (available/active foster volunteers).

## Important cross-initiative dependency

The access-request email explicitly uses "animals in care" and "percentage of animals placed in foster care" — the same population/measure language already in play (and already contested) in initiatives 01 and 02. **This is not an independent initiative** in practice; it explicitly wants to join foster-volunteer capacity data to the same Animals-in-Care population CATU defines. Do not confirm a KPI here without checking it against initiatives 01/02's measures first.

## Next

Run `rdlc-grill-coach` — root question is which KPI(s) are the confirmed core measure(s), now informed by both source documents above. High-level BRD scoping only; explicitly not schema/DAX yet.
