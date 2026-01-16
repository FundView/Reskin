# 0015 - Custom TLS Certificate Validation Overrides

- Pattern label: Services override TLS certificate validation with `ServerCertificateValidationCallback` (cert hash pinning and demo bypass).
- Service/controller: `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Services/FastGovPayAPIService.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects external integrations to be encapsulated behind adapters with explicit configuration. Inline TLS validation logic inside MVC services couples transport security to business logic and complicates .NET 9 reuse and testing.
- Search results (other occurrences, paths): 1 active occurrence in MVC4 UI.
- Candidate solutions (core boundary, adapter shape, tests):
- Move TLS validation/pinning to an HTTP client adapter configured in composition root.
- Use typed HTTP clients (`IHttpClientFactory`) with environment-specific handler policies.
- Add characterization tests for the adapter to verify certificate policy and demo mode behavior without touching core logic.

Subpatterns
- Certificate hash pinning in HTTP handler.
- Demo/override bypass via configuration flags.

Search results list (active)
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Services/FastGovPayAPIService.cs`
