# Testing Roadmap (UtilityBilling + Zoran Horvat Defaults)

## Defaults and non-negotiables
- Tests are written before refactoring; characterization tests gate every extraction.
- Default to Zoran Horvat's focus on testability-through-design and maintainable unit tests; treat tests as first-class production code.
- Prefer real collaborators over mocks when the cost is low and behavior is deterministic.
- Keep tests behavior-focused and domain-language-driven; avoid asserting on incidental implementation details.

## Observed UtilityBilling test patterns to standardize
- Scenario narratives + AAA: tests include a short domain story in comments, then `Arrange/Act/Assert`. Example: `UtilityBilling/Fast.FundView.UtilityBilling.MeteredServices.Tests/BilledReadsUpdaterTests.cs`.
- Scenario builder with domain actions: fluent `TestScenario` composes a list of actions (`AddMeterRead`, `UpdateBilledReads`, `IncrementBeginReadOffset`, etc.) and then `Execute`s them. Example: `UtilityBilling/Fast.FundView.UtilityBilling.MeteredServices.Tests/Models/TestScenario.cs`.
- Real collaborators in unit tests: tests use real services (e.g., `RealUsageUpdater`, `UsageAdjustmentApplier`, `BilledReadsUpdater`) instead of mocking. Example: `UtilityBilling/Fast.FundView.UtilityBilling.MeteredServices.Tests/TestScenario.cs`.
- Lightweight test entities: a single `TestUsageEntity` implements multiple interfaces to mirror production contracts without DB/IO. Example: `UtilityBilling/Fast.FundView.UtilityBilling.MeteredServices.Tests/Models/TestUsageEntity.cs`.
- Data-driven tests for pure logic: small, focused methods use `[Theory]` with `[InlineData]`. Example: `UtilityBilling/Fast.FundView.UtilityBilling.MeteredServices.Tests/RolloverUsageCalculatorTests.cs`.
- Structured test cases for combinatorics: `MemberData` with explicit `TestCase` records to keep inputs/expected outputs readable. Example: `UtilityBilling/Fast.FundView.UtilityBilling.MeteredServices.Tests/PendingReadUpdaterTests.cs`.

## Roadmap: how we write tests during the reskin
1) Behavior capture (pre-refactor, required)
   - For each service/controller, write characterization tests that lock current behavior.
   - Start with the highest-risk scenarios, then fill in edge cases and boundary values.
   - Use UtilityBilling-style scenario builders when the logic is stateful or sequence-driven.
2) Extraction safety net (during refactor)
   - Keep the same tests; refactor code until all characterization tests pass unchanged.
   - Add new tests only when new adapters or integration seams are introduced.
3) Parity verification (post-extraction)
   - Add contract and snapshot tests that compare legacy outputs to the new service layer.
   - Keep the tests deterministic; no time, IO, or random dependencies without explicit controls.

## Pattern guidance (default to UtilityBilling + Zoran Horvat)
- Use domain language in test names: encode the business rule, not the method name.
- Prefer scenario builders for multi-step flows; keep each builder method atomic and domain-focused.
- Prefer real objects over mocks unless a dependency is nondeterministic or expensive.
- Use data-driven tests for pure functions and numeric rules.
- Keep AAA visible; long scenarios live in builder methods, not inside the test body.

## Extraction checklist (tests-first requirement)
- Capture behavior with characterization tests before touching production code.
- Confirm tests fail when behavior changes (sanity check).
- Refactor/extract; tests must pass without updates.
- Only then add tests for new seams (adapters/ports).

## References
- Resource guide: `# Resource Guide Zoran Horvat.md`
