# 0006 - EFModel Validators Mixing Repo Access + UI Exceptions

- Pattern label: EFModel validators call repos directly and throw `ValidationButtonException`.
- Service/controller: `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtPersonValidator.cs`.
- Difference summary (vs known playbooks): validation logic lives in EFModel validators, pulls data via repos, and throws UI exceptions. This mixes validation rules, data access, and UI flow in a non-service layer, making core extraction and shared use harder.
- Search results (other occurrences, paths): 4 validator files with repo access; 3 of those throw `ValidationButtonException`.
- Candidate solutions (core boundary, adapter shape, tests):
- Move validation rules into core services that return structured validation results (errors + prompts).
- Keep `ValidationButtonException` creation in MVC adapters only.
- Add characterization tests for validation outcomes and prompt payloads before refactor.

Search results list (repo access)
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtCitationValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtPersonValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtVehicleValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtViolationValidator.cs`

Search results list (ValidationButtonException)
- `FundViewGit/FundView.MVC4.UI/EFModel/Helpers/PrintDocumentHelper.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtPersonValidator.cs`
- `FundViewGit/FundView.MVC4.UI/EFModel/Validation/CourtVehicleValidator.cs`
