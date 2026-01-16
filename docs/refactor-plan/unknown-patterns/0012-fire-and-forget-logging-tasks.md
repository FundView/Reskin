# 0012 - Fire-and-Forget Logging Tasks

- Pattern label: MVC controllers/helpers dispatch background logging via `TaskHelper.FireAndForget` (Task.Run + swallowed exceptions).
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/TaskHelper.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects side effects to be explicit, testable, and injected. Fire-and-forget tasks hide failures and timing, which makes parity checks and core extraction nondeterministic.
- Search results (other occurrences, paths): 4 active call sites in MVC4 UI using `TaskHelper.FireAndForget`.
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce `IBackgroundTaskQueue` or `ILoggingDispatcher` in adapters; keep core logic synchronous.
- Preserve best-effort semantics by swallowing/logging exceptions inside adapter boundaries, not inside core logic.
- Add characterization tests around request logging that assert non-blocking behavior (fake queue with deterministic drain).

Search results list (active)
- `FundViewGit/FundView.MVC4.UI/Controllers/ErrorController.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/WebRequestTrackingController.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries.UI/Extensions/HtmlHelperExtensions.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/LoggingHelper.cs`
