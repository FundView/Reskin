# 0057 - SqlClient compatibility layer for DbCommand + BulkCopy

Pattern label:
- SqlClient compatibility: runtime dispatch between System.Data.SqlClient and Microsoft.Data.SqlClient for commands, parameters, and bulk copy

Service/controller:
- FundViewUniverse/FundViewGit/Fast.SqlClientCompatibility/FastLegacySqlClientsDbCommandBuilder.cs (runtime client selection + bulk copy)
- FundViewUniverse/FundViewGit/Fast.SqlClientCompatibility.MicrosoftSqlClient/FastMicrosoftSqlClientOnlyDbCommandBuilder.cs (Microsoft.Data.SqlClient-only implementation)
- FundViewUniverse/FundViewGit/Fast.SqlClientCompatibility.MicrosoftSqlClient/ServiceCollectionExtensions.cs (DI wiring)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs (legacy DI wiring)

Difference summary (vs known playbooks):
- Uses an infrastructure adapter that inspects the active DbConnection type and builds provider-specific SqlCommand/SqlParameter/SqlBulkCopy.
- Bridges EF6-era System.Data.SqlClient and EF Core Microsoft.Data.SqlClient in the same code path.
- Depends on IActiveSqlUserService for ambient connection and transaction, rather than repository injection.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.SqlClientCompatibility.Abstractions/IFastDbCommandBuilder.cs
- FundViewUniverse/FundViewGit/Fast.SqlClientCompatibility.Abstractions/IFastDbBulkCopyCommandBuilder.cs
- FundViewUniverse/FundViewGit/Fast.SqlClientCompatibility.Abstractions/FastSqlCommandBuilderConfiguration.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Models/FundViewReadModel.cs (FetchRawSql uses IFastDbCommandBuilder)
- FundViewUniverse/FundViewGit/Fast.FundView.CashReceipting.Services/Transactions/ReceiptingTransactionNumberIncrementor.cs
- FundViewUniverse/FundViewGit/Fast.FundView.CashReceipting.Services/Transactions/ReceiptingReceiptNumberIncrementor.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Repositories/GLLedgerEntry/GLLedgerEntryNumberRepo.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Services.Posting.EFCore/Services/EFCoreLedgerEntryNumberIncrementer.cs
- FundViewUniverse/FundViewGit/Fast.FundView.GeneralLedger.Api/Configs/fast-sql-command-builder-configuration.json

Candidate solutions (core boundary, adapter shape, tests):
- Keep IFastDbCommandBuilder/IFastDbBulkCopyCommandBuilder as a shared port; provide separate implementations for legacy (System.Data) and new (Microsoft.Data).
- Isolate provider selection to adapters; keep core callers provider-agnostic with DbCommand/DbParameter only.
- Characterization tests: bulk copy timeout and command timeout behavior; verify transactions from IActiveSqlUserService are honored in both providers.
