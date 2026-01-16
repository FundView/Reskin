# 0011 - Direct ConfigurationManager Access

- Pattern label: Services/controllers/helpers read `ConfigurationManager.AppSettings` or `ConfigurationManager.ConnectionStrings` directly.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects configuration to be injected via DI (options, context configs, or providers). Direct access couples logic to legacy config files and makes .NET 9 reuse and test isolation harder.
- Search results (other occurrences, paths): 6 active files with direct `ConfigurationManager` usage outside CodeSmith; only 1 found outside MVC4 UI (FastSoftware.NotificationsAPI).
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce typed configuration objects (`IOptions<T>`) or `IConnectionStringProvider`.
- Keep configuration access in MVC adapters or composition root only.
- Replace direct calls with injected providers following GL/AP patterns.

Subpatterns
- MVC4 UI configuration access (controllers/helpers/services).
- Fast.* service layer configuration access (non-MVC).

Subpattern: MVC4 UI configuration access
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Services/CardConnectConnectionStringService.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/AccountController.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/Massive.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs`

Subpattern: Fast.* service layer configuration access
- Search results list (active):
- `FundViewGit/FastSoftware.NotificationsAPI/EmailNotificationService.cs`
