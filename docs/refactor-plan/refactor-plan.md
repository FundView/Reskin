A) FOLLOW-UP QUESTIONS (Non-Blocking)
- All current scope decisions are confirmed; no additional follow-up questions at this time.

B) COMPLETED REFACTOR PLAN (A-Z)

B1) Executive Summary (1-2 pages)
- Refactor thesis: 1:1 reskin with zero behavior change. Extract core logic from controllers and helper services into shared, framework-agnostic libraries that dual-target `$(LegacyTargetFrameworkVersionName)` and `$(DotNetTargetFrameworkVersionName)`. Keep controllers thin and delegate to use-case services so the same core logic runs in both legacy MVC and new services.
- Module scope: the entire repo is in scope except CodeSmith and other tooling folders. UI Razor files are translated to Angular in FundViewClient using its conventions and theming.
- Constraints and non-negotiables: no behavior, schema, API, or side-effect changes; all user journeys are no-regression; zero UI tolerance in legacy MVC; no new features during the reskin; dual-target core logic; EF6 stays in adapters until legacy retirement.
- Target outcomes and success criteria: near-100% test coverage for extracted logic (unit, integration, and E2E); zero regressions across all journeys; stable APIs and schemas; successful dual-run of legacy and new services for about 6 months, followed by legacy retirement after full reskin sign-off and beta.
- Proposed approach: pattern-by-pattern extraction with branch-by-abstraction, using GL/AP as canonical patterns; accept dependency web effects by mapping and batching cross-module changes; use NDepend to identify coupling, cycles, and architectural drift; release incrementally via rapid application development with heavy test coverage.
- Artifacts and storage: plan and TODO live at `docs/refactor-plan/refactor-plan.md` and `docs/refactor-plan/refactor-todo.md`.

B2) Scope Definition
- In-scope components: all modules under FundViewGit except CodeSmith, including controllers, services, repositories, mappers, view models, and entities. GL/AP serve as reference patterns and may receive cross-module fixes.
- UI translation scope: all Razor and MVC UI flows are translated to Angular in FundViewClient with 1:1 behavior parity; tracked and delivered in the FundViewClient repo.
- In-scope refactor targets: controller-to-service boundaries, helper services with business logic, repository query shaping, mapper-only transformations, report data preparation, import pipelines, and adapter boundaries for EF6 and EF Core.
- Out-of-scope items: new features, UI redesign beyond 1:1 parity, schema changes, API changes, and tooling like CodeSmith.
- Dependencies and external contracts: all existing API routes, DTOs, and DB schemas are frozen. Integrations (citation import, payment providers) must behave identically.

B3) Baseline Discovery (what to inventory)
- Repo map: enumerate projects by module and layer; explicitly capture GL/AP reference pattern implementations and the target module list.
- NDepend baseline: create an NDepend analysis baseline for the solution, capture dependency graphs and DSMs, and record hotspots for coupling and cycles.
- Runtime and dependency map: record EF6 and EF Core usage per module; identify System.Web and legacy dependencies that must remain in adapters.
- Data map: list DbContexts and their interface counterparts; identify raw SQL and stored procs and keep them in adapters.
- Test data baseline: production data handling is managed outside this project; no internal changes required.
- Operational map: document the current rapid delivery flow (dev/QA/prod) and existing release practices.
- Deliverables: inventory doc, dependency graph, module boundary map, NDepend baseline report, and a web-effect dependency ripple report.

B4) Refactor Strategy and Sequencing
- Primary approach: pattern-by-pattern extraction with branch-by-abstraction. Each slice includes tests-first characterization, core extraction, adapter wiring, and parity verification. Use GL/AP as canonical playbooks and replicate patterns module-by-module.
- Sequencing model: screen-first. Pick a legacy screen, identify direct and ripple dependencies, refactor those services, then build the matching FundViewClient screen.
- Sequencing rules: 1) write characterization tests before refactor; 2) classify the pattern against GL/AP and UtilityBilling testing playbooks; 3) map dependency ripples and batch cross-service changes; 4) extract pure logic and mapper-only code early; 5) wrap data access behind interfaces and keep EF6 in adapters; 6) update controllers/services to call new core; 7) run parity checks and NDepend; 8) update docs, ADRs, and backlog.
- Pattern catalog maintenance: for each validated pattern, record inputs/outputs, invariants, adapter boundaries, and test approach; keep GL/AP playbooks and the testing roadmap current.
- Unknown pattern handling: document how the pattern differs from known playbooks, search the codebase for other occurrences, and add all matches to the Unknown Patterns Backlog; draft candidate solutions (adapter shape, core boundaries, tests) and decide whether to promote to a new playbook. Skip unknown patterns and continue with known patterns to maintain forward progress.
- NDepend workflow: run before each wave to map dependencies and detect cycles, then on every PR and nightly to enforce "no new violations." If a violation is detected, classify it (new vs baseline), trace the dependency path, and fix by adjusting layering, adapters, or interfaces; if intentional, document the rationale and apply a narrow suppression. Keep the baseline refresh cadence per wave.
- Do-not-touch rules: no changes to behavior, schemas, APIs, or side effects; no feature work; no UI behavior changes in legacy MVC.
- Skip/rollback conditions: if behavior parity, contract stability, or tests fail, halt that slice, log the issue, and continue with other slices while the failure is investigated; unknown patterns are skipped per the workflow above.
- Deliverables: pattern catalog based on GL/AP, ADRs for core library design and adapter boundaries, and an Unknown Patterns register with occurrences and candidate solutions.

B5) Safety Nets (must be detailed)
- Unit tests: xUnit characterization tests for every extracted method; target near-100% coverage of core logic.
- Testing roadmap: follow UtilityBilling + Zoran Horvat defaults; tests are written before refactor. See `docs/refactor-plan/testing-roadmap.md`.
- Integration tests: API and MVC integration tests for all user journeys; parity validation across legacy and new services.
- Contract tests: freeze all endpoints and DTOs; verify serialized outputs match legacy behavior.
- E2E smoke tests: all modules and journeys; no exceptions due to the 1:1 reskin requirement.
- CI/CD hardening: no time limits; run as much testing as possible at all levels for each PR and nightly; prioritize deterministic, repeatable tests.
- NDepend ruleset v1 - architecture: no new cycles (assembly and namespace), enforce UI -> Adapters -> Core layering, enforce module boundaries (modules depend on shared abstractions only).
- NDepend ruleset v1 - core purity: core libraries must not reference System.Web, EF6, EF Core, Telerik/reporting, or MVC/UI packages; expand as needed.
- NDepend code-quality rules: optional report-only during reskin for visibility; no gating until post-reskin.
- NDepend suppression workflow: new violation -> confirm legacy or intentional -> document reason -> suppress narrowly (rule + scope) -> review suppressions at wave end.
- NDepend baseline refresh: baseline at Phase 0, refresh only after a completed wave with an ADR.
- NDepend report storage and review: PR + nightly runs, store HTML reports as CI artifacts with long-term GitHub retention, wave-end trend review.
- NDepend phased gating: Phase 0 baseline, Phase 1 architecture and core purity, Phase 2 expand architecture constraints as needed; all phases fail on new violations only.
- NDepend rules source: `docs/refactor-plan/ndepend/ndepend-rules.ndrules` (architecture-only during reskin).
- Observability: add start and stop logs and duration metrics at core use-case boundaries without altering behavior.
- Data safety: no schema changes. If a change is unavoidable, stop and replan.
- Feature controls: branch-by-abstraction with runtime toggles only when they do not change behavior; default cutover is no-op in terms of behavior.
- Deliverables: Refactor Safety Net spec with exact parity checks and coverage thresholds.

B6) Architecture Refactor Plan
- Current architecture: controllers call helper services and repositories directly; logic mixed with UI and data access.
- Target architecture: Controllers -> UseCase/Core logic -> Ports/Interfaces -> Adapters (EF6 or EF Core, reporting, integrations).
- Core naming and placement: follow existing GL/AP project naming and placement; no new naming conventions unless GL/AP already uses them.
- Dependency injection: legacy MVC uses the existing manual injection pattern in Areas Services; new services use DI container injection.
- API evolution plan: no API or DTO changes. Any behavioral change is out-of-scope.
- Shared concerns: preserve `IRequestContext`, `ILogger<T>`, and existing policies; use the same patterns as GL/AP.
- Deliverables: target architecture diagram, module boundary map, ADRs, and DI wiring guidance for MVC and new services.

B7) Code Transformation Plan (mechanics)
- Conventions: use `$(DualTargetFrameworkVersionNames)` for core libs; no System.Web or EF references in core; xUnit for tests; keep API contracts unchanged.
- Safe micro-refactors: extract method or class, rename, inline, and pure function extraction with parity tests.
- Risky refactors: not permitted unless behavior-neutral and proven by tests.
- Pattern playbook - MVC Areas Services injection: use the GL/AP pattern where MVC Services manually inject the refactored core service and delegate calls without behavior changes.
- Pattern playbook - Controller and HelperService: move business logic into UseCase classes in the core layer; controller remains thin.
- Pattern playbook - HelperService with CRUD and DbContext: extract rules to core; keep EF in adapters; inject `I<Module>DbContext` interface.
- Pattern playbook - Repository and TableQueryResult: move query specs to core; adapters execute EF queries without changing results.
- Pattern playbook - Mapper-only logic: move pure mapping to core; keep EF projections in adapters.
- Pattern playbook - Report generation: extract data shaping to core; keep reporting engines in adapters.
- Pattern playbook - Import pipelines: extract parsing and validation rules to core; keep IO and vendor SDK in adapters.
- Large-scale change tactics: codemods and batch updates aligned with dependency ripples; use GL/AP as the gold standard for naming and structure.
- Unknown pattern handling: log to Unknown Patterns Backlog; pause and pair-program before proceeding.
- Deliverables: automation plan, pattern catalog, AI context packet templates, review checklist, GL/AP playbooks at `docs/refactor-plan/gl-ap-playbooks.md`.

B8) Delivery Plan (work breakdown)
- Phase 0 (2-3 weeks): baseline discovery and safety nets; exit criteria: pattern catalog v1 based on GL/AP and NDepend baseline report.
- Phase 1 (2 weeks): core library scaffolding and DI templates for MVC and new services aligned to GL/AP naming.
- Phase 2 (3-5 weeks): CR module extraction by pattern, plus FundViewClient 1:1 UI translation; exit criteria: parity tests green and near-100% coverage.
- Phase 3 (3-5 weeks): Code Enforcement module extraction and UI translation; same exit criteria.
- Phase 4 (3-5 weeks): Permits module extraction and UI translation; same exit criteria.
- Phase 5 (3-5 weeks): System module extraction and UI translation; same exit criteria.
- Phase 6 (3-5 weeks): Municipal Court module extraction and UI translation; same exit criteria.
- Overall timeline: target 6 months with parallel work where web effects require cross-module refactors.
- Owners and RACI: Module Owner (R), Refactor Lead (A), QA Lead (C), Ops (C).
- Backlog governance: reskin and bugfix only; follow current branching strategy; small PRs with required reviews.
- Deliverables: roadmap, per-module parity checklist, and FundViewClient translation checklist.

B9) Release, Rollout, and Rollback
- Environments: dev, QA, prod as services complete; no fixed cadence.
- Deployment strategy: release tested changes continuously; legacy MVC uses shared core logic immediately.
- Rollout: after full reskin and parity validation, beta the new application (FundViewClient plus new services), then retire the legacy application after sign-off.
- Rollback: revert to previous core library and adapter versions if any parity failure appears.
- Deliverables: release runbook and rollback checklist.

B10) Validation and Quality Gates
- Functional equivalence: golden master outputs for reports and calculations; snapshot comparisons for DTOs and responses across MVC and API.
- UI parity: FundViewClient must match legacy behavior exactly before beta sign-off.
- Performance validation: ensure no regressions relative to existing baseline; document exceptions.
- Security validation: maintain existing authZ checks and policy enforcement points; no compliance changes required.
- NDepend gates: architecture-only gates fail on new violations; code-quality reports are informational during reskin; baseline refresh per completed wave.
- Deliverables: go or no-go checklist and parity validation report.

B11) Documentation + Training + Handover
- Documentation updates: module parity checklists, pattern catalog, ADRs, DI wiring guides, and FundViewClient translation notes.
- Team enablement: training on GL/AP patterns, core extraction, dual-targeting, and NDepend usage.
- Handover: module owners sign off on parity and readiness to retire legacy application.

B12) Sustainment (post-refactor)
- Guardrails: CI checks to prevent System.Web or EF usage in core; NDepend rules to prevent new cycles and layering violations.
- Legacy retirement: remove EF6 adapters and legacy MVC modules after beta success; keep core logic as single source of truth.
- Ongoing modernization: after reskin completion, re-open feature work and consider API improvements.

C) BEST-PRACTICE VS ASSUMPTION MAP
- Item: 1:1 reskin with zero behavior change. Best-practice basis: risk minimization and contract stability. Assumptions: parity tests can detect any drift. Evidence signals: existing GL/AP patterns that already do this.
- Item: Dual-targeted core libraries. Best-practice basis: shared logic across legacy and new services. Assumptions: extracted logic can avoid System.Web and EF. Evidence signals: GL/AP dual-targeting.
- Item: Branch-by-abstraction. Best-practice basis: safe incremental replacement. Assumptions: abstractions can be introduced without behavior change. Evidence signals: current pattern usage in GL/AP.
- Item: Near-100% test coverage. Best-practice basis: change safety for legacy code. Assumptions: test data and harness can scale. Evidence signals: xUnit adoption in repo.
- Item: Pattern-by-pattern sequencing. Best-practice basis: repeatable automation. Assumptions: patterns are consistent across modules. Evidence signals: similar structures in services and repositories.
- Item: NDepend architectural governance. Best-practice basis: dependency visualization and cycle detection. Assumptions: baseline can be established and enforced. Evidence signals: NDepend analysis reports.
- Item: Production-derived test data. Best-practice basis: realistic parity validation. Assumptions: data handling and access controls are acceptable. Evidence signals: existing local data copy practice.

D) REQUIREMENTS THAT MUST BE EXACT
- API contract stability: all endpoints and DTOs frozen; decisions by Product and API Lead; deadline before Phase 1; failure impact: client breakage.
- Behavior parity: zero behavior change across all journeys; decisions by Product and QA Lead; deadline before Phase 2; failure impact: regression risk.
- Schema stability: no schema changes; decisions by Data owner; deadline before Phase 1; failure impact: data integrity risk.
- UI parity in FundViewClient: identical behavior to legacy; decisions by Product; deadline before beta; failure impact: blocked retirement.
- Legacy retirement: occurs after full reskin and beta sign-off; decisions by Product; deadline before rollout; failure impact: duplicated maintenance.
- Core naming consistency: follow GL/AP patterns; decisions by Architecture owner; deadline before Phase 1; failure impact: fractured architecture.

E) OPTIONS + TRADEOFFS (Decision Matrix)
- NDepend rule scope: architecture-only vs architecture plus code quality. Decision: architecture-only during reskin, expand post-reskin as needed.
- NDepend gating: fail on new violations only vs fail on all violations vs phased rollout. Decision: fail on new violations only with phased rollout.
- NDepend cadence: PRs plus nightly vs nightly only. Decision: PRs plus nightly.
- NDepend forbidden dependencies: none vs minimal core purity list vs strict list. Decision: minimal core purity list, expand as needed.
- Test data strategy: production copy only vs production copy plus deterministic seed and snapshots. Decision: production copy for now, document and control access.
- Sequencing model: screen-first vs module-first. Decision: screen-first driven by legacy screens.
- Core library naming: follow GL/AP patterns vs introduce new naming. Decision: follow GL/AP patterns.
- UI translation: 1:1 parity vs incremental UX changes. Decision: 1:1 parity.
- Test depth: minimal vs balanced vs near-100%. Decision: near-100%.

F) RISK REGISTER + MITIGATIONS
- Risk: behavior drift despite refactor. Likelihood: Medium. Impact: High. Detection: parity test failures. Mitigation: golden master tests and strict contract checks. Contingency: rollback to previous core and adapter.
- Risk: dependency web effect increases scope. Likelihood: High. Impact: Medium. Detection: growing cross-module change sets. Mitigation: dependency mapping and batching. Contingency: re-sequence with smaller slices.
- Risk: near-100% coverage slows timeline. Likelihood: Medium. Impact: Medium. Detection: CI time and test backlog growth. Mitigation: parallelize test creation and reuse GL/AP patterns. Contingency: adjust phase scopes, not coverage targets.
- Risk: dual-target friction in shared core. Likelihood: Medium. Impact: Medium. Detection: build errors for netstandard2.0. Mitigation: core dependency rules and adapter boundaries. Contingency: keep logic in adapters until refactor tooling is ready.
- Risk: UI translation parity issues in FundViewClient. Likelihood: Medium. Impact: High. Detection: UI parity tests. Mitigation: map legacy behaviors and run automated UI tests. Contingency: block beta until parity achieved.
- Risk: NDepend noise or false positives. Likelihood: Medium. Impact: Low. Detection: repeated rule churn. Mitigation: baseline and suppression rules. Contingency: tune rules and thresholds.
- Risk: production data access delays. Likelihood: Medium. Impact: Medium. Detection: test environment instability. Mitigation: document data copy steps and access. Contingency: add deterministic seed data.

G) OPEN QUESTIONS BACKLOG
- None for current scope; revisit if NDepend or data-handling policies change.
