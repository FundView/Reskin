# 0010 - Hard-coded File System Paths and Direct IO

- Pattern label: Services/helpers/validators embed absolute or UNC file paths (e.g., `C:\Documents`, `C:\Centralize`, `\\enterprise\...`) and access the file system directly.
- Service/controller: `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects environment-specific concerns (file storage, paths, and IO) to be isolated behind adapters. These paths are hard-coded in MVC and EFModel layers, making portability and .NET 9 reuse harder without path abstraction.
- Search results (other occurrences, paths): see subpattern lists below (absolute and UNC path roots only; migrations and commented-only lines excluded).
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce `IFileStorage`/`IPathProvider` and move absolute path selection to adapters or config.
- Pass streams/DTOs into core services; keep file IO in MVC adapters.
- Add characterization tests around document/report generation that assert file outputs and file-not-found behavior before refactor.

Subpatterns
- C:\Documents storage (documents, validation error exports, and attachment inputs).
- C:\Centralize artifacts (help docs, SMT exports, court reports).
- C:\Attachment exports (print/attachment output folders).
- UNC shares (\\enterprise, \\10.93.*).
- Other local absolute paths (c:\docx, c:\users, c:\test, etc).
- Note: some files participate in multiple subpatterns (e.g., `PrintDocumentHelper`, `CourtUtilityService`).

Subpattern: C:\Documents storage
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtJuryPoolController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtWarrantService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/PurchaseOrders/Services/PurchaseOrdersDemoService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Models/SystemThirdPartySetup/SystemThirdPartySetupIndexViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemBankStatementService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/BPContractorPrint.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtUtilityRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtCitationValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtFeeTransactionValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtPersonValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtTransactionValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtVehicleValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtViolationFeeValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtViolationValidator.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/DocxConvert.cs`

Subpattern: C:\Centralize artifacts
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/Dashboard/Models/Main/MainDashboardIndexViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtChildSafetySeatBeltReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtCouncilReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtOCAGroupController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtQuarterlyReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtUtility/CourtUtilityMapToBaseWorksheetsViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtUtility/CourtUtilityMapToCourtGeneralSetupViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtUtility/CourtUtilityMapToCourtSpeedingWorkSheetsViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtUtility/CourtUtilityMapToCourtViolationCodeViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtUtility/CourtUtilityMapToCourtViolationStatusViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtUtility/CourtUtilityMapToSystemGeneralSetupViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtUtilityService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtWarrantReportService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/OCAAppointmentAndFeesReportService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemHelpController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/ConversionExtension.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtUtilityRepo.cs`

Subpattern: C:\Attachment exports
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPInspectionConsoleController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/CodeEnforcement/Controllers/CETaskConsoleController.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs`

Subpattern: UNC shares (\\enterprise, \\10.93.*)
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtUtilityService.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs`
- `FundViewGit/FundView.MVC4.UI/CustomSelectListsFromAPI.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs`

Subpattern: Other local absolute paths
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPPermitReceiptingAPIController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtUtilityService.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/ConversionExtension.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemHelpService.cs`
- `FundViewGit/FundView.MVC4.UI/Core2/DataHelpers/Business/CourtViolationFeeTransactionHelper.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtCitationRepo.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/System/SystemHelpRepoService.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/FileHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ConsoleHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/GetAllKeys.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/LoggingHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Utilities/WebRequest.cs`
- `FundViewGit/FundView.MVC4.UI/ReceiptingAPI/Reporting/ReceiptRegister.cs`
- `FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs`
