# 0049 - HttpContext.Items used as ambient request state bus

Pattern label:
- Request-scoped state stored in `HttpContext.Items` (ModelState, ValidationButtons, FundViewReadModel, ViewData, WebRequest metadata)

Service/controller:
- FundView.MVC4.UI/EFModel/Models/FundViewReadModel.cs (stores read model in `HttpContext.Items`)
- FundView.MVC4.UI/Controllers/BaseController.cs (writes ModelState/ValidationButtons, UnitTest flags)
- FundView.MVC4.UI/Controllers/ErrorController.cs (reads ModelState/ValidationButtons)
- FundView.MVC4.UI/Libraries.UI/Helpers/FundViewResult.cs (writes ModelState, throws ValidationException)
- FundView.MVC4.UI/Controllers/WebRequestTrackingController.cs (writes ViewData/WebRequest metadata)
- FundView.MVC4.UI/Libraries.UI/Extensions/HtmlHelperExtensions.cs (writes ViewData/WebRequest metadata)
- FundView.MVC4.UI/Global.asax.cs (disposes FundViewReadModel on exception)

Difference summary (vs known playbooks):
- Legacy MVC uses `HttpContext.Items` as an implicit request-scoped bus for state that affects control flow (validation errors, buttons, transactions, read model lifecycle).
- This bypasses DI and explicit parameters; core services depend on the ambient state being present.
- Refactoring to .NET 9 needs a compatible request-scope context (or adapter) so the same state is preserved without leaking across requests.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Models/FundViewReadModel.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/ErrorController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/FundViewResult.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/WebRequestTrackingController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Extensions/HtmlHelperExtensions.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Global.asax.cs

Candidate solutions (core boundary, adapter shape, tests):
- Keep `HttpContext.Items` usage in legacy MVC; introduce an `IRequestContext`/`IScopedContext` abstraction in core and map it to Items in adapters.
- Implement middleware for .NET 9 that populates the scoped context with equivalent data (ModelState/validation buttons/read model).
- Characterization tests for validation/error flows and FundViewReadModel lifecycle (commit/rollback/dispose) that rely on the ambient Items state.
