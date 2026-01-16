# 0062 - Conversion tooling and vendor schema mapping (CSV/XLSX caches + validation outputs)

Pattern label:
- Conversion tooling for vendor schema mapping, validation, and CSV/XLSX preprocessing.

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/ConversionExtension.cs (conversion caches, CSV/XLSX parsing, validation utilities)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ConversionHelper.cs (reflection-based validation + CSV output)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtUtilityService.cs (conversion file paths + vendor schema lookups)

Difference summary (vs known playbooks):
- Uses static in-memory caches (`vendorDataSet`, `xlsxDocuments`) for CSV/XLSX data and relies on manual file system paths (local + network share).
- Conversion validation writes CSVs to disk and uses reflection/expression parsing for rule evaluation, outside typical service/core boundaries.
- Mapping is driven by conversion schema tables (ConversionVendor*/ConversionSchema*) with dynamic dictionary operations and custom grouping logic.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Views/SystemUtility/ConversionDashboard.cshtml
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Models/SystemUtility/SystemUtilityConversionDashboardViewData.cs
- FundViewUniverse/FundViewGit/Fast.FundView.Conversion.Entities/ConversionSchemaDataTable.cs
- FundViewUniverse/FundViewGit/FundView.Enums/System/EnumConversionDataStandard.cs

Candidate solutions (core boundary, adapter shape, tests):
- Introduce a conversion core module that owns schema mapping and validation logic; keep CSV/XLSX IO and file paths in adapters.
- Replace static caches with scoped cache services; inject file path resolver for network/local paths.
- Preserve current CSV output formats with characterization tests using fixture datasets before refactor.
- Create adapter ports for conversion schema access (ConversionVendor*/ConversionSchema*) and keep EF6/EF Core split behind interfaces.
