# 0024 - Dynamic SQL String Concatenation in Helpers/Repos

- Pattern label: Helpers/repositories construct SQL strings via string concatenation (table/column names, IDs, and DDL/DML) and execute via raw SQL APIs.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/SQLHelper.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects data access to be parameterized and encapsulated behind repositories or EF queries. Broad dynamic SQL string building couples logic to schema details and complicates .NET 9 reuse, testing, and security reviews.
- Search results (other occurrences, paths): multiple helpers/repos build raw SQL strings outside migrations and CodeSmith.
- Candidate solutions (core boundary, adapter shape, tests):
- Move raw SQL building to adapter layer with parameterized inputs (`SqlParameter`, `ExecuteSqlRaw` with parameters).
- Encapsulate dynamic table/column access behind interfaces (e.g., `IRawSqlDbContext`, `ISchemaQueryService`).
- Add characterization tests around dynamic SQL outputs to preserve exact behavior during extraction.

Search results list (active)
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/SQLHelper.cs`
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/EnumHelper.cs`
- `FundViewGit/Fast.FundView.Legacy.Helpers/EnumRelationshipHelper.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Repository/GenericRepo.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/Dashboard/Services/WhatsNewService.cs`
