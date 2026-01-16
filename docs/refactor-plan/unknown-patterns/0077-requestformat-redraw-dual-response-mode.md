# 0077 - RequestFormat/Redraw Dual-Response Mode

- Pattern label: Request flags (`RequestFormat=EntityData`, `Redraw=true`, `data-redraw-only`) toggle between server-rendered HTML and entityData responses, with redraw requests merging prior entityData into new query params.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs` (RequestFormat/Redraw handling + RenderPartialViewOrEntityData).
- Difference summary (vs known playbooks): GL/AP playbooks assume explicit DTO responses. This flow relies on query-string flags and response chaining to decide HTML vs entityData, coupling client reload behavior to MVC controller logic and making it hard to express in .NET 9 APIs/Angular without a compatibility layer.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs`
- `FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView.js` (adds RequestFormat/Redraw on reload)
- `FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView/fundview-ui.js` (RequestFormat/Redraw URL toggles)
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Extensions/HtmlHelperExtensions.cs` (`data_redraw_only`)
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Views/BPFeeScheduleDescription/FeeRatePreview.cshtml`
- `FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Views/ReceiptingTransaction/Index.cshtml`
- Candidate solutions (core boundary, adapter shape, tests):
- Keep RequestFormat/Redraw handling in MVC adapters; map to explicit response DTOs in core services.
- For Angular, define explicit endpoints and avoid query-flag switching; provide legacy compatibility adapters during dual-run.
- Add characterization tests around redraw reloads and entityData merge behavior before refactor.
