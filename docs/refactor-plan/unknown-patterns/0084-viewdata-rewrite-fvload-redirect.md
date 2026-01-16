# 0084 - ViewData Persistence + FVLoad Request Rewrite

- Pattern label: Controller base stores ViewData in `HttpContext.Items` and rewrites the request path to `/Home/FVLoad` with a query string reconstructed from ViewData keys/values.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Controllers/WebRequestTrackingController.cs` (active), `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs` (commented legacy block).
- Difference summary (vs known playbooks): ViewModel state is propagated implicitly via `HttpContext.Items` and then used to rewrite the route mid-request. This couples MVC routing to ViewData shape and relies on side effects rather than explicit inputs, making .NET 9/Angular navigation and testability harder.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Controllers/WebRequestTrackingController.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs` (legacy/commented block)
- Candidate solutions (core boundary, adapter shape, tests):
- Keep the ViewData rewrite behavior in the legacy adapter; introduce explicit DTOs or query models for FVLoad navigation in .NET 9.
- Replace ViewData->query string reconstruction with explicit, typed navigation parameters in the new services/UI.
- Add characterization tests for the rewrite logic (ViewData keys → query string, underscore-to-dot mapping, FVLoad route target).
