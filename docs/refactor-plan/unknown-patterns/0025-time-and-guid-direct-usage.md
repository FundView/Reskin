# 0025 - Direct Time/Guid Usage (No Abstraction)

- Pattern label: Services/controllers/helpers use `DateTime.Now`/`UtcNow` and `Guid.NewGuid()` directly inside business logic and filenames.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtOCAGroupController.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects time/identity to be injectable for deterministic tests. Direct time/Guid calls make behavior nondeterministic and complicate characterization tests and core extraction.
- Search results (other occurrences, paths): widespread across MVC4 UI and Fast.* services (reports, batch processing, file naming, and DTO defaults).
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce `IClock` and `IGuidProvider` interfaces; keep direct usage in legacy adapters where needed.
- Replace inline time/Guid use in core with injected providers for deterministic tests.
- Add characterization tests around time-sensitive workflows (report filenames, status timestamps, due date calculations).

Search results list (active)
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtOCAGroupController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/AccountsPayable/Models/APPayment/APPaymentPrintChecksViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemBankStatementService.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/SQLHelper.cs`
- `FundViewGit/Fast.FundView.AccountsPayable.Services/APPaymentService.cs`
