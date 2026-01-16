# 0075 - Service Layer Reads Request.Params/Files (Implicit Request Binding)

- Pattern label: MVC service methods read `HttpContext.Current.Request.Params` and `Request.Files` directly, bypassing view models and controller binding.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemAttachmentService.cs` (Request.Params + Request.Files used inside service methods).
- Difference summary (vs known playbooks): GL/AP playbooks assume controllers or MVC adapters bind inputs and pass typed DTOs to core services. Here, services themselves reach into the request to parse parameters and uploaded files, making extraction and testability harder and coupling service logic to the web runtime.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemAttachmentService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemSecureSignatureService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationImportService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Services/GLYearEndAccountBalance/GLYearEndAccountBalanceService.cs`
- Candidate solutions (core boundary, adapter shape, tests):
- Move request parsing to MVC adapters/controllers; pass typed DTOs and streams into core services.
- Introduce upload/parameter DTOs and adapter mappers to isolate HttpContext usage to legacy web layer.
- Add characterization tests for request parameter parsing and file upload behavior before refactor.
