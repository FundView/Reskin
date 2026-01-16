# 0040 - Zip archive creation and download of server-side directories

Pattern label:
- Zip archive creation and download of server-side directories (ZipFile.CreateFromDirectory + FileStream)

Service/controller:
- FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationImportService.cs

Difference summary (vs known playbooks):
- Service logic performs filesystem archive creation and returns downloadable content directly, coupling core logic to IO and storage layout.
- Archive naming, temp file handling, and delete/overwrite behavior are embedded in service flow rather than an adapter.
- Output bytes and filenames are part of user-visible behavior that must remain unchanged during reskin.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationImportService.cs

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IArchiveBuilder (directory -> stream) or IInvalidFileBundleBuilder; adapters handle filesystem access and ZipFile usage.
- Keep directory discovery and file IO in MVC adapters; pass only streams/metadata to core.
- Characterization tests before refactor: archive contents list, ordering, and filename parity.
