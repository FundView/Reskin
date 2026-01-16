# 0068 - Signature tablet capture via SigWebTablet (biometric + base64 image)

Pattern label:
- Local signature pad capture using SigWebTablet JS, writing SignatureBiometric + SignatureImgBase64 fields for document signing.

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Lib/Scripts/ThirdParty/SigWebTablet.js (tablet HTTP API wrapper)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView/signatureFunctions.js (capture workflow + hidden fields)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView/fundview-ui.js (binds signature button handlers)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Views/SystemDocument/Sign.cshtml (canvas + hidden signature fields)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs (consumes Signature* fields)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs (signature image/biometric in doc output)

Difference summary (vs known playbooks):
- Depends on a local SigWeb tablet service (tablet.sigwebtablet.com:47289/47290) and browser XHR to capture signatures.
- UI behavior is wired through CSS class names + jQuery handlers rather than view-model bindings.
- Signature payload (biometric string + base64 image) flows into print/sign pipelines and must remain byte-for-byte compatible.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Lib/Scripts/ThirdParty/SigWebTablet.js
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView/signatureFunctions.js
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Lib/Scripts/FundView/fundview-ui.js
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Views/SystemDocument/Sign.cshtml
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemDocumentService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs

Candidate solutions (core boundary, adapter shape, tests):
- Provide a FundViewClient signature-capture adapter wrapping SigWebTablet or replacement vendor SDK; keep capture logic out of core.
- Define a stable signature DTO (SignatureImgBase64, SignatureBiometric) and validate in MVC/.NET 9 adapters before core use.
- Characterization tests: require signature before confirm, payload populates hidden fields, and signed document output matches legacy.
