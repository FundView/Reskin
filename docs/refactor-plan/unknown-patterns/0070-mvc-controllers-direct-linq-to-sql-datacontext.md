# 0070 - MVC Controllers Direct LINQ-to-SQL DataContext Access

- Pattern label: MVC controllers instantiate `FundViewDataContext` via `FundViewDataContextFactory.Create()` and run LINQ-to-SQL queries/updates directly.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPFeeControllerController.cs`.
- Difference summary (vs known playbooks): GL/AP playbooks expect MVC shims to delegate to core services, but these controllers perform data access and business rules in the MVC layer using the CodeSmith LINQ-to-SQL context. This bypasses service boundaries, couples UI to persistence, and complicates dual-target core extraction.
- Search results (other occurrences, paths): found across base and area controllers in FundViewGit.
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce core services/use cases and move LINQ-to-SQL queries into legacy adapters; keep `FundViewDataContext` usage behind interfaces.
- For .NET 9 services, translate LINQ queries into EF Core equivalents or dedicated read models while keeping legacy adapters for MVC.
- Add characterization tests on controller outputs (grid JSON, view data) before refactors.

Search results list (controllers)
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/AuditingController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPFeeControllerController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPStepControllerController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPInspectionConsoleController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Controllers/BPProjectValuationReportController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtBondAdjustmentController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtCitationController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Controllers/CourtChildSafetySeatBeltReportController.cs`
