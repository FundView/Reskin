# 0080 - ExcludeAllModelBinder on Grid ViewData

- Pattern label: Model binder attribute disables binding for specific ViewData types (grid payloads) by returning null and skipping property assignment.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Binder/ExcludeAllModelBinder.cs`, `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtOfficerViewData.cs` (CourtOfficerGridViewData).
- Difference summary (vs known playbooks): GL/AP playbooks assume standard binding or DTO mapping. This pattern explicitly opts out of MVC binding for a ViewData class, likely to prevent overposting or avoid binding large grid payloads. .NET 9 endpoints will need an equivalent opt-out (e.g., `[BindNever]`) or explicit request models to preserve behavior.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Binder/ExcludeAllModelBinder.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtOfficerViewData.cs` (ModelBinder attribute on CourtOfficerGridViewData)
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtJudgeViewData.cs` (commented binder usage)
- Candidate solutions (core boundary, adapter shape, tests):
- Use `[BindNever]`/custom binders in MVC and explicit request DTOs in .NET 9 to avoid accidental binding.
- Add characterization tests for affected endpoints to confirm unchanged binding behavior (null model vs default values).
- Document this binder opt-out in the refactor playbook so extracted controllers/services preserve it.
