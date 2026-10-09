<!--
Sync Impact Report
- Version change: (unset/template) → 1.0.0
- Modified principles:
  - [PRINCIPLE_1_NAME] → I. Modular Architecture
  - [PRINCIPLE_2_NAME] → II. Clean Code
  - [PRINCIPLE_3_NAME] → III. Secure Development
  - [PRINCIPLE_4_NAME] → IV. Documented Requirements & Measurable Acceptance
  - [PRINCIPLE_5_NAME] → V. Mandatory Verification (Unit, Integration, E2E)
- Added sections:
  - VI. Traceability (as Core Principle)
  - VII. Definition of Done (as Core Principle)
  - Quality Gates & Independent Review
  - Multi-Agent Development
- Removed sections: none (placeholders replaced)
- Follow-up TODOs: none
-->

# Unstructured Data AI Constitution

## Core Principles

### I. Modular Architecture
All features MUST be delivered as well-bounded modules with explicit public
interfaces and minimal coupling. A module MUST own a single coherent
responsibility, expose only documented contracts, and remain independently
buildable and testable. Cross-module dependencies MUST flow through stable
interfaces—not shared internals, global mutable state, or ad-hoc coupling.
New work MUST justify module placement; organizational-only or catch-all
modules are forbidden.

Rationale: Modular boundaries enable parallel multi-agent work, safer change,
and isolated verification.

### II. Clean Code
Code MUST be readable, intentional, and maintainable. Names MUST reveal purpose;
functions and types MUST stay small and cohesive; duplication MUST be removed
when it obscures intent. Comments MUST explain non-obvious rationale, not
restate code. Dead code, speculative abstractions, and unexplained complexity
are forbidden. Refactors that improve clarity without changing behavior are
encouraged when they reduce risk or review cost.

Rationale: Clean code is the primary durable interface between humans and
agents across implementation, QA, and review.

### III. Secure Development
Security MUST be designed and reviewed as a first-class requirement. Secrets
MUST never be committed, logged, or embedded in client-visible artifacts.
Inputs MUST be validated at trust boundaries; authentication, authorization,
and least privilege MUST be enforced for every protected operation. Dependency
and configuration changes MUST be assessed for security impact. Known insecure
patterns (injection, unsafe deserialization, path traversal, SSRF, weak
crypto, overly broad CORS/permissions) MUST NOT be introduced. Security-relevant
defects MUST be fixed before the related work is considered done.

Rationale: Production-grade delivery requires preventing and detecting security
failures before merge.

### IV. Documented Requirements & Measurable Acceptance
No feature work MAY begin without documented business requirements and
measurable acceptance criteria in the Spec Kit artifacts (spec/plan/tasks as
applicable). Acceptance criteria MUST be testable, observable, and free of
vague language. Agents and contributors MUST NOT invent business requirements;
underspecified intent MUST be clarified with stakeholders and recorded in the
spec before implementation proceeds. Technical design choices may be proposed,
but product/business rules require explicit documented approval.

Rationale: Verification and traceability are impossible without shared,
written requirements.

### V. Mandatory Verification (Unit, Integration, E2E)
Every change that affects behavior MUST include automated verification at the
appropriate layers:
- Unit tests for module logic and edge cases
- Integration tests for contracts, shared schemas, persistence, and
  inter-component communication
- End-to-end (E2E) tests for user-visible or system-level workflows touched by
  the change

Tests MUST fail for the defect or missing capability before the fix/feature is
accepted as verified. Flaky, non-deterministic, or permanently skipped tests do
not satisfy this principle. Manual-only checks MAY supplement—not replace—
automated coverage for in-scope behavior.

Rationale: Automated multi-layer tests are the only scalable proof that
requirements remain satisfied.

### VI. Traceability
Every requirement and acceptance criterion MUST map to implementation artifacts
and to test evidence that demonstrates satisfaction. Trace identifiers (or
equivalent explicit links) MUST connect: requirement → design/task → code →
unit/integration/E2E results (and CI run evidence where applicable). Gaps in
the chain MUST block completion. Orphan implementation or unmapped tests MUST
be reconciled or removed.

Rationale: Full traceability makes compliance reviewable and prevents silent
scope drift.

### VII. Definition of Done
A task, story, or pull request is complete ONLY when all of the following hold:
1. Requirements and acceptance criteria are documented and unchanged without
   explicit amendment
2. Implementation matches those criteria within modular and clean-code bounds
3. Security obligations for the change are satisfied
4. Required unit, integration, and E2E tests pass
5. GitHub CI quality gates are green for the change
6. Independent review has approved the change
7. Traceability links from requirement to implementation and test evidence are
   present and reviewable

No task MAY be marked complete based on implementation alone.

Rationale: Successful verification—not coding activity—defines completion.

## Quality Gates & Independent Review

All merges to protected branches MUST pass automated GitHub CI quality gates
covering at minimum: lint/static analysis (as configured for the stack), unit
tests, integration tests, and E2E tests applicable to the change. CI failure
MUST block merge. Local verification MUST precede push when practical; CI
remains the authoritative gate.

Independent code review is mandatory. The implementer MUST NOT be the sole
approver of their own change. Review MUST verify constitution compliance,
requirement coverage, test adequacy, security posture, and traceability—not
only style. Review findings that violate Core Principles MUST be resolved
before approval.

## Multi-Agent Development

Multi-agent (or multi-role) development MUST separate responsibilities:
- Implementation agents/roles produce code and developer-facing docs against
  approved specs
- QA agents/roles own test design, execution evidence, and acceptance
  verification against criteria
- Review agents/roles perform independent compliance and quality review

A single agent MAY assist across phases only when outputs are explicitly
handed off and re-validated by the responsible role; self-certification of
one's own implementation as fully verified is forbidden. Agents MUST NOT
start coding outside an approved specification, MUST NOT invent business
requirements, and MUST escalate ambiguity rather than guess.

## Governance

This constitution supersedes conflicting informal practices. All specifications,
plans, tasks, pull requests, and reviews MUST comply with it.

Amendments MUST:
1. Update `.specify/memory/constitution.md` with clear principle/section diffs
2. Increment version per semantic versioning (MAJOR: incompatible removals or
   redefinitions; MINOR: new principles/sections or material expansions;
   PATCH: clarifications and non-semantic refinements)
3. Set **Last Amended** to the amendment date; preserve **Ratified** unless
   the document is formally re-adopted
4. Include a temporary Sync Impact Report comment for human review prior to
   commit (remove before or as part of finalizing the amendment commit)

Compliance review expectations:
- Spec Kit planning and implementation workflows MUST check constitution
  alignment
- CI and independent review are enforcement points for verification and
  security principles
- Exceptions require written justification, time-boxed duration, and explicit
  approver identity; undocumented exceptions are invalid

**Version**: 1.0.0 | **Ratified**: 2026-10-09 | **Last Amended**: 2026-10-09
