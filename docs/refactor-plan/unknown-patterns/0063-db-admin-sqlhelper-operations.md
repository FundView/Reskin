# 0063 - Database admin and schema manipulation helpers (backup/restore/drop constraints)

Pattern label:
- SQLHelper-based database admin operations (backup/restore/create/drop constraints) executed from application code.

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/SQLHelper.cs (backup/restore, create DB, delete tables, drop constraints)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/SqlHelper.cs (drop constraints/indexes, dynamic schema SQL)

Difference summary (vs known playbooks):
- These helpers run admin-level SQL (ALTER/CREATE/RESTORE/BACKUP) and schema manipulation from app code, which is outside typical service/core boundaries.
- They build dynamic SQL with server file paths and sys catalog queries, tying behavior to SQL Server and environment-specific paths.
- Refactor playbooks focus on business logic extraction; admin DB operations should remain infra-only and not move into core services.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Migrations/* (commented references to SQLHelper drop-constraint operations)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Wrappers/AsyncContext.cs (raw SQL execution context)

Candidate solutions (core boundary, adapter shape, tests):
- Keep admin SQL helpers in legacy or tooling-only adapters; do not move into core libraries.
- Introduce an `IDatabaseAdminService` interface for any required operations and restrict usage to trusted admin flows.
- Move environment-specific file paths and server details into configuration and validate access at the edge.
- Add manual runbooks and smoke tests for admin operations (backup/restore) instead of unit tests.
