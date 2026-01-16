# 0030 - PDF watermarking/merge via PdfSharp in MVC helpers

Pattern label:
- PDF watermarking/merge via PdfSharp in MVC helpers

Service/controller:
- FundView.MVC4.UI/Libraries.UI/Helpers/FileHelper.cs (watermarking)
- FundView.MVC4.UI/Libraries/Extensions/PDFExtensions.cs (merge helpers)

Difference summary (vs known playbooks):
- Uses PdfSharp directly in MVC helper/extension code to open, modify, and merge PDFs.
- Watermarking writes a temp file to disk and re-encodes the PDF for legacy download responses.
- Not expressed as a core boundary; must remain in legacy adapters during 1:1 reskin.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/FileHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Extensions/PDFExtensions.cs
- rg -n "PdfSharp|PdfDocument|PdfReader" FundViewUniverse/FundViewGit

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IPdfMutator with WatermarkIfPdf(Stream/path, watermark) and MergePages(IEnumerable<PdfPage>) methods.
- Legacy adapter keeps PdfSharp implementation and temp file behavior; .NET 9 adapter can use a supported PDF library.
- Characterization tests before refactor: watermark text placement, file round-trip integrity, and merged page count.
