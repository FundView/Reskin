# 0043 - FastScreenBuilder grid/table UI composition in ViewData

Pattern label:
- FastScreenBuilder/GridTable server-side UI composition (TableAttributes, TableColumn, DataFv)

Service/controller:
- FundView.MVC4.UI/Areas/SystemSetup/Models/SystemDocumentSignatureRequest/SystemDocumentSignatureRequestIndexViewData.cs
- FundView.MVC4.UI/Areas/AccountsPayable/Models/APBatch/APBatchPayInvoicesViewData.cs
- FundView.MVC4.UI/Areas/GeneralLedger/Models/GLWorkingBudget/GLWorkingBudgetChangePercentViewData.cs
- FundView.MVC4.UI/Areas/SystemSetup/Models/SystemNote/SystemNoteCreateMultipleViewData.cs
- FundView.MVC4.UI/Libraries.UI/Extensions/HtmlHelperExtensions.cs
- FastScreenBuilder/ScreenBuilder.cs

Difference summary (vs known playbooks):
- ViewData classes construct grid/table definitions server-side using FastScreenBuilder types and embed DataFv actions, headers, and search row config.
- UI behavior is defined in C# model constructors rather than controllers or front-end, coupling rendering details to MVC.
- Angular reskin requires translating these grid definitions to client-side components or a shared config schema.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Models/SystemDocumentSignatureRequest/SystemDocumentSignatureRequestIndexViewData.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Models/SystemDocumentSignatureRequest/SystemDocumentSignatureRequestSignedViewData.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Models/SystemNote/SystemNoteCreateMultipleViewData.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/AccountsPayable/Models/APBatch/APBatchPayInvoicesViewData.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Models/GLWorkingBudget/GLWorkingBudgetChangePercentViewData.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Models/GLFiscalYear/GLFiscalYearInitializeViewData.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Extensions/HtmlHelperExtensions.cs
- FundViewUniverse/FundViewGit/FastScreenBuilder/ScreenBuilder.cs

Candidate solutions (core boundary, adapter shape, tests):
- Define a grid configuration DTO schema and serialize FastScreenBuilder definitions into JSON; Angular reads config to render grids.
- Keep FastScreenBuilder in MVC adapters only; core returns data + column metadata without UI actions.
- Characterization tests before refactor: grid headers, column ordering, and DataFv action URL parity.
