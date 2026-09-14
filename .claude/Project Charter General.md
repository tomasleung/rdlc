# Project Charter

## 1. Project Identity

**Project:** [Project Name]

**Purpose:** 
[One sentence describing why this project exists.]

**Problem:**
[The specific problem we are solving.]

**Primary User:**
[Who will use this?]

---

## 2. Desired Outcome

We are successful when:

- [Outcome 1]
- [Outcome 2]
- [Outcome 3]

### North-Star Outcome

[The single most important result this project must achieve.]

---

## 3. Scope

### In Scope

- [Capability]
- [Capability]
- [Capability]

### Out of Scope

- [Capability]
- [Capability]
- [Capability]

Do not implement out-of-scope functionality unless explicitly approved.

---

## 4. Product Principles

The project must follow these principles:

1. Keep the solution simple.
2. Prefer working functionality over unnecessary abstraction.
3. Do not build functionality without a clear user/business benefit.
4. Reuse existing components before creating new ones.
5. Make changes incrementally.
6. Preserve existing working functionality.
7. Ask before making major architectural changes.

---

## 5. Technical Context

**Repository:** [GitHub repository]

**Primary Stack:**
- [Language]
- [Framework]
- [Database]
- [Services]

**Existing Architecture:**
[Short description.]

**Important Constraints:**
- [Constraint]
- [Constraint]
- [Constraint]

---

## 6. AI Coding Rules

The AI agent must:

- Read the existing code before modifying it.
- Understand the current architecture before introducing new patterns.
- Make the smallest reasonable change.
- Explain significant architectural decisions.
- Never silently change requirements.
- Never remove existing functionality without approval.
- Validate changes with appropriate tests/checks.
- Keep documentation synchronized with meaningful changes.

When requirements are ambiguous:
1. Identify the ambiguity.
2. State the assumption.
3. Prefer the simplest reversible solution.

---

## 7. Definition of Done

A feature is complete when:

- [ ] Required functionality works.
- [ ] Existing functionality still works.
- [ ] Appropriate tests/checks pass.
- [ ] No unnecessary complexity was introduced.
- [ ] Documentation is updated where necessary.
- [ ] The change satisfies the original user/business outcome.

---

## 8. Change Control

Major changes require explicit approval:

- Architecture
- Database model
- Authentication/security
- External integrations
- Technology stack
- Core business rules
- Project scope

---

## 9. Current Priority

**Current milestone:**

[What we are trying to accomplish now.]

**Next milestone:**

[What comes after that.]

**Current WIP limit:** [e.g. 1–2 features]