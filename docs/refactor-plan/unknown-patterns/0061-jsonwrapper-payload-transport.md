# 0061 - JsonWrapper JSON payload transport (implicit string conversions)

Pattern label:
- JsonWrapper-based payload transport in ViewData/DTOs (implicit string <-> object conversion).

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs (injects JsonWrapper<jQueryDataTableParamModel> into request)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/AccountsPayable/Models/APInvoiceAccount/APInvoiceAccountEditViewData.cs (Inputs JsonWrapper handling)

Difference summary (vs known playbooks):
- Request dictionaries and ViewData store JsonWrapper<T> values or JSON strings that rely on implicit conversion to typed objects.
- JsonWrapper uses Newtonsoft.Json with custom converters (AnyDecimalConverter) and ReferenceLoopHandling.Ignore, which is behavior-sensitive and not covered by core DTO patterns.
- Value injection treats JsonWrapperBase as a "simple" property, enabling string->object mapping outside typical MVC model binding.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.Legacy/Misc/JsonWrapper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ValueInjectorHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Models/SystemBankStatement/SystemBankStatementIndexViewData.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.ViewData/APInvoices/APInvoiceEditQueryDTO.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.ViewModels/GLLedgerEntries/GLLedgerEntryEditViewData.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/BatchRepo.cs

Candidate solutions (core boundary, adapter shape, tests):
- Keep JsonWrapper usage in MVC adapters only; convert to typed core DTOs before invoking core services.
- Centralize serialization behavior in a legacy adapter helper (preserve AnyDecimalConverter and JSON shape).
- For .NET 9 services, bind to explicit DTOs and map from JsonWrapper in controllers/adapters to avoid implicit string conversion.
- Add characterization tests for JsonWrapper round-trip and ValueInjectHelper.ParseValue handling of JsonWrapperBase.
- Add per-screen tests to snapshot JSON payload shapes for grids and row inputs before refactor.
