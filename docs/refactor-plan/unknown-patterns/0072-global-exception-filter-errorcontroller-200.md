# 0072 - Global Exception Filter Routes to ErrorController (HTTP 200 + JSON Errors)

- Pattern label: MVC global exception filter catches all exceptions, routes to `ErrorController.Index`, forces HTTP 200, and returns JSON error payloads (validation errors + optional buttons).
- Service/controller: `FundViewGit/FundView.MVC4.UI/Global.asax.cs` (ExceptionPublisherExceptionFilter).
- Difference summary (vs known playbooks): Error handling is centralized in MVC, uses HttpContext.Items for model state + request metadata, logs via fire-and-forget, and returns HTTP 200 even on exceptions. This is tightly coupled to MVC and client-side expectations, and does not match core service error handling patterns.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Global.asax.cs`
- `FundViewGit/FundView.MVC4.UI/Service References/App_Start/FilterConfig.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/ErrorController.cs`
- Candidate solutions (core boundary, adapter shape, tests):
- Keep global exception filter and ErrorController behavior in legacy MVC adapters; expose core exceptions as standardized DTOs.
- For .NET 9 services, map to ProblemDetails (or an equivalent error DTO) but preserve legacy JSON shape for MVC/legacy clients until cutover.
- Add characterization tests around error JSON payloads (`error`, `errorButtons`) and HTTP status expectations for AJAX requests.
