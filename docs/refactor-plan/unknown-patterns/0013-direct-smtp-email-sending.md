# 0013 - Direct SMTP Email Sending (MVC Services/Controllers)

- Pattern label: MVC controllers/services create `SmtpClient`/`MailMessage` directly (with hard-coded host/credentials) to send emails.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Controllers/AccountController.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects side effects (email) behind adapters or dedicated services. Direct SMTP calls couple UI code to infrastructure, bypass centralized notification services, and make .NET 9 reuse and testing harder.
- Search results (other occurrences, paths): 3 active files with `SmtpClient` usage (2 in MVC4 UI, 1 in Notifications API).
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce `IEmailSender`/`INotificationDispatcher` in core and provide adapters for SMTP/NotificationsAPI.
- Move SMTP configuration to injected settings; keep secrets out of code.
- Add characterization tests for email workflows using a fake email sender (assert recipients, subject/body, and no blocking behavior).

Subpatterns
- MVC4 UI direct SMTP usage (controllers/services).
- Notifications API centralized email service (preferred adapter target).

Subpattern: MVC4 UI direct SMTP usage
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Controllers/AccountController.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemRegistrationFormService.cs`

Subpattern: Notifications API email service
- Search results list (active):
- `FundViewGit/FastSoftware.NotificationsAPI/EmailNotificationService.cs`
