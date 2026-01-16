# 0023 - Repositories Returning HTML Markup / UI Actions

- Pattern label: EFModel repositories build HTML fragments (`<a>`, `<input>`, `data-fv` URLs) and return them as part of data results.
- Service/controller: `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtViolationRepo.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects repositories to return data, not UI markup. Embedding HTML and route links in repositories couples data access to MVC rendering and makes core extraction and Angular migration harder.
- Search results (other occurrences, paths): widespread across MunicipalCourt, BuildingPermit, and System repositories.
- Candidate solutions (core boundary, adapter shape, tests):
- Replace HTML string construction with DTO fields (ids, labels, action types) and build markup in MVC views or Angular components.
- Keep legacy HTML construction in MVC adapters only; core returns data and action metadata.
- Add characterization tests that validate rendered actions (URLs/labels) before refactor.

Search results list (active)
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/BuildingPermit/BPFeeRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/BuildingPermit/BPPermitRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtViolationRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtUtilityRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtPaymentPlanRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/System/SystemPropertyRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/System/SystemSubmittalRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/System/SystemContactRepoService.cs`
