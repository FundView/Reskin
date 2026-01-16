# 0005 - MVC Controllers with Direct Repo Access (Report Workflows)

- Pattern label: MVC controllers call `*Repo` static methods directly (often report-focused).
- Service/controller: `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Controllers/FastGovPayMunicipalCourtController.cs`.
- Difference summary (vs known playbooks): GL/AP playbooks expect MVC controllers to delegate to services/helper services; here controllers perform repo queries directly (and sometimes sync-block async calls), blending orchestration, data access, and report assembly.
- Search results (other occurrences, paths): 10 controller files in FundViewGit.
- Candidate solutions (core boundary, adapter shape, tests):
- Move repo calls into helper/query services in `Fast.FundView.<Module>.Services` and keep controllers thin shims.
- For report workflows, create a report data service in core, with EF6 adapters for legacy and EF Core adapters for new services.
- Add characterization tests for report outputs and controller responses before refactor.

Search results list
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPPermitReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPProjectValuationReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/CodeEnforcement/Controllers/CECaseReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/CodeEnforcement/Controllers/CETaskReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Controllers/FastGovPayBuildingPermitController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Controllers/FastGovPayMunicipalCourtController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtChildSafetySeatBeltReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtCouncilReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtOCAGroupController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtQuarterlyReportController.cs`
