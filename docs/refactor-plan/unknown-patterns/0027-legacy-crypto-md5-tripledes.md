# 0027 - Legacy crypto (MD5 + TripleDES ECB)

Pattern label:
- Legacy crypto (MD5 + TripleDES ECB) for signatures

Service/controller:
- Fast.FundView.SystemSecureSignatures.Services/SystemSecureSignatureHelperService.cs
- Fast.FundView.SystemSecureSignatures.Repositories/SystemSecureSignatureCrypto.cs

Difference summary (vs known playbooks):
- Uses custom crypto utilities (MD5 hash derivation + TripleDES in ECB mode) embedded in services/repos.
- Encryption/decryption and hashing logic is not behind a core port, and behavior must remain identical during 1:1 reskin.
- Security modernization is out of scope for reskin but must be isolated for future replacement.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.SystemSecureSignatures.Services/SystemSecureSignatureHelperService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.SystemSecureSignatures.Repositories/SystemSecureSignatureCrypto.cs
- rg -n "MD5.Create|TripleDES" FundViewUniverse/FundViewGit (only in SystemSecureSignatures)

Candidate solutions (core boundary, adapter shape, tests):
- Core port: ISignatureCrypto (EncryptImage/DecryptImage/ComputeHash/VerifyHash) with legacy adapter using current algorithms and parameters.
- Characterization tests before refactor: hash verification round-trips, decrypt(encrypt(x, key), key) == x, and stored hashes remain valid.
- Document in ADR that crypto algorithms are intentionally preserved for 1:1 reskin; revisit after legacy retirement.
