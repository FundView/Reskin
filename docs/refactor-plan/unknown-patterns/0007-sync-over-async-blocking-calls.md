# 0007 - Sync-over-Async Blocking Calls

- Pattern label: Legacy MVC and helpers block on async operations via `GetAwaiter().GetResult()`.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/SQLHelper.cs`.
- Difference summary (vs known playbooks): async APIs are forced into sync execution inside MVC controllers, helpers, and EFModel helpers. This spreads sync-over-async calls across layers, which makes it harder to define clean async-first core boundaries for .NET 9 while preserving MVC behavior.
- Search results (other occurrences, paths): 13 active files with `GetAwaiter().GetResult()` in controllers/helpers/services; additional commented occurrences in MVC migrations and legacy helpers.
- Candidate solutions (core boundary, adapter shape, tests):
- Keep async APIs in core; expose sync wrappers only in legacy adapters (or a centralized `LegacyAsyncBridge`).
- Add a dependency rule so new sync-over-async usage is only allowed in the legacy adapter layer.
- Add characterization tests around sync adapter entry points to confirm parity before any async refactor.

Search results list (active)
- `FundViewGit/FundView.MVC4.UI/Service References/App_Start/Startup.Auth.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/SQLHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Services/FastGovPayAPIService.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/spJurorNumberHelper.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/spCourtTransactionNumberHelper.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/spBPTransactionNumberHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPPermitReceiptingAPIController.cs`
- `FundViewGit/FundView.MVC4.UI/CustomSelectListsFromAPI.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/AccountController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemUsersController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemContactsController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtOCAGroupController.cs`
