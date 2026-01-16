# 0044 - ServiceAttribute reflection-based dispatch (FVLoad service routing)

Pattern label:
- Reflection-based service routing via `[Service]` attributes and dynamic invocation

Service/controller:
- FundView.MVC4.UI/Controllers/BaseController.cs (FVLoad routing + reflection invocation)
- FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs (ServiceDictionary)
- FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingTransactionService.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationService.cs
- FundView.MVC4.UI/Areas/AccountsPayable/Services/APInvoiceService.cs

Difference summary (vs known playbooks):
- Legacy MVC uses a custom dispatcher: services are static classes decorated with `[Service]` and invoked via reflection based on URL parsing.
- Parameter binding and return types are constrained (e.g., `Task<string>`/`Task<DocX>` + `(request, IFundViewReadModel)`), not standard MVC controllers.
- This custom routing pattern is tightly coupled to MVC and makes .NET 9 reuse harder without an adapter.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingTransactionService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/AccountsPayable/Services/APInvoiceService.cs

Candidate solutions (core boundary, adapter shape, tests):
- Keep ServiceAttribute dispatch in legacy adapters; new .NET 9 controllers call the same application services directly.
- Introduce explicit controller endpoints that forward to service methods, replacing reflection routing over time.
- Characterization tests before refactor: verify URL->service resolution, parameter binding, and output shape for FVLoad paths.
