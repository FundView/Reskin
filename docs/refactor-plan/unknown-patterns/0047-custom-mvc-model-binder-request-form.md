# 0047 - Custom MVC model binder reads Request.Form for multi-select

Pattern label:
- Custom `DefaultModelBinder` override with Request.Form access and naming conventions

Service/controller:
- FundView.MVC4.UI/Binder/FundViewModelBinder.cs (SetProperty override)
- FundView.MVC4.UI/Global.asax.cs (registers as default binder)

Difference summary (vs known playbooks):
- Legacy MVC uses a global custom model binder that:
  - Converts empty strings to null.
  - Converts enum "0" to null for dropdown default values.
  - For multi-select fields (name ends with `IDARRAY` or `AdditionalMetadataAttribute` SelectType=Multiple), bypasses MVC binder and reads raw `HttpContext.Current.Request.Form` using `"{TypeName}.{PropertyName}"` key.
- This coupling to Request.Form + naming conventions is not standard and must be preserved in .NET 9 or adapter logic.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Binder/FundViewModelBinder.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Global.asax.cs

Candidate solutions (core boundary, adapter shape, tests):
- Keep the legacy binder in MVC; add a matching custom binder or input-normalization layer in .NET 9 to preserve multi-select semantics.
- Introduce an `IRequestValueNormalizer` used by controllers/services to emulate the same empty-string/enum/multi-select rules.
- Characterization tests for posted form values (single vs multi-select) to lock behavior before extracting.
