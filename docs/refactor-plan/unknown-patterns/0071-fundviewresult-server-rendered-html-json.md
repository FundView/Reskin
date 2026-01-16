# 0071 - FundViewResult Server-Rendered HTML in JSON Responses

- Pattern label: FundViewResult JSON envelope that embeds rendered Razor partials (`html`) and an action list (`action`) alongside `entityData`/alerts; grids can include `sFundViewResult`.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Controllers/MasterController.cs` (uses FundViewResult for entityData responses).
- Difference summary (vs known playbooks): Instead of clean JSON DTOs, MVC actions return a custom JSON shape that mixes server-rendered HTML and UI actions. This couples MVC rendering to client behaviors and complicates reuse in .NET 9 APIs and Angular (client expects `sFundViewResult` and `action` semantics).
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/FundViewResult.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/GridHelper.cs` (`sFundViewResult` in grid payload)
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Extensions/ModelState.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/MasterController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtCitationController.cs`
- `FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView.js` (client-side processing of response actions)
- Candidate solutions (core boundary, adapter shape, tests):
- Keep FundViewResult in MVC adapters; map core responses to a typed DTO for .NET 9 services.
- For Angular, replace server-rendered HTML with client-rendered templates; keep a compatibility adapter only for legacy MVC.
- Add characterization tests around FundViewResult serialization and client action handling to lock response shape.
