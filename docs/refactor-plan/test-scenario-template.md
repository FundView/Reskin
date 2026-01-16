# Reusable Test Scenario Template (UtilityBilling Pattern)

Use this template to create scenario-based unit tests that mirror the UtilityBilling approach.
It is optimized for multi-step domain flows and defaults to Zoran Horvat-style maintainable tests.

## When to use
- Multi-step stateful logic (meter reads, adjustments, corrections, rollovers).
- Behavioral rules that need narrative framing and repeatable steps.
- Tests that should use real collaborators instead of mocks.

## Structure
- A `TestScenarioCreator` that wires real collaborators.
- A `TestScenario<T>` that collects `Action<T>` steps and executes them in order.
- A lightweight `TestEntity` that implements the required interfaces.
- Tests that narrate the scenario and assert outcomes after execution.

## Template (copy and adapt)
```csharp
using Xunit;

namespace <Your.Module>.Tests
{
    public static class TestScenarioCreator
    {
        public static TestScenario<T> CreateTestScenario<T>()
            where T : IUseCaseInput, IStateCarrier
        {
            var serviceA = new ServiceA();
            var serviceB = new ServiceB();
            var useCase = new UseCase<T>(serviceA, serviceB);

            return new TestScenario<T>(useCase);
        }
    }

    public record TestScenario<T>(UseCase<T> UseCase)
        where T : IUseCaseInput, IStateCarrier
    {
        private List<Action<T>> Actions { get; init; } = new();

        public TestScenario<T> AddStep(Action<T> action)
        {
            var next = Actions.ToList();
            next.Add(action);
            return this with { Actions = next };
        }

        public TestScenario<T> ApplyDomainStep(UseCaseInput input)
        {
            return AddStep(entity => UseCase.UpdateEntity(entity, input));
        }

        public void Execute(T entity)
        {
            foreach (var action in Actions)
            {
                action(entity);
            }
        }
    }

    public sealed class TestEntity : IUseCaseInput, IStateCarrier
    {
        // Minimal state needed for the scenario.
        public int? BeginRead { get; set; }
        public int? EndRead { get; set; }
        public int BilledUsage { get; set; }
    }

    public class UseCaseTests
    {
        [Fact]
        public void UpdateEntity_CorrectsUsage_WhenClerkFixesErroneousRead()
        {
            // Scenario:
            // Begin read 1000, end read wrongly entered as 20000,
            // correction applies offset, next read entered at 22000.

            var scenario = TestScenarioCreator.CreateTestScenario<TestEntity>()
                .ApplyDomainStep(new UseCaseInput { BeginRead = 20000, EndRead = 22000 });

            var testCase = new TestEntity();

            scenario.Execute(testCase);

            Assert.Equal(20000, testCase.BeginRead);
            Assert.Equal(22000, testCase.EndRead);
        }
    }
}
```

## Guidance
- Keep the scenario narrative at the top of each test.
- Prefer real collaborators unless the dependency is nondeterministic or expensive.
- Use data-driven tests (`[Theory]` + `MemberData`) for combinatorial inputs.
- Keep test bodies short; move sequence logic into scenario builder methods.

## References
- UtilityBilling examples: `FundViewUniverse/UtilityBilling/Fast.FundView.UtilityBilling.MeteredServices.Tests`
- Resource guide: `# Resource Guide Zoran Horvat.md`
