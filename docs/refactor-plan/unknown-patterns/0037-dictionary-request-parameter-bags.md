# 0037 - Stringly-typed request parameter bags (Dictionary<string, string>)

Pattern label:
- Stringly-typed request parameter bags passed through services/repos/viewdata (Dictionary<string, string> / IDictionary)

Service/controller:
- FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingTransactionService.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemPropertyService.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemTemplateService.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemHelpService.cs
- FundView.MVC4.UI/EFModel/Repository/BuildingPermit/BPPermitRepo.cs
- FundView.MVC4.UI/EFModel/Repository/CashReceipting/ReceiptingTransactionItemRepository.cs
- FundView.MVC4.UI/Areas/Dashboard/Services/WhatsNewService.cs
- Fast.FundView.AccountsPayable.Services/APVendorService.cs
- Fast.FundView.Legacy/Extensions/IDictionaryExtensions.cs

Difference summary (vs known playbooks):
- Services and repositories accept raw `Dictionary<string, string>` inputs and parse values with ad-hoc key lookups and conversions.
- Request semantics are implicit (string keys, optional/default behaviors), making core extraction and DTO mapping error-prone.
- GL/AP playbooks favor strongly-typed request DTOs and explicit validation; this pattern embeds MVC request coupling in logic.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingTransactionService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingBatchService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemPropertyService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemTemplateService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemHelpService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/BuildingPermit/BPPermitRepo.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/CashReceipting/ReceiptingTransactionItemRepository.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/Dashboard/Services/WhatsNewService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APVendorService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.SystemGlobals.Services/SystemGlobalHelperService.cs

Candidate solutions (core boundary, adapter shape, tests):
- Core port: strongly-typed request DTOs (commands/queries) with explicit validation and defaults.
- MVC adapters map `Dictionary<string, string>` (or NameValueCollection) into DTOs and handle optional keys.
- Characterization tests before refactor: missing key behavior, default values, and key-to-field mapping parity.
