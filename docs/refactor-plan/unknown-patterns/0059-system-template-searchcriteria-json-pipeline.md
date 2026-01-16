# 0059 - SystemTemplate search-criteria JSON pipeline (template providers + DB persistence)

Pattern label:
- SystemTemplateTable stores JSON search criteria; template providers serialize/deserialize typed criteria via reflection.

Service/controller:
- FundViewUniverse/FundViewGit/Fast.FundView.SystemTemplates.Providers/SystemTemplateProvider.cs (reflection-based JSON converter setup)
- FundViewUniverse/FundViewGit/Fast.FundView.SystemTemplates.Services/SystemTemplateHelperService.cs (template lookup + JSON (de)serialization)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/System/SystemTemplateRepo.cs (grid generation + criteria parsing)
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLTrialBalances/GLTrialBalanceSystemTemplateProvider.cs (template provider registration)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtDocketService.cs (search criteria usage)

Difference summary (vs known playbooks):
- Search criteria is persisted as JSON in `SystemTemplateTable` and interpreted at runtime to build filters, grids, and reports.
- Template providers use reflection to register converters for implicit string lists/enums, making behavior dependent on runtime type inspection.
- Tight coupling of persistence, serialization, and grid/report logic complicates core extraction and .NET 9 reuse without a dedicated template boundary.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtDocketService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtWarrantService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtViolationConsoleService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Models/GLTrialBalanceReport/GLTrialBalanceReportIndexViewData.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Models/BPDepositReport/BPDepositReportIndexViewData.cs

Candidate solutions (core boundary, adapter shape, tests):
- Introduce `ISystemTemplateSerializer` and `ISystemTemplateRepository` ports; keep JSON parsing and converter setup in adapters.
- Use typed search-criteria DTOs in core; map from JSON strings at the edge.
- Characterization tests: round-trip JSON (template -> object -> JSON), filter outputs for docket/warrant templates, and grid row stability.
