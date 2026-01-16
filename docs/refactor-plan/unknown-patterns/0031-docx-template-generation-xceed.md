# 0031 - DocX template generation/merge via Xceed

Pattern label:
- DocX template generation/merge via Xceed (Xceed.Words.NET)

Service/controller:
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemActionSetService.cs
- FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtWarrantService.cs
- FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtQuarterlyReportController.cs
- FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPInspectionConsoleController.cs
- Fast.FundView.SystemSecureSignatures.Services/SystemDocumentSignatureRequestService.cs

Difference summary (vs known playbooks):
- Controllers/services load and manipulate DocX templates directly (merge, replace tags, build tables) using Xceed.Words.NET.
- Document generation is mixed with request handling, filesystem access, and signature insertion; not isolated behind a core port.
- Output must remain byte-for-byte compatible for 1:1 reskin; conversion to PDF is tied to Word interop elsewhere (see 0022).

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APDocumentGeneratorService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CodeEnforcement/Controllers/CETaskConsoleController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtCitationReportController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/DocxConvert.cs
- rg -n "DocX\\.Load|DocX\\.Create" FundViewUniverse/FundViewGit

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IDocxTemplateRenderer (LoadTemplate, ReplaceTokens, Merge, RenderToStream) with legacy adapter using Xceed.
- Keep template file paths and filesystem IO in MVC adapters; pass template bytes into core.
- Characterization tests before refactor: template token replacement, merged document page count, and output size/hash for representative templates.
