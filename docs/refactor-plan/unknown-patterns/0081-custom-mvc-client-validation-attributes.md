# 0081 - Custom MVC Client Validation Attributes + JS Adapters

- Pattern label: Custom `ValidationAttribute` implementations (RequiredIf/RangeIf/IsDateAfter/etc.) wired to jQuery Unobtrusive adapters in `custom-validation.js`, including an override of `$.validator.methods.number`.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries.UI/Attributes/ValidationAttirbutes.cs`, `FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView/custom-validation.js`.
- Difference summary (vs known playbooks): Validation rules are embedded in MVC ViewData and depend on unobtrusive adapter names/parameter conventions. Angular/.NET 9 will not honor these out of the box, so parity requires porting both server-side rules and client-side adapters (including numeric parsing changes).
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Attributes/ValidationAttirbutes.cs`
- `FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView/custom-validation.js`
- `FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Models/ReceiptingTransaction/ReceiptingTransactionIndexViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtCitation/CourtCitationEditViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Models/PermitPermitViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/AccountsPayable/Models/APVendor/APVendorEditViewData.cs`
- Candidate solutions (core boundary, adapter shape, tests):
- Define shared validation rules in core (e.g., FluentValidation) and mirror them in Angular form validators; keep MVC adapters for legacy until cutover.
- Preserve rule names/parameters when mapping to Angular to avoid behavior drift (e.g., RequiredIf target values, date comparisons, numeric parsing).
- Add characterization tests covering validation edge cases and client/server parity before refactor.
