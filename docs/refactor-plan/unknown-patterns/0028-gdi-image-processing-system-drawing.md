# 0028 - GDI+ image processing in helpers/services (System.Drawing)

Pattern label:
- GDI+ image processing in helpers/services (System.Drawing Image/Bitmap/Graphics)

Service/controller:
- Fast.FundView.SystemSecureSignatures.Services/SystemSecureSignatureHelperService.cs
- FundView.MVC4.UI/Libraries.UI/Helpers/FileHelper.cs
- FundView.MVC4.UI/Libraries/Helpers/ImageHelper.cs

Difference summary (vs known playbooks):
- Uses System.Drawing (GDI+) directly in service/helper code for image load, resize, crop, rotate, and text rendering.
- System.Drawing.Common is Windows-only in .NET 6+; image processing must remain in legacy adapters or be replaced with a compatible library for new services.
- Behavior must remain identical during 1:1 reskin (signature image rotation/cropping/rendering).

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.SystemSecureSignatures.Services/SystemSecureSignatureHelperService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries.UI/Helpers/FileHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ImageHelper.cs
- rg -n "Image.FromStream|Graphics.FromImage|Bitmap(" FundViewUniverse/FundViewGit

Candidate solutions (core boundary, adapter shape, tests):
- Core port: IImageProcessor (Load/Rotate/Resize/Crop/RenderText) with a legacy adapter using System.Drawing.
- For .NET 9 services, add a platform-safe adapter (e.g., ImageSharp) but keep outputs byte-for-byte compatible; validate via characterization tests.
- Characterization tests before refactor: rotation when height > width, crop/resize dimensions, and text-rendered image size.
