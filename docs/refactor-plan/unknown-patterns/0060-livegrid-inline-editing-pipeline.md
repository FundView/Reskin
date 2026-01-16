# 0060 - LiveGrid inline editing pipeline (HTML inputs + data-updated AJAX)

Pattern label:
- LiveGrid inline-editing pipeline: LegacyLiveGridInput + LiveGridBuilder HTML + GridHelper.BuildLiveGridJsonData responses.

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/GridHelper.cs (BuildLiveGridJsonData + DataFv helpers)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/LegacyLiveGridBuilder.cs (DataUpdated query string + HTML wrapper)
- FundViewUniverse/FundViewGit/FastScreenBuilder/LiveGrids/LiveGridBuilder.cs (input/select HTML with data-updated)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemBankStatementJournalEntryService.cs (live grid row inputs)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtViolationController.cs (live grid JSON responses)

Difference summary (vs known playbooks):
- LiveGrid embeds editable HTML inputs in grid rows with `data-updated` actions serialized via `DataFv`, and posts JSON updates through query-string-encoded `ItemUpdating`.
- UI concerns (HTML, data-fv routing, update payloads) are created inside services/controllers, making core extraction and .NET 9 reuse difficult without a dedicated boundary.
- The response shape (`GridHelper.BuildLiveGridJsonData`) is a bespoke JSON contract separate from DataTables.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPInspectionControllerController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CodeEnforcement/Controllers/CECaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemPropertyController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Services/GLLedgerEntryService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Models/GLLedgerEntry/GLLedgerEntryEditViewData.cs

Candidate solutions (core boundary, adapter shape, tests):
- Introduce a LiveGrid core port for row update commands and validation; keep HTML input generation and data-fv wiring in MVC adapters.
- Replace query-string `ItemUpdating` payloads with typed DTOs at the edge; map to core commands.
- Characterization tests: live grid JSON response shape, HTML input rendering, and update flow per module before refactor.
