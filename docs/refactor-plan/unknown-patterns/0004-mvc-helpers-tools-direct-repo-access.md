# 0004 - MVC Helpers/Tools with Direct Repo Access (Bypass Service Shims)

- Pattern label: MVC `Libraries/Helpers` and `Areas/*/Tools` classes call `*Repo` static methods directly.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/MunicipalCourtHelper.cs`.
- Difference summary (vs known playbooks): GL/AP playbooks expect MVC shims to delegate to helper services, but helpers/tools bypass that layer, mixing UI helper logic with data access and domain rules. These helpers are often shared across controllers and services, which makes extraction and reuse harder without a dedicated core boundary.
- Search results (other occurrences, paths): 2 helper files and 1 tool file found in FundViewGit.
- Candidate solutions (core boundary, adapter shape, tests):
- Move domain logic in helpers/tools into core services; keep UI formatting and HttpContext interactions in MVC adapter helpers.
- Replace direct repo calls with injected query services and adapters; keep helpers thin wrappers.
- Add characterization tests around helper outputs (strings, grids, computed values) before refactor.

Search results list (Helpers)
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/CitationImportLockService.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/MunicipalCourtHelper.cs`

Search results list (Tools)
- `FundViewGit/FundView.MVC4.UI/Areas/AccountsPayable/Tools/APInvoiceCreator.cs`
