# 0051 - Telerik Reporting report pipeline and designer assets

Pattern label:
- Telerik Reporting report rendering (ReportBook/ReportProcessor + .Designer.cs/.resx report definitions)

Service/controller:
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.TelerikReports.Services/APBatchRegisterService.cs (ReportBook + parameters + PDF output)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Services/BPFeeReportService.cs (dynamic Telerik table column sizing)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingBatchService.cs (crosstab data sources)
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.TelerikReports/GLBudgetReportExpenseByDepartment.Designer.cs (auto-generated report definition)
- FundViewUniverse/FundViewGit/Fast.FundView.BuildingPermits.TelerikReports/BPFeeReport.Designer.cs (auto-generated report definition)

Difference summary (vs known playbooks):
- Report logic lives in Telerik designer assets (.Designer.cs/.resx) plus runtime code that mutates report layout (tables, crosstabs, widths).
- Rendering relies on Telerik.Reporting runtime and ReportBook composition; this is not a simple service extraction.
- Outputs are layout-sensitive PDFs; exact formatting and pagination are part of the behavior that must not regress.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.TelerikReports.Services/APBatchRegisterService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.TelerikReports.Services/APPaymentsReportService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.TelerikReports.Services/APOpenInvoicesReportService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/BuildingPermit/Services/BPFeeReportService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CashReceipting/Services/ReceiptingBatchService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.*.TelerikReports/*.Designer.cs (report definitions)

Candidate solutions (core boundary, adapter shape, tests):
- Keep Telerik report definitions and rendering in adapter projects; extract data shaping into core query services.
- Introduce a report adapter interface (e.g., `IReportRenderer`) that takes report key, datasets, and parameters; bind in MVC and .NET 9 implementations.
- Preserve runtime layout mutations (column resizing, crosstab data sources) in adapter code.
- Characterization tests: generate PDFs from fixed data and compare snapshots (page count, totals, key text) to catch layout regressions.
