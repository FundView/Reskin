# 0058 - Legacy CSV/XLSX import/export pipeline (stringly-typed rows + file staging)

Pattern label:
- Legacy CSV/XLSX ingestion and export: CSVHelper/CsvHelper parse files into header-based dictionaries, often with file staging on disk.

Service/controller:
- FundViewUniverse/FundViewGit/Fast.FundView.Legacy.Helpers/LegacyCsvHelper.cs (CsvHelper-based parsing, CSV/XLSX switch)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/CSVHelper.cs (custom CSV parser + file helpers)
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLYearEndAccountBalance/GLYearEndAccountBalanceService.cs (CSV/XLSX import + attachment staging)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemBankStatementService.cs (CSV upload handling)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/ReceiptingAPI/MunicipayFileParser.cs (CSV read + file copy to attachments)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtUtilityRepo.cs (CSV mapping files in SMT exports)
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APForm1099CsvExporter.cs (CSV export)

Difference summary (vs known playbooks):
- CSV/XLSX parsing produces stringly-typed dictionaries based on header rows and is tightly coupled to file system paths, with mixed responsibilities (upload, staging, parsing, mapping, and persistence).
- Core extraction expects typed commands and abstractions for file storage and parsing; these pipelines intertwine IO, parsing, and domain mapping in MVC/services/helpers.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLBatches/GLBatchService.cs (CSV import)
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APVendorImportService.cs (CSV/XLSX import)
- FundViewUniverse/FundViewGit/Fast.FundView.Banking.Services/BankReconciliations/UploadedBankFileItemImporter.cs (CSV import)
- FundViewUniverse/FundViewGit/Fast.FundView.SystemLedger.Services/SystemLedgerAccountReMapperFactory.cs (CSV logging/export)
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLAccounts/GLAccountImportHelperService.cs (CSV import)

Candidate solutions (core boundary, adapter shape, tests):
- Define `IImportFileReader` + `IFileStorage` ports; keep CSV/XLSX parsing and disk staging in adapters, pass typed rows to core.
- Normalize CSV parsing behavior in one adapter (CsvHelper or custom parser), wrap legacy quirks in tests.
- Characterization tests: parse CSV/XLSX headers, delimiter/quote handling, and row mapping to domain commands before refactor.
