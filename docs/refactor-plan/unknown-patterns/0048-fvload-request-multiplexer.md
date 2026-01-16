# 0048 - FVLoad request multiplexer and dynamic view/service dispatch

Pattern label:
- FVLoad pipeline: URL multiplexer + dynamic view model/service dispatch + partial rendering

Service/controller:
- FundView.MVC4.UI/Controllers/BaseController.cs (FVLoad action + dynamic dispatch)
- FundView.MVC4.UI/Libraries.UI/Extensions/HtmlHelperExtensions.cs (FVLoad helper)
- FundView.MVC4.UI/Controllers/WebRequestTrackingController.cs (RewritePath to /Home/FVLoad + ViewData injection)
- FundView.MVC4.UI/Views/Shared/FVLoad.cshtml (FVLoad view)
- FundView.MVC4.UI/Models/FVLoadModel.cs (URLs model)
- FundView.MVC4.UI/Lib/Scripts/FundView.js (AJAX calls to /Home/FVLoad)

Difference summary (vs known playbooks):
- Legacy UI routes many AJAX/grid/post actions through a single /Home/FVLoad endpoint that accepts multiple URLs (URLs=...).
- FVLoad dynamically resolves ViewModel types or Service methods using dictionaries from FVDataContextFactory, then injects parameters via ValueInjectHelper.
- Request flow relies on HttpContext.Items for ViewData/ContextDictionary, uses RequestFormat=EntityData, and commits/rolls back FundViewReadModel based on context flags (UnitTest).
- WebRequestTrackingController rewrites path to /Home/FVLoad and merges ViewData into querystring, coupling MVC routing to FVLoad semantics.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Extensions/HtmlHelperExtensions.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/WebRequestTrackingController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Views/Shared/FVLoad.cshtml
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Models/FVLoadModel.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView.js

Candidate solutions (core boundary, adapter shape, tests):
- Keep FVLoad in legacy MVC; implement an adapter that calls core use cases directly in .NET 9 (avoid URL multiplexing).
- Introduce an `IFVLoadDispatcher` port to encapsulate URL parsing, view-model creation, and service invocation; migrate callers gradually.
- Characterization tests for multi-URL dispatch, EntityData rendering, context commit/rollback, and ViewData/ContextDictionary injection.
