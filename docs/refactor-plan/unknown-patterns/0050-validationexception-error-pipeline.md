# 0050 - ValidationException-driven UI error pipeline

Pattern label:
- Throw `ValidationException` to short-circuit flow and return JSON errors via ErrorController

Service/controller:
- FundView.MVC4.UI/Libraries.UI/Helpers/FundViewResult.cs (Serialize writes ModelState to HttpContext.Items then throws)
- FundView.MVC4.UI/Controllers/BaseController.cs (ThrowIfInvalid/UpdateModelOrThrow set Items + throw)
- FundView.MVC4.UI/Controllers/ErrorController.cs (maps ValidationException to JSON errors + buttons)
- FundView.MVC4.UI/Global.asax.cs (ExceptionPublisherExceptionFilter routes to ErrorController)

Difference summary (vs known playbooks):
- Validation is handled by throwing exceptions instead of returning typed results.
- Error response format is produced centrally (ErrorController) using HttpContext.Items for ModelState/ValidationButtons.
- This exception-driven flow is cross-cutting and must be preserved to avoid UI regression (AJAX expects JSON error payloads).

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/FundViewResult.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/ErrorController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Global.asax.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/AccountController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/CodeEnforcement/Controllers/CECaseController.cs

Candidate solutions (core boundary, adapter shape, tests):
- Keep exception-driven validation in legacy MVC; in .NET 9 map ValidationException to a standardized error result DTO.
- Introduce a `IValidationErrorFactory` to build error payloads instead of relying on HttpContext.Items.
- Characterization tests for AJAX endpoints to assert error JSON shape and button payloads when ValidationException is thrown.
