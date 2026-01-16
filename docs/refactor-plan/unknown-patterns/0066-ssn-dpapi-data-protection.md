# 0066 - SSN encryption/decryption via DPAPI + DataProtection fallback

Pattern label:
- SSN fields decrypted with DPAPI (CurrentUser) and fallback to ASP.NET Core DataProtection.

Service/controller:
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Repositories/APVendorRepo.cs (DecryptSsn/DecryptSSNUsingDPAPIAndGetLastFour)

Difference summary (vs known playbooks):
- Uses OS/user-scoped DPAPI (`ProtectedData` + `DataProtectionScope.CurrentUser`) in repository code, which is environment-dependent.
- Mixed crypto scheme with fallback to DataProtection provider; behavior depends on runtime identity and key ring availability.
- Not part of the standard GL/AP extraction patterns and needs explicit parity handling in .NET 9.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Repositories/APVendorRepo.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AccountsPayable.Repositories/Fast.FundView.AccountsPayable.Repositories.csproj (ProtectedData package)
- FundViewUniverse/FundViewGit/DirectoryPackagesProps/Directory.Packages.props (ProtectedData version)

Candidate solutions (core boundary, adapter shape, tests):
- Keep crypto implementation in legacy adapter and expose `ISsnCryptoService` for core usage.
- Preserve DPAPI + DataProtection fallback order and regex-based plaintext detection.
- Add characterization tests with known DPAPI-encrypted samples and DataProtection-encrypted samples to lock decryption behavior.
