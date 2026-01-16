# 0022 - Word Interop DocX-to-PDF Conversion

- Pattern label: Services/helpers rely on `WordInteropPdfConverter` (Office/interop) for DocX-to-PDF conversion.
- Service/controller: `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/DocxConvert.cs`.
- Difference summary (vs known playbooks): GL/AP playbook expects infrastructure-specific dependencies (COM/Office interop) to live behind adapters. Direct interop usage in helpers couples logic to Windows/Office availability and complicates .NET 9 reuse.
- Search results (other occurrences, paths): 3 active files referencing `WordInteropPdfConverter` or `Fast.NetOfficeInterop`.
- Candidate solutions (core boundary, adapter shape, tests):
- Introduce `IDocxToPdfConverter` adapter in core; keep Word interop implementation in legacy adapter or a Windows-only service.
- Provide alternate converter implementation for .NET 9 (e.g., headless conversion service) while preserving legacy behavior during parallel run.
- Add characterization tests around docx-to-pdf conversion inputs/outputs (file size/type, known error cases, timeout behavior).

Subpatterns
- Direct converter instantiation in helpers.
- DI registration of Word interop converter.

Subpattern: Direct converter instantiation
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/DocxConvert.cs`

Subpattern: DI registration of Word interop converter
- Search results list (active):
- `FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs`
- `FundViewGit/Fast.FundView.GeneralLedger.Api/Startup.cs`
