# 0019 - Runtime Assembly Loading for Reflection Registries

- Pattern label: Helpers load assemblies at runtime (`Assembly.LoadFrom` / `Assembly.Load`) to build service/controller/property registries.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs` (GetControllerDictionaryAPI).
- Difference summary (vs known playbooks): GL/AP playbook expects explicit references and DI-driven registration. Runtime assembly loading (especially with hard-coded paths) couples behavior to deployment layout and makes .NET 9 reuse and testing harder.
- Search results (other occurrences, paths): 2 active files using assembly loading for reflection registries.
- Candidate solutions (core boundary, adapter shape, tests):
- Replace runtime assembly loading with DI registration or explicit assembly references at startup.
- Move reflection registries behind an adapter that receives `Assembly` instances from composition root.
- Add characterization tests around registry outputs (expected controller/service keys) before refactor.

Subpatterns
- Hard-coded file path assembly load (`Assembly.LoadFrom`).
- Named assembly load with fallback (`Assembly.Load("CodeSmith")`).

Subpattern: Hard-coded file path assembly load
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs`

Subpattern: Named assembly load with fallback
- Search results list (active):
- `FundViewGit/Fast.FundView.Legacy/Extensions/PropertyInfoExtensions.cs`
