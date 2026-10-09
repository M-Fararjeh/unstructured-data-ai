<!--
Sync Impact Report
- Version change: 1.0.0 → 1.1.0
- Modified principles:
  - VII. Definition of Done → expanded with additive production-readiness
    completion criteria (items 8–9); items 1–7 preserved unchanged in intent
- Added sections:
  - Production Readiness
    - Environment Management
    - Database Migrations, Backups, Restore & Rollback
    - Observability & Operations
    - Release Management
    - Non-Functional Requirements
    - Mandatory Technical Documentation
- Removed sections: none
- Existing principles I–VI and sections Quality Gates & Independent Review,
  Multi-Agent Development, Governance: preserved; Governance compliance
  bullets expanded to reference production readiness
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
8. Production Readiness obligations applicable to the change (environments,
   data/migration safety, observability, release controls, NFRs, and mandatory
   documentation) are satisfied
9. Deployment or release of the change follows Release Management rules,
   including approvals and a verified rollback path when the change is
   releasable

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

## Production Readiness

Production readiness governs how software is operated, released, and sustained.
These rules ADD to—and MUST NOT weaken—Core Principles, quality gates, or
security requirements. Where a change touches runtime behavior, data, APIs,
or deployment, the applicable subsections below are mandatory.

### Environment Management
The project MUST maintain distinct Development, Staging, and Production
environments with isolated configuration, credentials, and data. Environment
promotion MUST be unidirectional for releases (Development → Staging →
Production) unless an explicitly approved emergency procedure documents
otherwise. Production data MUST NOT be used in Development; if masked or
anonymized subsets are required for Staging, the sanitization method MUST be
documented and approved. Environment-specific settings MUST be externalized
(not hard-coded); secrets remain subject to Secure Development. Changes MUST
be validated in Staging before Production deployment.

### Database Migrations, Backups, Restore & Rollback
Schema and data migrations MUST be versioned, reviewable, and applicable in
order through automated, repeatable procedures. Migrations MUST be tested in
non-production environments before Production. Every migration MUST have a
documented rollback or forward-fix strategy that has been validated where
technically feasible. Automated backups MUST exist for Production (and for
Staging when it holds non-ephemeral data), with documented retention.
Restore procedures MUST be documented and periodically verified; an untested
backup does not satisfy this requirement. Destructive data operations require
explicit approval and a restore point.

### Observability & Operations
Applications MUST emit structured logs suitable for search and correlation,
without logging secrets or sensitive payloads beyond approved policy.
Monitoring MUST cover service health and critical dependency signals.
Health-check endpoints (or equivalent probes) MUST distinguish liveness and
readiness where the runtime model supports it. Error tracking MUST capture
actionable failure telemetry with enough context to diagnose without
weakening security or privacy controls. Alerts for Production-impacting
failures MUST be defined for critical paths; silent Production failure is
unacceptable.

### Release Management
Releases MUST use explicit semantic versioning for shippable artifacts and
record what changed (version, commit/tag, environment, approver). Deployment
to Staging and Production MUST require documented approval from a role other
than the sole implementer. Production deployment is forbidden when required
CI quality gates are red or when Staging validation for the release candidate
has not succeeded. Every releasable change MUST have a documented, tested
rollback procedure (previous artifact/version and/or data rollback/forward-fix
as applicable). Hotfixes remain subject to security, verification, and
traceability obligations; process compression MUST NOT omit rollback planning
or independent approval.

### Non-Functional Requirements
Performance, scalability, reliability, and availability requirements that
apply to a feature or service MUST be documented as measurable criteria
(targets, thresholds, or SLOs) before the related work is accepted as done.
Changes that can affect those properties MUST include verification evidence
appropriate to risk (benchmarks, load tests, chaos/failover drills, or
equivalent). Capacity and scaling assumptions MUST be explicit. Reliability
mechanisms (timeouts, retries with backoff, idempotency where required,
degradation behavior) MUST be designed deliberately—not left as accidental
defaults. Availability expectations for Production MUST be stated; work that
reduces availability below documented targets MUST NOT ship without an
approved exception.

### Mandatory Technical Documentation
The following documentation MUST be kept current for in-scope deliverables:
- Technical documentation describing architecture, modules, key flows, and
  operational constraints
- API documentation for every externally or cross-module consumable interface
  (contracts, errors, auth, versioning/compatibility notes)
- Deployment documentation covering environments, build/release steps,
  configuration, migrations, health checks, monitoring/alerts, and rollback

Documentation updates are part of Definition of Done for changes that alter
behavior, interfaces, or operations. Undocumented Production behavior is a
defect. Agents MUST NOT invent business requirements in documentation; they
MUST reflect approved specs and implemented reality.

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
- Production readiness (environments, data safety, observability, release
  controls, NFRs, and mandatory documentation) MUST be checked for releasable
  and operationally impactful changes
- Exceptions require written justification, time-boxed duration, and explicit
  approver identity; undocumented exceptions are invalid

**Version**: 1.1.0 | **Ratified**: 2026-10-09 | **Last Amended**: 2026-10-09
