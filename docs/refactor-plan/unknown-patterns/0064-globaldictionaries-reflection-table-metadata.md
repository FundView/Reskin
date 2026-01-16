# 0064 - GlobalDictionaries reflection-based table metadata cache

Pattern label:
- Static GlobalDictionaries cache for table/column/primary-key metadata used by EntitySaver and legacy SQL helpers.

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Dictionaries/GlobalDictionaries.cs (static reflection cache)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/EntitySaver.cs (column/PK validation + dynamic save)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Models/SaveMethod.cs (GlobalDictionaries primary keys in save flow)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Repository/System/SystemNotificationRepo.cs (PK lookup via GlobalDictionaries)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/SqlHelper.cs (PK lookup via GlobalDictionaries)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Dictionaries/FundViewTableColumnProvider.cs (adapts GlobalDictionaries to ITableColumnProvider)

Difference summary (vs known playbooks):
- Uses a static reflection-based cache across multiple assemblies to populate schema metadata at startup.
- Operates on string table names and dictionary-based row data, which is outside the GL/AP service extraction pattern.
- Global static caches bypass DI and can break when entity assemblies change or when running outside MVC4 UI.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Entities/EntityBase.cs (GlobalDictionaries primary keys)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Import/ImportHelper.cs (primary key lookup for import temp tables)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ConversionHelper.cs (primary key lookup)
- FundViewUniverse/FundViewGit/Fast.FundView.Legacy.Helpers/LegacySqlHelper.cs (commented GlobalDictionaries usage)

Candidate solutions (core boundary, adapter shape, tests):
- Keep GlobalDictionaries in the legacy adapter and expose an `ITableColumnProvider`/`ITableMetadataProvider` interface in core.
- Implement a .NET 9 provider using EF Core model metadata; inject the provider into EntitySaver/SqlHelper instead of static access.
- Add characterization tests that snapshot primary key + column name lists for representative entities and verify SaveMethod/EntitySaver behavior.
