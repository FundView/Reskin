# 0033 - WCF/SOAP service references (ClientBase, BasicHttpBinding)

Pattern label:
- WCF/SOAP service references (ClientBase, BasicHttpBinding)

Service/controller:
- FastSoftware.FundView.CitationImport/Vendor/CopSync/CopSyncServiceWrapper.cs
- CopSync/Wrapper.cs
- MuniciPayWrapper/MuniciPayWrapper/ProductionClient.cs
- MuniciPayWrapper/MuniciPayWrapper/DemoClient.cs

Difference summary (vs known playbooks):
- Uses generated WCF SOAP clients (Service References) with BasicHttpBinding and endpoints defined in code.
- Client configuration, credentials, and timeouts are embedded in wrappers; not expressed as a core port.
- .NET 9 requires WCF client packages or alternative adapters; behavior must remain identical in 1:1 reskin.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/CopSync/Service References/CopSync/Reference.cs
- FundViewUniverse/FundViewGit/CopSync/Service References/CopSyncDemo/Reference.cs
- FundViewUniverse/FundViewGit/MuniciPayWrapper/MuniciPayWrapper/Service References/ProductionEnvironment/Reference.cs
- FundViewUniverse/FundViewGit/MuniciPayWrapper/MuniciPayWrapper/Service References/DemoEnvironment/Reference.cs
- rg -n "System.ServiceModel|ClientBase<" FundViewUniverse/FundViewGit

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IVendorSoapClient (Fetch/GetPending/MarkAsReceived) and IMuniciPayClient; adapters wrap generated WCF clients.
- Keep binding, endpoints, and credential wiring in adapter composition; allow separate demo/prod adapters.
- Characterization tests before refactor: request/response mapping, timeout behavior, and error propagation from SOAP faults.
