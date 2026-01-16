# 0042 - DynamicModel/Massive direct database access in controllers

Pattern label:
- DynamicModel/Massive dynamic SQL access with string table names and ExpandoObject rows

Service/controller:
- FundView.MVC4.UI/Controllers/BaseController.cs
- FundView.MVC4.UI/Controllers/AccountController.cs
- FundView.MVC4.UI/Libraries/Helpers/Massive.cs
- Codesmith/Massive.cs

Difference summary (vs known playbooks):
- Controllers use DynamicModel with string table names and dynamic rows (ExpandoObject) to query and insert data directly.
- Bypasses EF repositories and typed DTOs, tightly coupling UI flows to database schema and membership tables.
- GL/AP playbooks expect repository/query services with explicit contracts and adapter isolation; this pattern is hard to reuse in .NET 9 without a compatibility adapter.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/BaseController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/AccountController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/Massive.cs
- FundViewUniverse/FundViewGit/Codesmith/Massive.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemUsersController.cs (commented)

Candidate solutions (core boundary, adapter shape, tests):
- Core port: typed query/services for UDD and membership operations; adapters handle DynamicModel calls until retired.
- Replace DynamicModel table access with EF or repository services; keep schema-specific logic in adapter layer.
- Characterization tests before refactor: data shape returned by DynamicModel and mutation side effects (UDD inserts/updates).
