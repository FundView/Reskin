# 0055 - PrintDocument queue + signature/attachment pipeline

Pattern label:
- User-scoped print queue with cancel prompt + DocX generation + signature/attachment output

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs (queue + DocX/PDF generation)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/UsersQueue.cs (user queue + cancellation token)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs (print/sign workflow)
- FundViewUniverse/FundViewGit/Fast.FundView.Legacy.DocX/IPrintDocumentHelper.cs (legacy abstraction)

Difference summary (vs known playbooks):
- Enforces a per-user print queue with cancellation via ValidationButtonException prompt.
- Combines DocX template processing, image tag replacement, signature workflows, and attachment saves in one pipeline.
- Output correctness depends on sequence (queue -> render -> tag replacement -> signature -> PDF), with cancellation handling mid-stream.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/UsersQueue.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtWarrantService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationService.cs

Candidate solutions (core boundary, adapter shape, tests):
- Introduce a core document-generation port returning DocX/PDF streams; keep queue/cancel UI in MVC adapter.
- Centralize signature + attachment persistence in adapters, keep tag replacement rules in core.
- Characterization tests: queue collision prompt, cancel token interruption, PDF output hash with/without signatures.
