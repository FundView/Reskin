# 0045 - Reflection-based value injection (ValueInjectHelper/InjectFrom)

Pattern label:
- Reflection-based property/field injection via `ValueInjectHelper.InjectFrom` / `InjectFrom`

Service/controller:
- FundView.MVC4.UI/Libraries/Helpers/ValueInjectorHelper.cs (reflection mapping + type conversion)
- FundView.MVC4.UI/Controllers/BaseController.cs (URL query dictionary + ContextDictionary injection)
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs (request/document merging)
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemNoteService.cs (request dictionary injection)
- FundView.MVC4.UI/EFModel/Repository/CashReceipting/ReceiptingTransactionItemRepository.cs (request dictionary -> search request)
- FundView.MVC4.UI/Libraries/Helpers/MunicipalCourtHelper.cs (entity/data merging)

Difference summary (vs known playbooks):
- Legacy MVC uses a custom reflection injector to copy values across objects and from string/object dictionaries using `InjectorPropertyAttribute` and ad-hoc conversion rules (enum/TimeSpan/DateTime/decimal/string lists).
- Mapping logic is implicit and spread across controllers/services/repos, which makes core extraction risky without preserving conversion semantics.
- The helper blurs boundaries between request parsing, model binding, and domain updates, unlike explicit GL/AP mappings.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ValueInjectorHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemNoteService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/CashReceipting/ReceiptingTransactionItemRepository.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/MunicipalCourtHelper.cs

Candidate solutions (core boundary, adapter shape, tests):
- Keep `ValueInjectHelper` in legacy adapters; introduce an `IValueInjector` port so .NET 9 adapters can reuse the same conversion logic.
- Add characterization tests around representative injections (e.g., SystemDocument, SystemNote, MunicipalCourt) to lock enum/DateTime/decimal/TimeSpan conversions before extraction.
- Gradually replace reflection injection with explicit mappings in core while maintaining the same conversion rules in a shared utility.
