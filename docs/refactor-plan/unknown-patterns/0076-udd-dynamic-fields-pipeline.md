# 0076 - User Defined Data (UDD) Dynamic Field Pipeline

- Pattern label: UDD metadata-driven fields stored in `SystemUDDOption`/`SystemUDDDropDownOption` with values in `SystemEntityUDD`, dynamically rendered and persisted via `Request.Params`.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs` (UDD input generation + update pipeline using DynamicModel and request params).
- Difference summary (vs known playbooks): GL/AP playbooks assume fixed DTOs and services with typed inputs. UDD relies on runtime metadata, dynamic table access, and request-param indexing to build UI fields, search filters, and report columns across modules, making extraction and tests more complex.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemUDDController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemPropertyController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemContactsController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Models/BPProjectPermit/BPProjectPermitEditViewData.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Views/BPProjectPermit/Edit.cshtml`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Views/SystemUDD/SystemUDDEntityIndex.cshtml`
- Candidate solutions (core boundary, adapter shape, tests):
- Extract a `UserDefinedData` core service that owns metadata + value retrieval; provide EF6/EF Core adapters for `SystemUDD*` tables.
- Move request parsing and index-based value mapping into MVC adapters; pass typed UDD DTOs into core.
- Add characterization tests for UDD input generation, value persistence, and search filter behavior before refactor.
