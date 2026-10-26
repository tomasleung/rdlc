# BRD Content Standard v1.0

## 1. Purpose

This standard defines the minimum governed content for a Business Requirements Document produced through the RDLC design-intake process. It is artifact-neutral: project-specific decisions must come from the confirmed Shared Understanding Summary, approved organizational evidence, and authorized source artifacts.

A BRD must explain:

1. the business problem;
2. the decision or action the capability supports;
3. the governed scope and exclusions;
4. the required information, measures, rules, and source authority;
5. how quality and acceptance will be demonstrated;
6. who owns definitions, implementation, approval, and change control; and
7. what the initiative explicitly does not decide.

## 2. Authority and precedence

Apply inputs in this order:

1. Confirmed Shared Understanding Summary
2. Approved organization policy or governance standard
3. Applicable approved artifact template
4. Approved source definitions, KPI contracts, and data contracts
5. Approved prior artifacts used as factual references
6. Prior artifacts used only as structural or visual examples
7. Explicitly accepted assumptions

A prior BRD must never override a confirmed decision for the current artifact.

## 3. Artifact states

Use one of these statuses:

- **Draft for business review**: content is generated from confirmed intake but approvals remain open.
- **Approved**: required approvers have explicitly approved the artifact.
- **As Delivered**: the document records what was actually implemented and identifies deviations from the approved request.
- **Superseded**: a later artifact has replaced this version.

Never label an artifact Approved without explicit evidence.

## 4. Required sections

### 4.1 Document metadata

Include:

- RDLC document type
- RDLC phase
- RDLC stage
- capability name
- capability domain
- author
- version
- status
- created date
- last-updated date
- decision owner
- definition owner
- technical owner
- source-system authority
- historical or operational time window, when applicable
- refresh frequency, when applicable
- primary delivery format
- information classification

Unknown ownership or approval values must be marked **Pending** or **To be assigned**, not invented.

### 4.2 Executive summary

State in plain business language:

- the current problem;
- the proposed capability;
- the principal measures or outputs;
- the decision supported;
- the main interpretation boundary; and
- the intended delivery.

Target length: two to four short paragraphs plus one interpretation or scope callout.

### 4.3 Business problem

Describe:

- the current condition;
- the information or decision gap;
- the people or process affected;
- the operational or governance consequence; and
- why existing reports or processes are insufficient.

Avoid solution-first wording. Do not present the proposed technical design as the business problem.

### 4.4 Decision supported

Include:

- decision to be made;
- intended business action;
- decision owner;
- decision frequency or timing;
- why timing matters, if material; and
- what the artifact does not decide.

A descriptive report must not imply performance, causation, targets, or recommendations unless those uses were explicitly confirmed.

### 4.5 Current state

Document:

- existing process or artifact;
- existing trusted definitions;
- current sources;
- known limitations;
- manual effort or fragmentation; and
- previous phases or delivered capability that must remain authoritative.

### 4.6 Desired capability

Describe the target capability as observable business outcomes and governed behaviours. Include only confirmed outputs, dimensions, time treatment, delivery, and controls.

### 4.7 Business questions

List questions the artifact must answer. For each question, identify the intended measure, view, or evidence. Do not include questions that are outside scope.

### 4.8 Analytical approach

When applicable, define:

- reporting grain;
- population;
- entity and event identifiers;
- time zone;
- interval boundaries;
- effective-dated treatment;
- distinct-count keys;
- additive and non-additive behaviour;
- dimensional attribution;
- cohort or period logic;
- historical restatement; and
- current/open record treatment.

Use worked examples when the rules are easy to misinterpret.

### 4.9 Business definitions and KPI contracts

For every governed measure include:

- business name;
- business definition;
- formula;
- numerator and denominator, where applicable;
- population;
- grain;
- dimensions;
- event or time rule;
- inclusions;
- exclusions;
- unknown and exception handling;
- interpretation guidance;
- owner;
- refresh frequency;
- validation requirement; and
- approval status.

Measures must be internally consistent. Numerator and denominator populations, dimensions, and reporting periods must be compatible.

### 4.10 Data sources

For each source identify:

- source system or artifact;
- business authority;
- governed purpose;
- required keys and timestamps;
- required fields or evidence;
- expected history;
- known quality concerns; and
- dependency or access status.

Do not invent physical table or column names. When physical profiling is incomplete, state the logical requirement and label the physical detail as a dependency.

### 4.11 Data-design requirements

Include this section when effective dating, transformations, SCDs, lineage, reconciliation, or entity resolution are material.

Define:

- required output grain;
- effective start and end semantics;
- event precedence and tie-breaking;
- source identifiers retained for audit;
- change detection;
- current-row handling;
- conflict handling;
- unknown and unmapped treatment;
- parent/child or segment behaviour;
- lineage and traceability; and
- reconciliation controls.

An SCD row change must not automatically imply a new business event or metric count unless explicitly defined.

### 4.12 Scope

Separate:

- In scope
- Deferred to a named future phase
- Out of scope

Avoid overlapping items. State boundaries that prevent the current artifact from being interpreted as a capacity, performance, forecasting, outcome, or operational-control solution when those capabilities are not included.

### 4.13 Assumptions, constraints, dependencies, and accepted limitations

Distinguish each category:

- **Assumption**: proposed condition accepted for the artifact.
- **Constraint**: fixed boundary imposed by system, policy, time, or design.
- **Dependency**: required external evidence, access, mapping, approval, or implementation input.
- **Accepted limitation**: unresolved issue explicitly permitted to carry forward.

Do not relabel missing facts as assumptions.

### 4.14 Report and consumption requirements

Define:

- required report views;
- filters and comparisons;
- narrative and interpretation rules;
- zero, null, unknown, and unavailable presentation;
- low-volume or suppression controls;
- drill-through or export permissions;
- aggregate versus record-level access;
- delivery format;
- publication frequency; and
- accessibility or print requirements.

### 4.15 Data quality and validation

Include:

- completeness thresholds;
- validity and conformance tests;
- referential integrity;
- interval and overlap checks;
- duplication and distinct-count tests;
- source-to-output reconciliation;
- worked-example validation;
- refresh repeatability;
- restatement controls;
- exception handling; and
- publication blocking criteria.

Every threshold must have an owner and a stated action when breached.

### 4.16 Success criteria

Success criteria describe business or capability outcomes. Each criterion must include how it will be checked and the target state.

Examples:

- users can answer the confirmed business question;
- historical reporting is available for the approved period;
- results are reproducible;
- quality limitations are visible; and
- the approved delivery can be refreshed without manual record correction.

### 4.17 Acceptance criteria

Acceptance criteria describe the evidence required to approve the artifact or solution. Cover:

- governance approval;
- definition approval;
- source interpretation;
- data integrity;
- reconciliation;
- security and access;
- operational readiness;
- refresh readiness; and
- known limitation acceptance.

Do not confuse success criteria with acceptance evidence.

### 4.18 Deliverables

For each deliverable state:

- name;
- purpose;
- format;
- owner;
- publication or storage location, when known; and
- approval or readiness state.

### 4.19 Governance and change control

Identify:

- business decision owner;
- business definition owner;
- technical owner;
- source-system authority;
- publication approver;
- change-approval requirements;
- regression-test requirements;
- historical-restatement policy; and
- disclosure requirements.

### 4.20 Approvals

Provide role, name, decision/status, and date. Leave unresolved names or dates blank or Pending. Do not infer approval from participation in the intake.

### 4.21 Appendices

Use appendices for:

- worked examples;
- mapping principles;
- validation scenarios;
- source-to-output traceability;
- glossary;
- decision register; and
- reference artifacts.

## 5. Decision-to-artifact mapping

Before generation, map each material confirmed node to a BRD section. The mapping must cover:

- purpose and intended action;
- consumers and owners;
- scope and exclusions;
- grain, population, and time;
- definitions and formulas;
- dimensions and business rules;
- source authority;
- quality tolerances;
- privacy and access;
- refresh and history;
- validation and acceptance;
- delivery;
- risks and limitations;
- approvals; and
- change control.

If a material confirmed node has no destination, revise the document structure or use an appendix. If a BRD section requires a material decision that was not settled, reopen only that dependency branch.

## 6. Reference-artifact rules

Reference artifacts may support structure, terminology, existing facts, established definitions, ownership, and visual standards. They must not silently supply current-project scope, measures, thresholds, sources, approval state, or business rules.

When a prior artifact conflicts with the confirmed summary, the confirmed summary governs unless an approved policy or definition has higher authority.

## 7. Drafting rules

- Use direct, auditable language.
- Preserve approved business terminology.
- Define ambiguous terms at first use.
- Prefer active voice.
- Keep one governed concept per definition.
- Use requirement language consistently: **must** for mandatory, **should** for recommended, **may** for permitted.
- Do not use **will** to disguise an unsettled commitment.
- Do not claim causation from descriptive analysis.
- Do not create targets from historical averages without approval.
- Do not label a high or low value good or bad without a governed interpretation rule.
- Use concrete dates rather than relative dates in the artifact.

## 8. Pre-delivery quality check

Confirm that:

- explicit shared-understanding confirmation was received;
- artifact type and scope are authorized;
- every material decision appears in the BRD;
- no stakeholder decision is invented;
- facts match their sources;
- assumptions and accepted limitations are labelled;
- open approvals say Pending;
- scope and exclusions do not conflict;
- KPI name, definition, formula, grain, population, dimensions, and time rules agree throughout;
- distinct-count and non-additivity warnings are present when material;
- source authority and physical-profile dependencies are clear;
- validation tests the governed definitions;
- success criteria and acceptance criteria remain distinct;
- refresh, restatement, and change control are included when material;
- no project-specific content was copied from a reference merely because it existed there;
- title, file name, version, status, and dates agree; and
- the output opens and contains every required section.
