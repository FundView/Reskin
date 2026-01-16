# 0086 - JSON ActionFilter reads Request.InputStream for parameter binding

- Pattern label: MVC ActionFilter reads `HttpContext.Request.InputStream` and deserializes JSON into action parameters.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtVehiclePostViewData.cs` (JsonFilter).
- Difference summary (vs known playbooks): Binding bypasses MVC model binding by manually reading the request body stream and injecting a parameter. This relies on request stream semantics and content type checks and will need explicit handling in .NET 9/ASP.NET Core to preserve behavior.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Models/CourtVehiclePostViewData.cs`
- Candidate solutions (core boundary, adapter shape, tests):
- Keep the filter in the MVC adapter; expose a typed DTO boundary for the core service.
- Implement an ASP.NET Core model binder/filter equivalent for JSON payloads with the same content-type gating.
- Add characterization tests that assert JSON body is parsed once, parameter is set, and non-JSON requests fall back to standard binding.
