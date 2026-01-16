# 0085 - Parallel.ForEach helper conversions and data hydration

- Pattern label: Helper methods use `Parallel.ForEach` to mutate collections or model instances during data hydration and conversion.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs`, `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/SQLHelper.cs`, `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ConversionHelper.cs`.
- Difference summary (vs known playbooks): These helpers rely on in-method parallelism and thread-safe collections to produce outputs. GL/AP patterns tend to keep core logic deterministic and single-threaded, so concurrency assumptions (ordering, thread-safety, shared state) need to be preserved explicitly during extraction.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/SQLHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ConversionHelper.cs`
- Candidate solutions (core boundary, adapter shape, tests):
- Keep the parallel loop in the legacy adapter where behavior is already validated; expose a core method that accepts a prepared collection.
- Add characterization tests that assert output contents for representative inputs (treat ordering as undefined unless callers rely on it).
- Document concurrency assumptions (thread-safe collections, no shared mutable EF contexts) before moving logic into .NET 9 core.
