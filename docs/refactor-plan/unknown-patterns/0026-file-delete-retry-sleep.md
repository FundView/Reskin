# 0026 - Recursive file delete retry with Thread.Sleep

Pattern label:
- Recursive file delete retry with Thread.Sleep

Service/controller:
- FundView.MVC4.UI/Areas/SystemSetup/Services/SystemHelpService.cs (calls delete helper)
- FundView.MVC4.UI/Libraries/Helpers/FileAccessHelper.cs (implements retry)

Difference summary (vs known playbooks):
- Helper uses blocking Thread.Sleep + recursion on IOException with no max retries or cancellation.
- Runs on the request thread and is not expressed as a core boundary/adapter pattern.
- Behavior is "retry until success" and must be preserved during 1:1 reskin.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/FileAccessHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemHelpService.cs
- rg -n "Thread.Sleep(" FundViewUniverse/FundViewGit (only this helper)

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IFileDeleteRetryPolicy with DeleteFileUntilSuccess(string path); legacy adapter keeps identical sleep(100ms) + retry-on-IOException behavior.
- MVC adapter calls the core policy and stays synchronous to preserve MVC behavior.
- Characterization tests before refactor: retries on IOException until success; non-IOException exceptions bubble; delay invoked once per retry (inject delay/clock for tests).
