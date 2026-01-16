# 0035 - XLSX report generation and PDF conversion via Fast.Common.Spreadsheets

Pattern label:
- XLSX report generation and PDF conversion via Fast.Common.Spreadsheets (IConvertXlsxToPdf, TelerikXlsxToPdfConverter)

Service/controller:
- Fast.FundView.AccountsPayable.Services/APInvoiceExportService.cs
- Fast.FundView.AccountsPayable.Services/APTemplateService.cs
- Fast.FundView.GeneralLedger.Services/GLLedgerEntries/GLJournalEntryReportService.cs
- Fast.FundView.CashReceipting.Reporting.Services/TransactionReport/TransactionReport.cs
- Fast.FundView.Banking.Services/BankReconciliationReportHelper.cs
- FundView.MVC4.UI/Libraries/Helpers/XlsxConvert.cs
- Fast.FundView.GeneralLedger.Api/Startup.cs
- FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs

Difference summary (vs known playbooks):
- Report services/controllers build ExcelPackage outputs and immediately convert to XLSX/PDF streams using IConvertXlsxToPdf, coupling export logic to Telerik/OfficeOpenXml details.
- Export format branching (PDF vs XLSX), layout/format decisions, and conversion steps are embedded in services instead of an adapter boundary.
- Output artifacts are binary files that must remain byte-for-byte stable across legacy MVC and new .NET 9 services.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.InvoiceSelection.Controllers/APPayablesProcessSelectionController.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APPaymentRankingReportHelperService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APOpenItemsReportService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APInvoiceReviewReportHelperService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APInvoiceExportService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APVendorMaintenanceGridReportHelperService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Services/APTemplateService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.CashReceipting.Reporting.Services/TransactionReport/TransactionReport.cs
- FundViewUniverse/FundViewGit/Fast.FundView.CashReceipting.Reporting.Services/TransactionReport/TransactionReportFactory.cs
- FundViewUniverse/FundViewGit/Fast.FundView.CashReceipting.Reporting.Services/PostingRegister/PostingRegister.cs
- FundViewUniverse/FundViewGit/Fast.FundView.CashReceipting.Reporting.Services/PostingRegister/PostingRegisterFactory.cs
- FundViewUniverse/FundViewGit/Fast.FundView.CashReceipting.Reporting.Services/PaymentReport/PaymentReport.cs
- FundViewUniverse/FundViewGit/Fast.FundView.CashReceipting.Reporting.Services/PaymentReport/PaymentReportFactory.cs
- FundViewUniverse/FundViewGit/Fast.FundView.Banking.Services/BankReconciliationReportHelper.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLWorkingBudgets/GLWorkingBudgetService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLWorkingBudgets/GLWorkingBudgetRegisterService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLChartOfAccounts/GLChartOfAccountsMappingService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLLedgerEntries/GLJournalEntryReportService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLBudgetManagements/GLBudgetManagerBudgetReportService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLBudgetAdjustments/GLBudgetAdjustmentRegisterService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services/GLFinancials/GLFinancialsReportService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Api/Controllers/GLTransactions/GLTransactionManageIndexController.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Api/Controllers/APBatches/APPayablesProcessPayInvoicesController.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Api/Startup.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/XlsxConvert.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/XLSXHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs
- FundViewUniverse/FundViewGit/Fast.FundView.Legacy.Helpers/LegacyXlsxReportHelper.cs
- FundViewUniverse/FundViewGit/Fast.FundView.Legacy/Interfaces/ILegacyXlsxReportHelper.cs

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IReportExportWriter or IXlsxReportRenderer that accepts a report model and returns a stream; Telerik/EPPlus conversion stays in adapters.
- Keep format decisions (PDF vs XLSX, column widths, orientation) declarative in core, but execute conversion in legacy and .NET 9 adapters.
- Characterization tests before refactor: XLSX cell values/columns, PDF page counts/layout hashes, and byte-level output comparison for regression.
