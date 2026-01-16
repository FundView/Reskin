# 0067 - Ambient principal lookup via HttpContext.Current + Thread.CurrentPrincipal

Pattern label:
- Static Permissions helper resolves the current user from HttpContext.Current or Thread.CurrentPrincipal.

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/MenuHelper.cs (Permissions.GetPrincipal/IsInRole)

Difference summary (vs known playbooks):
- Uses ambient, static principal resolution instead of injected user context; tightly couples authorization logic to ASP.NET MVC runtime.
- Thread.CurrentPrincipal fallback can differ across execution contexts and background work, making behavior brittle during refactor.
- Not aligned with GL/AP core extraction where user context is passed explicitly or injected.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/MenuHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Core2/DataHelpers/Business/MunicipalCourtBusinessUtilities.cs (commented fallback)

Candidate solutions (core boundary, adapter shape, tests):
- Introduce `ICurrentUserContext` (or similar) with role checks; inject in MVC adapter and .NET 9 services.
- Keep MVC adapter backed by HttpContextAccessor; map to Thread.CurrentPrincipal only in legacy edge.
- Add characterization tests for role checks and anonymous behavior on common menu entry points.
