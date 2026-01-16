# 0083 - Reflection-Based Dynamic Type Instantiation

- Pattern label: Runtime type creation via `Activator.CreateInstance` (including string-based type names) to build request parameters, view models, and EFModel entities on the fly.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs`, `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs`, `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemActionSetService.cs`, `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/ConversionExtension.cs`, `FundViewGit/FundView.MVC4.UI/Import/ImportHelper.cs`, `FundViewGit/FundView.MVC4.UI/EFModel/Repository/GenericRepo.cs`, `FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Controllers/ReceiptingMasterController.cs`, `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Models/SystemGroupTemplate/SystemGroupTemplateSystemGroupTemplateGroupViewData.cs`.
- Difference summary (vs known playbooks): GL/AP patterns favor explicit DI and strongly typed factories. Reflection-based instantiation hides dependencies, bypasses DI, makes type discovery implicit, and complicates .NET 9 reuse and testing (no compile-time safety or clear registration list).
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs` (dynamic request parameter + view model creation)
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemActionSetService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/ConversionExtension.cs`
- `FundViewGit/FundView.MVC4.UI/Import/ImportHelper.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/GenericRepo.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Controllers/ReceiptingMasterController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Models/SystemGroupTemplate/SystemGroupTemplateSystemGroupTemplateGroupViewData.cs`
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce explicit type maps/factories (dictionary of string key -> Type) registered in composition root; keep reflection inside legacy adapter if needed.
- Replace string-based type resolution with typed factories or DI registrations in .NET 9 services.
- Add characterization tests that assert the allowed type map and expected outputs before refactor (input key -> concrete type and behavior).
