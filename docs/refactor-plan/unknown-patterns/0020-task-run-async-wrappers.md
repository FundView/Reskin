# 0020 - Task.Run Async Wrappers in Repos/Helpers

- Pattern label: Services/helpers/repositories use `Task.Run` to wrap synchronous work or return placeholder tasks.
- Service/controller: `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtFeeRepo.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects true async boundaries or explicit sync APIs. `Task.Run` wrappers introduce thread-pool usage and blur async semantics, which complicates extraction and testing in .NET 9.
- Search results (other occurrences, paths): 7 active files using `Task.Run` outside `TaskHelper` (fire-and-forget handled separately).
- Candidate solutions (core boundary, adapter shape, tests):
- Replace placeholder `Task.Run` with `Task.FromResult`/synchronous returns in core.
- For CPU-bound work (reports/docx), centralize in adapter services with explicit timeouts and cancellation support.
- Add characterization tests around any `Task.Run` usage to confirm timing/timeout behavior and deterministic results.

Subpatterns
- CPU/interop offloading via `Task.Run` (reports, docx conversion).
- Placeholder async responses (`Task.Run(() => 1)` / `Task.Run(() => new List<T>())`).
- Async wrappers around LINQ/data shaping (repositories).
- Parallelization helpers using `Task.Run` (partitioned loops).

Subpattern: CPU/interop offloading
- Search results list (active):
- `FundViewGit/Fast.FundView.Legacy.Telerik/ReportGenerator.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/DocxConvert.cs`

Subpattern: Placeholder async responses
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/EFModel/Models/FundViewReadModel.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Wrappers/AsyncContext.cs`

Subpattern: Async wrappers around data shaping
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtViolationRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtFeeRepo.cs`

Subpattern: Parallelization helpers
- Search results list (active):
- `FundViewGit/CopSync/Extensions.cs`
