# 0056 - Dynamic SelectList/Select2 pipeline (config JSON + SQL builder + reflection)

Pattern label:
- SelectList/Select2 pipeline: JSON config -> dynamic SQL builder -> reflection dispatch for custom lists

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs (BatchSelectList, GetSelectListData, BuildSelectListFromTable/Enum)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/SelectListConfigJson.cs (config payload)
- FundViewUniverse/FundViewGit/Fast.FundView.Legacy.Helpers/SelectListSqlBuilder.cs (dynamic SQL builder with table/column validation)

Difference summary (vs known playbooks):
- Builds select-list queries from JSON config (table/column names, dependent filters, search text) and returns Select2 JSON.
- Uses reflection to invoke custom select list methods based on URL strings.
- Dynamic SQL generation with parameterization + table/column validation; behavior depends on string inputs and table metadata.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/SelectListConfigJson.cs
- FundViewUniverse/FundViewGit/FastSoftware.Core/SelectListConfiguration.cs
- FundViewUniverse/FundViewGit/Fast.FundView.Legacy.Helpers/SelectListSqlBuilder.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs (SelectListSqlBuilderFactory wiring)

Candidate solutions (core boundary, adapter shape, tests):
- Introduce a core SelectList query port taking a typed config object; keep reflection-based custom lists in MVC adapters.
- Preserve SQL validation rules (table/column allow-list) in a shared component.
- Characterization tests: Select2 search results, dependent filters, ordering, and default value behavior.
