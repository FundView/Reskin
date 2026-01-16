# 0074 - Legacy Namespace Coupling in Fast.* Assemblies (FundView.Mvc.UI)

- Pattern label: Fast.* projects define and consume types in `FundView.Mvc.UI.*` namespaces (interfaces, expressions, enums), embedding legacy MVC namespaces in new assemblies.
- Service/controller: `FundViewGit/Fast.FundView.AccountsPayable.Providers/IAPVendorSearchable.cs` (Fast.* assembly defines FundView.Mvc.UI interface).
- Difference summary (vs known playbooks): Extracted services and domain code keep legacy MVC namespaces and UI enum dependencies, which ties core logic to MVC-layer naming and types instead of neutral core contracts, complicating clean .NET 9 reuse.
- Search results (other occurrences, paths):
- `FundViewGit/Fast.FundView.AccountsPayable.Providers/IAPVendorSearchable.cs`
- `FundViewGit/Fast.FundView.AccountsPayable.Expressions/APVendorExpressions.cs`
- `FundViewGit/Fast.FundView.AccountsPayable.Services/GlobalUsings.cs` (FundView.Mvc.UI.EFModel/Enums)
- `FundViewGit/Fast.FundView.Banking.Expressions/GlobalUsings.cs`
- `FundViewGit/Fast.FundView.GeneralLedger.Services/GlobalUsings.cs`
- `FundViewGit/Fast.FundView.SystemLedger.Services/GlobalUsings.cs`
- `FundViewGit/PostingApi.EntityRepository.Abstractions/GlobalUsings.cs`
- Candidate solutions (core boundary, adapter shape, tests):
- Keep core libraries in `Fast.FundView.*` namespaces and add a legacy shim/adapter package that exposes `FundView.Mvc.UI.*` types (type forwarders or thin wrappers) for MVC compatibility.
- Define neutral DTOs/enums in core and map to legacy MVC enums in adapters; enforce via NDepend that core cannot reference `FundView.Mvc.UI.*` namespaces.
- Add characterization tests for shim mapping to ensure legacy MVC behavior stays unchanged during migration.
