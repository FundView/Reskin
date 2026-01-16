# 0039 - FVDataContextFactory global registries and user cache

Pattern label:
- Reflection-based service/viewmodel registries + global user cache (FVDataContextFactory)

Service/controller:
- FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs (FVDataContextFactory)
- FundView.MVC4.UI/Global.asax.cs
- FundView.MVC4.UI/Controllers/BaseController.cs
- FundView.MVC4.UI/CustomSelectListsFromAPI.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs

Difference summary (vs known playbooks):
- Static dictionaries store user info, service method registries, controller methods, and viewmodel types built via reflection at runtime.
- User/tenant context is pulled via HttpContext and cached globally; membership database is queried during initialization.
- Hard-coded assembly load path is used to build controller registry; registry lookups are not explicit dependencies.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Global.asax.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/CustomSelectListsFromAPI.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemActionSetService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Models/SystemActionSet/SystemActionSetApplyViewData.cs

Candidate solutions (core boundary, adapter shape, tests):
- Core port: explicit registry interfaces for service/viewmodel discovery; build registries in MVC adapter startup only.
- Replace global user cache with scoped user context abstraction; keep membership lookups in adapters.
- Characterization tests before refactor: registry population, service lookup behavior, and user/role resolution parity.
