# 0009 - FundViewReadModel Creation Outside DI

- Pattern label: Controllers/helpers create `FundViewReadModel` directly or via `GetHttpContextContext` instead of resolving via DI or context providers.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Controllers/FastGovPayMunicipalCourtController.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects MVC shims to resolve `IFundViewReadModel` via context/DI. Direct instantiation bypasses DI scoping, hides the context source, and makes EF6/EF Core swap and .NET 9 reuse harder.
- Search results (other occurrences, paths): 6 active files with `new FundViewReadModel(...)` usage outside tests; 2 active call sites for `FundViewReadModel.GetHttpContextContext(...)` plus the provider class.

Subpatterns
- Manual `new FundViewReadModel(...)` (connection string or AsyncContext) in controllers/helpers.
- HttpContext-scoped context retrieval via `FundViewReadModel.GetHttpContextContext(...)`.
- Provider bridge: `FundViewReadModelProvider` returns `GetHttpContextContext(...)`.
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce an `IFundViewReadModelFactory`/`IContextFactory` to create contexts from connection strings or AsyncContext.
- Keep connection string discovery in MVC adapters; pass only the factory/context into core services.
- Replace `new FundViewReadModel(...)` with injected factory calls following GL/AP patterns.
- Centralize `GetHttpContextContext(...)` usage inside MVC adapter boundaries only; prefer explicit factory/context injection elsewhere.
- Add characterization tests for FastGovPay flows and `CustomSelectListsFromAPI` before refactor.

Search results list (manual `new FundViewReadModel(...)`)
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Controllers/CardConnectController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Controllers/FastGovPayBuildingPermitController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Controllers/FastGovPayMunicipalCourtController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemDocumentController.cs`
- `FundViewGit/FundView.MVC4.UI/CustomSelectListsFromAPI.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/SQLHelper.cs`

Search results list (GetHttpContextContext usage)
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/FundViewReadModelProvider.cs`

Creation implementation
- `FundViewGit/FundView.MVC4.UI/EFModel/Models/FundViewReadModel.cs` (defines `GetHttpContextContext`, instantiates new context when missing)
