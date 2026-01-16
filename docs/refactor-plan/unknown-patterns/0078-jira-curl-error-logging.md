# 0078 - Jira Incident Creation via Curl + Local File Spool

- Pattern label: Error logging pipeline writes Jira issues by shelling out to `cmd.exe`/`curl`, writing request payloads to `c:\curl`, and using hard-coded Jira credentials.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/LoggingHelper.cs` (CreateJiraIssue + ExecuteCommand).
- Difference summary (vs known playbooks): Instead of a typed integration or logging provider, the app uses OS-level commands and local file spooling with embedded credentials, coupling runtime behavior to Windows paths and external tooling. This does not align with GL/AP core extraction or .NET 9 service patterns and creates portability/security concerns.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/LoggingHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Controllers/ErrorController.cs` (calls LogWebRequest with CreateJiraTask)
- Candidate solutions (core boundary, adapter shape, tests):
- Extract an `IIncidentReporter` adapter with a secure credential source; keep curl-based implementation in legacy only if needed.
- Replace `cmd.exe`/file-spool with a native HTTP client adapter and isolate to MVC/infra layer.
- Add characterization tests around error-to-incident behavior (ensure no behavior change during reskin).
