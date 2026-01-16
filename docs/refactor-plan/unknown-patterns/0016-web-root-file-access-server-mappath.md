# 0016 - Web Root File Access via Server.MapPath

- Pattern label: MVC controllers/services read files from web root using `Server.MapPath` or `HttpContext.ApplicationInstance.Server.MapPath`.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtViolationController.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects file IO to be isolated behind adapters. Direct `Server.MapPath` usage ties logic to MVC hosting and file layout, complicating .NET 9 reuse and testing.
- Search results (other occurrences, paths): 5 active `MapPath` uses in MVC4 UI controllers/services; commented occurrences excluded.
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce an `IWebRootFileProvider`/`IFileStorage` adapter for template and attachment access.
- Move web-root path resolution to MVC adapters or composition root; pass file streams/contents into core services.
- Add characterization tests that capture expected template reads and attachment path behavior before refactor.

Subpatterns
- Web-root config reads (XML config in services).
- Report/template reads (DocX, embedded reports).
- Attachment path resolution (MVC controller storage).

Subpattern: Web-root config reads
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Services/FastGovPayAPIService.cs`

Subpattern: Report/template reads
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtViolationController.cs`
- `FundViewGit/FundView.MVC4.UI/Global.asax.cs`

Subpattern: Attachment path resolution
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtViolationController.cs`
