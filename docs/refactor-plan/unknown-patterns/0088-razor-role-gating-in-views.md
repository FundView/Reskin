# 0088 - Razor view role gating via User.IsInRole / Permissions.IsInRole

- Pattern label: UI navigation and screen sections are gated in Razor with `User.IsInRole(...)`, `Permissions.IsInRole(...)`, and `user.IsAdmin` checks.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Views/Shared/ModuleMenu.cshtml` (role-driven module menu).
- Difference summary (vs known playbooks): Authorization rules are embedded in view templates instead of centralized service logic. The same role checks are duplicated across ModuleMenu, _NavbarHeaderPartial, and module navbars, which makes the reskin to Angular brittle and risks behavior drift. This also ties UI visibility to MVC runtime types (`User`, `Permissions`) rather than injectable permission services.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Views/Shared/_NavbarHeaderPartial.cshtml`
- `FundViewGit/FundView.MVC4.UI/Views/Shared/ModuleMenu.cshtml`
- `FundViewGit/FundView.MVC4.UI/Views/Shared/ModuleNavbars/GeneralLedger.cshtml`
- `FundViewGit/FundView.MVC4.UI/Views/Shared/ModuleNavbars/AccountsPayable.cshtml`
- `FundViewGit/FundView.MVC4.UI/Views/Shared/ModuleNavbars/MunicipalCourt.cshtml`
- `FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Views/SystemDocument/Print.cshtml`
- `FundViewGit/FundView.MVC4.UI/Areas/Dashboard/Views/Main/DashboardIndex.cshtml`
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce a permission service (e.g., `IMenuAuthorization`/`IPermissionService`) that takes role/claims and returns allowed menu items.
- Expose allowed menu items/roles to FundViewClient via an API endpoint; render client menus from the same permission model.
- Keep MVC adapter rendering by calling the same permission service so legacy and new UI stay aligned.
- Add characterization tests that snapshot visible menu items for representative role sets (admin, module-specific, deny roles).
