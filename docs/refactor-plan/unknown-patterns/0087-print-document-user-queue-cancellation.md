# 0087 - Print document user-queue cancellation gate

- Pattern label: Process-scoped per-user queue with cancellation tokens used to gate print jobs and prompt cancel/close on duplicates.
- Service/controller: `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/UsersQueue.cs`, `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs`.
- Difference summary (vs known playbooks): Print flows rely on a static, in-memory `UserQueue` keyed by username + database to prevent concurrent print jobs. It issues `ValidationButtonException` prompts for duplicate requests and uses a 10-hour `CancellationTokenSource` TTL. Behavior is process-scoped and not distributed, so concurrency/queue semantics must be preserved during extraction.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/UsersQueue.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs`
- Candidate solutions (core boundary, adapter shape, tests):
- Core port: `IUserOperationGate` returning a token or a conflict result; legacy adapter keeps the in-memory queue and ValidationButtonException prompts.
- .NET 9 adapter can swap to a distributed gate if needed without altering call sites.
- Characterization tests before refactor: single active job per (user, database), duplicate attempt yields cancel/close prompt, cancel action stops the running job and removes the user from the queue.
