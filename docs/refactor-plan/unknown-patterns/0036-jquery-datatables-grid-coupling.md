# 0036 - jQuery DataTables param parsing and grid serialization in repos/services

Pattern label:
- jQuery DataTables param parsing and grid serialization (request.param -> jQueryDataTableParamModel -> SerializeGrid)

Service/controller:
- FundView.MVC4.UI/EFModel/Repository/BatchRepo.cs
- FundView.MVC4.UI/EFModel/Repository/BuildingPermit/BPPermitRepo.cs
- FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtViolationRepo.cs
- FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationService.cs
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundView.MVC4.UI/Libraries.UI/Helpers/GridHelper.cs
- Fast.FundView.Legacy/JQueryDataTableParamModel.cs

Difference summary (vs known playbooks):
- Repositories/services deserialize jQuery DataTables request params (paging/sort/sEcho) and return DataTables JSON via SerializeGrid.
- UI grid contracts and JSON shape are embedded in data access layers, coupling MVC specifics to core behavior.
- GL/AP playbooks favor core DTOs (TableQueryResult) with adapters handling UI-specific serialization.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/BatchRepo.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/BuildingPermit/BPPermitRepo.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/BuildingPermit/BPFeeRepo.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtViolationRepo.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/MunicipalCourt/CourtTransactionRepo.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/System/SystemPropertyRepo.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/System/SystemTemplateRepo.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingTransactionService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationService.cs

Candidate solutions (core boundary, adapter shape, tests):
- Core port: TableQuery (paging/sort/filter) + TableQueryResult rows/metadata; keep jQuery param parsing in MVC adapters.
- Adapter layer maps jQueryDataTableParamModel to core queries and renders DataTables JSON via SerializeGrid.
- Characterization tests before refactor: order/sort paging, sEcho passthrough, and JSON shape/column alignment parity.
