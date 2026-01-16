# 0034 - XML import/export parsing with XmlSerializer/XmlDocument

Pattern label:
- XML import/export parsing with XmlSerializer/XmlDocument (vendor citation pipelines, OCA exports)

Service/controller:
- FastSoftware.FundView.CitationImport/CitationImportXmlFileManager.cs
- FastSoftware.Import/XmlHelper.cs
- FastSoftware.FundView.CitationImport/Vendor/TicketWriterIdExtractor.cs
- FundView.MVC4.UI/Areas/MunicipalCourt/Services/OCAAppointmentAndFeesReportService.cs
- CopSync/CopSync.cs

Difference summary (vs known playbooks):
- XML parsing/serialization logic is embedded in import/export flows, including manual namespace rewrites and file-based processing.
- Mixes data shaping, file IO, and schema handling inside services/helpers, rather than via a core port.
- Output schemas/encodings must remain identical during 1:1 reskin; vendor-specific quirks must be preserved.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FastSoftware.FundView.CitationImport/CitationImportXmlFileManager.cs
- FundViewUniverse/FundViewGit/FastSoftware.Import/XmlHelper.cs
- FundViewUniverse/FundViewGit/FastSoftware.FundView.CitationImport/Vendor/TicketWriterIdExtractor.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/OCAAppointmentAndFeesReportService.cs
- FundViewUniverse/FundViewGit/CopSync/CopSync.cs

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IXmlImportParser/IXmlExportWriter with explicit schema/version handling; adapters handle vendor-specific namespace rewrites and file IO.
- Keep file-system paths and vendor configs in MVC/legacy adapters; pass raw XML strings or streams to core.
- Characterization tests before refactor: schema/namespace output, deserialization of known vendor samples, and invalid-file routing behavior.
