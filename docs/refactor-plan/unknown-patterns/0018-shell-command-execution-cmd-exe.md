# 0018 - Shell Command Execution via cmd.exe

- Pattern label: Helpers spawn `cmd.exe` with arbitrary command strings using `ProcessStartInfo`.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ConsoleHelper.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects OS-level side effects to be isolated behind adapters. Direct shell execution in helpers ties behavior to Windows hosting and makes .NET 9 reuse and testing harder.
- Search results (other occurrences, paths): 2 active helpers execute shell commands via `cmd.exe`.
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce `ICommandRunner`/`IShellExecutor` adapter with allowlisted commands and environment-specific configuration.
- Keep shell execution in legacy adapters only; core receives parsed results or DTOs.
- Add characterization tests that stub the command runner and assert expected outputs/errors.

Search results list (active)
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ConsoleHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/LoggingHelper.cs`
