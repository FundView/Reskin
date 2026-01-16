# 0041 - ContextDictionary ambient output and parameter injection

Pattern label:
- Ambient key-value output via `IFundViewReadModel.ContextDictionary` merged into subsequent requests

Service/controller:
- FundView.MVC4.UI/Controllers/BaseController.cs
- FundView.MVC4.UI/Areas/GeneralLedger/Services/GLBatchService.cs
- FundView.MVC4.UI/Areas/AccountsPayable/Services/APBatchService.cs
- FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationImportService.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemBankStatementService.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemUDDService.cs

Difference summary (vs known playbooks):
- Services write data to `context.ContextDictionary`, and BaseController injects/merges it into later service/viewmodel inputs.
- Output parameters are implicit and order-dependent, making behavior hard to test and separate from MVC orchestration.
- GL/AP playbooks favor explicit DTO outputs and controller-driven composition, not ambient per-request key-value state.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Services/GLBatchService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/AccountsPayable/Services/APBatchService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Services/GLLedgerEntryService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemBankStatementService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemUDDService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationImportService.cs

Candidate solutions (core boundary, adapter shape, tests):
- Core port: return explicit result DTOs (entity data + next-step parameters) instead of mutating ContextDictionary.
- MVC adapters orchestrate multi-step flows and pass results explicitly to viewmodels/services.
- Characterization tests before refactor: validate entityData payloads and chained-parameter behavior (Redraw flows, batch IDs).
