# 0032 - Process-scoped semaphore locks for cross-request workflows

Pattern label:
- Process-scoped semaphore locks (ConcurrentDictionary<string, SemaphoreSlim>)

Service/controller:
- FundView.MVC4.UI/Libraries/Helpers/CitationImportLockService.cs
- Fast.FundView.CashReceipting.Services/CashReceiptingLockService.cs

Difference summary (vs known playbooks):
- Uses in-memory, process-scoped locks to serialize operations by city/vendor across requests.
- Lock registry is static and not distributed; behavior depends on single-process execution.
- Concurrency control is embedded in helper/service rather than an explicit core boundary.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/CitationImportLockService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.CashReceipting.Services/CashReceiptingLockService.cs
- rg -n "ConcurrentDictionary<string, SemaphoreSlim>" FundViewUniverse/FundViewGit

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IConcurrencyGate or ILockProvider with AcquireAsync(key) returning a disposable/releaser.
- Legacy adapter keeps in-memory SemaphoreSlim behavior; .NET 9 can optionally swap to distributed locks without changing call sites.
- Characterization tests before refactor: operations serialize for same key, allow parallel for different keys, and always release locks on exceptions.
