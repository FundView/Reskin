# 0021 - Hard-Coded Credentials in Code

- Pattern label: Services/controllers embed credentials in code (SMTP username/password literals).
- Service/controller: `FundViewGit/FundView.MVC4.UI/Controllers/AccountController.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects secrets to be injected via configuration providers or secret stores. Hard-coded credentials tie behavior to source control and block clean reuse in .NET 9.
- Search results (other occurrences, paths): 2 active files with hard-coded SMTP credentials (commented examples excluded).
- Candidate solutions (core boundary, adapter shape, tests):
- Move credentials to injected settings (`IOptions<T>` or secret store); keep SMTP adapter in legacy layer.
- Provide `IEmailSender` adapter configured at composition root; core receives only DTOs.
- Add characterization tests that assert email flows without asserting on secret values.

Search results list (active)
- `FundViewGit/FundView.MVC4.UI/Controllers/AccountController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemRegistrationFormService.cs`
