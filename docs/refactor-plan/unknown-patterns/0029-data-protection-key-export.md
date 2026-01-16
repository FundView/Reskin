# 0029 - DataProtection key export to local disk

Pattern label:
- DataProtection key export to local disk (IKeyManager + XmlSerializer)

Service/controller:
- FundView.MVC4.UI/Libraries/Helpers/GetAllKeys.cs

Difference summary (vs known playbooks):
- Directly reads the ASP.NET Core DataProtection key ring and serializes key material to a user-scoped local folder (`c:\temp\keys\{user}`).
- Uses static lock + _alreadyDone flag and filesystem writes in a helper; not expressed as a core boundary/adapter.
- Sensitive key export behavior must remain identical during 1:1 reskin; future hardening is a separate effort.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/GetAllKeys.cs
- rg -n "IKeyManager|GetAllKeys" FundViewUniverse/FundViewGit

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IDataProtectionKeyExporter with ExportAllKeys(); legacy adapter keeps current file output and locking semantics.
- For .NET 9 services, prefer standard DataProtection key persistence providers; only keep export path if explicitly required.
- Characterization tests before refactor: exported file count equals key count, filename format `key-{id}.xml`, and directory creation uses current user name.
