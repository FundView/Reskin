# 0069 - FVFile base64 file response format (filename//contentType\base64)

Pattern label:
- Custom file download payload encoded as a string, parsed client-side and saved via `saveAs`.

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/FileHelper.cs (FVFile formatting)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Extensions/LegacyReportResultExtensions.cs (ToFVFile)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView.js (file response parsing and download)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs (returns FVFile)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtOCAGroupController.cs (returns FVFile)

Difference summary (vs known playbooks):
- Files are returned as a string payload (`filename//contentType\base64`) instead of standard HTTP file responses.
- Client-side JS parses the payload and uses `b64toBlob` + `saveAs` to trigger downloads.
- The format is tightly coupled to legacy FVLoad `data-fv` workflow and must remain byte-for-byte compatible during reskin.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/FileHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Extensions/LegacyReportResultExtensions.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView.js
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingTransactionService.cs

Candidate solutions (core boundary, adapter shape, tests):
- Keep `FVFile` encoding in legacy adapters; expose a typed `LegacyFileResult` in core with name, content type, and stream.
- In FundViewClient, implement a decoding adapter for the legacy string format or negotiate a modern download API per module.
- Characterization tests: file name/content type parsing, base64 decode equality, and download triggers for key reports.
