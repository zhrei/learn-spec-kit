<!--
Sync Impact Report
- Version change: N/A → 1.0.0
- Modified principles: N/A (new document)
- Added sections: Core Principles (4), Quality Gates & Definition of Done, Development Workflow & Review Process, Governance
- Removed sections: None
- Templates requiring updates:
  ✅ .specify/templates/plan-template.md (footer version/path)
  ✅ .specify/templates/spec-template.md (no changes required)
  ✅ .specify/templates/tasks-template.md (no changes required)
  ⚠ Pending: None
- Follow-up TODOs: None
-->

# Learn Spec Kit Constitution

## Core Principles

### Code Quality (Non-Negotiable)
The codebase MUST remain simple, readable, and maintainable.

- Functions MUST strive to be ≤5 lines; any function >10 lines REQUIRES a refactor or explicit reviewer-approved justification in the PR description.
- Names MUST be descriptive (no unclear abbreviations). Types MUST be explicit where supported; avoid unsafe casts and `any`-like escape hatches.
- Complexity MUST be controlled: cyclomatic complexity ≤5 per function unless justified; deep nesting (>2 levels) MUST be refactored.
- Duplication MUST be eliminated (DRY). Shared logic lives in well-named modules.
- Formatting and linting MUST pass in CI. No warnings ignored without justification.
- Comments explain “why,” not “what.” TODO comments are not allowed; create an issue and link.
- Errors MUST be handled meaningfully; no silent catches.

Rationale: High-quality code reduces defects, accelerates onboarding, and lowers long-term cost.

### Testing Standards (TDD Discipline)
Tests drive implementation and guard behavior.

- TDD is REQUIRED: write failing tests before implementation (unit → integration → contract where applicable).
- Coverage MUST be ≥90% statements/branches overall, and 100% for critical paths and bug fixes.
- Tests MUST be deterministic, isolated, and fast. External effects are mocked/faked unless exercising contracts.
- Contract tests MUST exist for public interfaces/APIs and run in CI.
- Performance tests MUST exist where performance budgets apply (see Performance Requirements) and gate merges.
- A PR MUST NOT merge unless tests fail first (new work) then pass on CI with coverage gates met.

Rationale: Tests specify intent, prevent regressions, and enable safe refactoring.

### User Experience Consistency (Accessibility Included)
User-facing experiences MUST be consistent, accessible, and predictable.

- A shared design language (tokens, components, interaction patterns) MUST be used consistently across screens and features.
- Accessibility MUST meet WCAG 2.1 AA: keyboard navigation, contrast, semantics/ARIA where applicable.
- Copy and error messages MUST be clear, actionable, and consistent in tone and structure.
- States (loading, empty, error, success) MUST be explicitly handled with visible feedback.
- UX review is REQUIRED for user-facing changes; deviations from the design language REQUIRE justification.

Rationale: Consistency and accessibility build user trust and reduce cognitive load.

### Performance Requirements (Budgets and Guardrails)
Performance is a feature and MUST be budgeted, measured, and enforced.

- Backend/API: p95 latency <200ms and p99 <500ms under expected load; error rate <0.1%. Budgets MUST be validated in CI or staging perf checks.
- Frontend (if applicable): interactive within 2s on a mid-tier device/network; interactions <100ms target, <200ms max for p95.
- No N+1 queries; indexes MUST exist for frequent predicates; memory use bounded and monitored.
- Proven regressions MUST block merges until resolved or justified with a temporary waiver and issue link.
- Optimizations MUST follow measurement and profiling; premature optimization is discouraged.

Rationale: Predictable performance improves usability and operational reliability.

## Quality Gates & Definition of Done

- All linters, formatters, type checks, and tests MUST pass in CI; coverage gates are enforced.
- New/changed behavior MUST have automated tests (unit/integration/contract as appropriate).
- Public interfaces MUST include updated contracts/schemas and documentation.
- Performance and accessibility checks MUST pass where applicable.
- Security basics: input validation, least privilege, safe dependencies (scanner clean) MUST be verified.
- PR description MUST include rationale, risks, and links to related issues/specs.

## Development Workflow & Review Process

- Branching and commits: use conventional commit style where possible; small, focused PRs are preferred.
- Reviews: at least one qualified reviewer REQUIRED. Reviewers enforce the Constitution and request refactors over accepting complexity.
- Changes that increase complexity require explicit justification and a follow-up simplification plan where feasible.
- Versioning: use Semantic Versioning for releases; document breaking changes, migration steps, and deprecations.
- Runtime guidance: keep quickstarts and contracts in sync with changes; update user-visible docs together with code.

## Governance

- Supremacy: This Constitution supersedes other practices. Conflicts MUST be resolved in favor of this document.
- Amendments: Propose via PR including change log, rationale, impact analysis, and migration plan if needed. Maintainers approve.
- Versioning Policy: Semantic Versioning for this Constitution
  - MAJOR: Backward-incompatible removals/redefinitions of principles or governance
  - MINOR: New principle/section added or materially expanded guidance
  - PATCH: Clarifications and non-semantic refinements
- Compliance: All PRs MUST include a Constitution Check. Periodic audits MAY be run to ensure ongoing compliance.

**Version**: 1.0.0 | **Ratified**: 2025-09-24 | **Last Amended**: 2025-09-24