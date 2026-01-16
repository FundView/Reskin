# 0038 - Context-based service locator (GetRequiredService / ServiceProviderHelper)

Pattern label:
- Context-based service locator via IFundViewReadModel.GetRequiredService / ServiceProviderHelper

Service/controller:
- FundView.MVC4.UI/EFModel/Models/FundViewReadModel.cs
- FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingTransactionService.cs
- FundView.MVC4.UI/Areas/BuildingPermit/Services/BPProjectPermitService.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemWhitelistService.cs
- FundView.MVC4.UI/Reports/ReportGenerator.cs
- FundView.MVC4.UI/Controllers/BaseController.cs

Difference summary (vs known playbooks):
- Services and view models resolve dependencies at runtime through `context.GetRequiredService<T>()` or `ServiceProviderHelper`, hiding explicit dependencies.
- EF context doubles as a DI container/service locator; service resolution is tied to HttpContext/OWIN and legacy provider setup.
- GL/AP playbooks prefer explicit constructor injection and narrow ports; this pattern complicates core extraction and DI boundaries for .NET 9 reuse.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Models/FundViewReadModel.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingTransactionService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Services/BPProjectPermitService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemWhitelistService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Reports/ReportGenerator.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Models/GLBudgetAdjustment/GLBudgetAdjustmentEditViewData.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtViolation/CourtViolationEditViolationNonCashPaymentActionSetModalViewData.cs

Candidate solutions (core boundary, adapter shape, tests):
- Core port: explicit service dependencies passed via constructors or method parameters; avoid resolving from EF context.
- Keep service locator in MVC adapters temporarily; map resolved services into core calls.
- Characterization tests before refactor: verify runtime service resolution paths (OWIN vs ServiceProviderHelper) and ensure scoped lifetimes remain intact.
