# 0046 - Legacy ASP.NET Membership/Identity provider usage

Pattern label:
- System.Web.Security Membership/Roles + OWIN Identity pipeline over MembershipDbContext

Service/controller:
- FundView.MVC4.UI/Controllers/AccountController.cs (MembershipProvider + direct membership DB queries)
- FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemUsersController.cs (MembershipDbContext + RoleManager)
- FundView.MVC4.UI/Service References/App_Start/Startup.Auth.cs (MembershipDbContext/UserManager/RoleManager wiring)
- FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs (membership roles lookup)
- FundView.MVC4.UI/Web.config (SqlMembershipProvider/SqlRoleProvider configuration)

Difference summary (vs known playbooks):
- Legacy MVC uses ASP.NET Membership + Roles with OWIN Identity wrappers and a separate membership database.
- Role checks are scattered across controllers, services, and Razor views, tying authorization to legacy provider types.
- Core extraction needs a stable adapter that preserves MembershipDbContext behavior and role resolution while enabling .NET 9 reuse.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Controllers/AccountController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Controllers/SystemUsersController.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Service References/App_Start/Startup.Auth.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Tools/DatabaseHelpers/FVDataContext.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Web.config
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Views/Shared/_NavbarHeaderPartial.cshtml

Candidate solutions (core boundary, adapter shape, tests):
- Keep ASP.NET Membership provider configuration in legacy MVC; expose an `IUserRoleService`/`IUserAccountService` port for core logic.
- Implement a legacy adapter that uses MembershipDbContext/RoleManager and an API adapter for .NET 9 that calls the same membership store.
- Characterization tests around role resolution, authentication flows, and SystemUsers admin operations before extracting.
