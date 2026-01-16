# 0052 - CopSync citation import pipeline (SOAP + XML cleanup + dynamic mapping)

Pattern label:
- CopSync vendor import: SOAP client + XML string normalization + dynamic dictionary mapping to citation entities

Service/controller:
- FundViewUniverse/FundViewGit/CopSync/CopSync.cs (XML cleanup + XmlSerializer + dynamic Person/Vehicle/Citation/Violation dictionaries)
- FundViewUniverse/FundViewGit/CopSync/Wrapper.cs (SOAP client wrapper, prod/demo endpoints)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Import/CitationImport/ThirdPartyImporterFactory.cs (vendor wiring)
- FundViewUniverse/FundViewGit/FastSoftware.FundView.CitationImport/Vendor/CopSync/CopSyncServiceWrapper.cs (newer vendor adapter)

Difference summary (vs known playbooks):
- Vendor-specific SOAP integration with custom XML string munging prior to XmlSerializer.
- Maps XML into dynamic dictionaries representing multiple related entities (person, vehicle, citation, violations).
- Writes and reads filesystem artifacts (CopSyncDONOTFETCH.txt) as part of the import flow.
- Behavior depends on vendor schema quirks (namespaces, xsi:nil, copsync type attributes), making refactor sensitive.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/CopSync/CopSync.cs
- FundViewUniverse/FundViewGit/CopSync/Wrapper.cs
- FundViewUniverse/FundViewGit/CopSync/Service References/CopSync/Reference.cs
- FundViewUniverse/FundViewGit/CopSync/Service References/CopSyncDemo/Reference.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Import/CitationImport/ThirdPartyImporterFactory.cs
- FundViewUniverse/FundViewGit/FastSoftware.FundView.CitationImport/Vendor/CopSync/CopSyncServiceWrapper.cs

Candidate solutions (core boundary, adapter shape, tests):
- Keep CopSync SOAP client and XML normalization in an adapter; expose a core port that accepts normalized DTOs.
- Introduce a deterministic XML normalization step with characterization tests using sample CopSync payloads.
- Map dynamic dictionaries to typed DTOs early; keep entity-specific defaults (e.g., bond fields) centralized.
- Add regression tests for citation counts, violation numbering, and required field defaults.
