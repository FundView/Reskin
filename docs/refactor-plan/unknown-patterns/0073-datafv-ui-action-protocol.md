# 0073 - DataFv UI Action Protocol (data-fv JSON Command Bus)

- Pattern label: DataFv JSON payloads embedded in `data-fv` attributes to drive UI navigation (modal, new tab, file download, method call), optional callbacks, and reload behavior.
- Service/controller: `FundViewGit/FastScreenBuilder/DataFv.cs` (serialization + command model).
- Difference summary (vs known playbooks): UI behavior is driven by a custom command protocol instead of typed API responses or standard routing. The client JS parses `data-fv` and executes actions, so UI flow is tightly coupled to serialized JSON and legacy scripts, which complicates Angular migration and .NET 9 service reuse.
- Search results (other occurrences, paths):
- `FundViewGit/FastScreenBuilder/DataFv.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/GridHelper.cs` (creates data-fv buttons/links)
- `FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView.js` (parses `data-fv` and executes actions)
- `FundViewGit/FundView.MVC4.UI/Views/Shared/ModuleNavbars/*.cshtml` (data-fv in nav links)
- Candidate solutions (core boundary, adapter shape, tests):
- Preserve DataFv behavior in MVC adapters; define a translation layer to Angular router/actions for new UI.
- Replace data-fv with explicit client-side commands in FundViewClient while keeping legacy `data-fv` support during dual-run.
- Add characterization tests around DataFv serialization and JS action handling (modal/tab/file/reload/callback).
