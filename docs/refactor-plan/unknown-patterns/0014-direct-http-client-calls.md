# 0014 - Direct HTTP Client Calls in Services/Helpers

- Pattern label: Services/helpers instantiate `HttpClient`/`HttpWebRequest` directly (often with inline auth/headers) to call external endpoints.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries/Utilities/WebRequest.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects external integrations to be accessed via adapters/ports and configured at composition roots. Direct HTTP calls in helpers/services hardwire transport concerns, complicate DI, and make testing and .NET 9 reuse harder.
- Search results (other occurrences, paths): 6 active files with direct HTTP client usage (MVC4 UI helpers/services + Fast.* vendor APIs).
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce typed HTTP clients (`IHttpClientFactory` in .NET 9) or `IVendorApiClient` interfaces; inject into services.
- Keep authentication, base URLs, and TLS/cert handling in adapter configuration only.
- Add characterization tests using fake `HttpMessageHandler` or stubbed web request interfaces to assert requests and responses.

Subpatterns
- MVC4 UI helper/web-request usage (synchronous `HttpWebRequest`).
- MVC4 UI service usage of `HttpClient` with custom handler.
- Fast.* vendor API `HttpClient` usage (citation import).

Subpattern: MVC4 UI helper/web-request usage
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Libraries/Utilities/WebRequest.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/LoggingHelper.cs`

Subpattern: MVC4 UI service `HttpClient` usage
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Areas/FastGovPay/Services/FastGovPayAPIService.cs`

Subpattern: Fast.* vendor API `HttpClient` usage
- Search results list (active):
- `FundViewGit/Fast.FundView.CitationImport.EforceClient/DigiTicketVendorApi.cs`
- `FundViewGit/Fast.FundView.CitationImport.EforceClient/EforceVendorApi.cs`
- `FundViewGit/Fast.FundView.CitationImport.FundViewApi/AbstractFundViewApiVendorApi.cs`
