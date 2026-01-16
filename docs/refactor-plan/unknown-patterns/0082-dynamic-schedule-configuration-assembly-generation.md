# 0082 - Dynamic Schedule Configuration Assembly Generation

- Pattern label: Schedule configuration services are generated at runtime via `ScheduleConfigurationFactoryAssemblyBuilder`, then registered by scanning the dynamic assemblies.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs`.
- Difference summary (vs known playbooks): GL/AP patterns expect explicit, compile-time service registrations. Here, schedule configuration types are created dynamically and registered from in-memory assemblies, which makes the runtime behavior opaque to static analysis, DI registration in .NET 9 harder to replicate, and test setup more brittle.
- Search results (other occurrences, paths):
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs` (CreateDynamicAssemblies/GetDynamicAssemblies/registration).
- Candidate solutions (core boundary, adapter shape, tests):
- Keep the dynamic assembly generation in the MVC adapter and expose a narrow core interface for schedule configuration resolution.
- Replace runtime type generation with explicit registrations or source-generated types in the new .NET 9 service while preserving outputs.
- Add characterization tests that assert the schedule configuration services can be resolved for each schedule type before refactor.
