# 0008 - Service Layer HttpContext.Current Dependencies

- Pattern label: Services/helpers read `HttpContext.Current` for request params, files, OWIN services, or per-request storage.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs`.
- Difference summary (vs known playbooks): core extraction expects explicit inputs (DTOs, commands, files) passed into services. These services and helpers reach into `HttpContext.Current`, making request data and storage implicit, tying logic to MVC and making reuse in .NET 9 harder.
- Search results (other occurrences, paths): 30 non-view files with active `HttpContext.Current` usage across services, helpers, EFModel, binders, and controllers.

Subpatterns
- File uploads via `HttpContext.Current.Request.Files`.
- Request params via `Request.Params`, `Request.Form`, `Request.QueryString`, or `Request[...]` in non-UI code.
- Request metadata via `HttpContext.Current.User`, `Server`, `RequestContext`, or `UserHostAddress` in non-UI code.
- UI helpers and MVC plumbing using `HttpContext.Current` (helpers, extensions, model binder).
- OWIN service provider resolution via `HttpContext.Current.GetOwinContext()`.
- FundViewReadModel ambient storage in `HttpContext.Current.Items`.
- Note: some files participate in multiple subpatterns (e.g., `BaseController`, `FundViewReadModel`).

Candidate solutions (core boundary, adapter shape, tests)
- Introduce an `IRequestContext`/`ILegacyRequestContext` abstraction for request params, files, and user identity; populate it from MVC adapters only.
- Move request parsing and file upload handling to controllers; pass typed inputs to core services.
- Replace `HttpContext.Current` storage (e.g., `FundViewReadModel`) with scoped DI or an explicit context provider; keep any remaining HttpContext use in MVC adapters only.
- Add characterization tests around request-bound flows before refactor (file uploads, param parsing, and OWIN service resolution).

Subpattern: File uploads
- Notes: upload handling is embedded in services/helpers instead of controllers; most also read request params.
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Services/GLBatchService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Services/GLBudgetAdjustmentService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Services/GLWorkingBudgetImportExportService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Services/GLYearEndAccountBalance/GLYearEndAccountBalanceService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationImportService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemAttachmentService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemBankStatementItemService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemBankStatementService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemHelpService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemSecureSignatureService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemThirdPartySetupService.cs`

Subpattern: Request params (non-upload, non-UI)
- Notes: param-only usage is limited; most param reads are co-located with file uploads above.
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs`
- `FundViewGit/FundView.MVC4.UI/Core2/Extensions/CourtGroup.cs`

Subpattern: Request metadata (non-UI)
- Notes: request/user metadata is read outside controllers; `FVDataContext.cs` also reads user/host metadata (listed above).
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Services/FastGovPayAPIService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtViolationCode/CourtViolationCodeEditCourtViolationCodeMainTabViewData.cs`

Subpattern: UI helpers and MVC plumbing
- Notes: UI helpers and binders read request state via `HttpContext.Current`.
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Extensions/HtmlHelperExtensions.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/BindHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/FundViewResult.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/MenuHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Binder/FundViewModelBinder.cs`

Subpattern: OWIN service provider resolution
- Notes: service provider is pulled from OWIN context inside MVC controllers/repos.
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPPermitReceiptingAPIController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtOCAGroupController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtQuarterlyReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtOCAGroupRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Models/FundViewReadModel.cs`

Subpattern: FundViewReadModel ambient storage
- Notes: `FundViewReadModel` is stored and retrieved from `HttpContext.Current.Items`.
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/EFModel/Models/FundViewReadModel.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/FundViewReadModelProvider.cs`
- `FundViewGit/FundView.MVC4.UI/Global.asax.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs`
