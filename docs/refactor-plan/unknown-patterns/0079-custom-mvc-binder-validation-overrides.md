# 0079 - Custom MVC Binder/Validation Overrides

- Pattern label: Global MVC model binders override primitive parsing (decimal currency, Guid parsing) and disable implicit required value-type validation.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Binder/DecimalModelBinder.cs`, `FundViewGit/FundView.MVC4.UI/Binder/GuidModelBinder.cs`, `FundViewGit/FundView.MVC4.UI/Global.asax.cs` (ModelBinders + AddImplicitRequiredAttributeForValueTypes = false).
- Difference summary (vs known playbooks): GL/AP playbooks assume default binding/validation. Legacy MVC uses custom binders that parse currency with CurrentCulture and suppress implicit required for value types, changing model state and null handling. .NET 9 APIs will parse/validate differently unless these overrides are replicated.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Global.asax.cs`
- `FundViewGit/FundView.MVC4.UI/Binder/DecimalModelBinder.cs`
- `FundViewGit/FundView.MVC4.UI/Binder/GuidModelBinder.cs`
- Candidate solutions (core boundary, adapter shape, tests):
- Keep MVC binders for legacy; add equivalent input normalization in the .NET 9 API boundary (custom binders or minimal API model binders).
- Add regression tests around decimal currency parsing and nullable value-type validation behavior (implicit required off).
- Document binder/validation rules in the reskin playbook to enforce parity across MVC + new services.
