# GL/AP Legacy MVC Service Playbooks (Extracted Patterns)

Source locations
- `FundViewGit/FundView.MVC4.UI/Areas/AccountsPayable/Services/*.cs`
- `FundViewGit/FundView.MVC4.UI/Areas/GeneralLedger/Services/*.cs`

Purpose
- Define the legacy MVC shim patterns used to call refactored helper services.
- Provide a repeatable playbook for other modules during reskin.

Playbook 1: Legacy MVC Service Shim (Area Services)
- Shape: class in `FundView.Mvc.UI.Areas.<Module>.Services` with `[Service("/Area/Endpoint")]`.
- Methods: `static` methods that accept view data or command DTOs plus `IFundViewReadModel context`.
- Dependency resolution: `var service = context.GetRequiredService<SomeHelperService>();`
- Behavior: no business logic beyond orchestration, input mapping, and response shaping.
- Output patterns:
  - Grid: `result.SerializeGrid()`
  - Entity data: `result.ToEntityData()` or `new { ... }.ToEntityData()`
  - File output: `report.ToFVFile()`
  - Void: `string.Empty`

Examples
- `APBatchRepostService.GetIndexTable` delegates to `APBatchRepostHelperService` and serializes grid.
- `GLAccountService.EditPost` delegates to `GLAccountHelperService` and returns `ToEntityData()`.

Playbook 2: Dictionary-to-DTO Mapping
- When legacy endpoints submit `Dictionary<string,string>`:
  - Create DTO (example: `APBatchRepostMaintenanceCommandDTO`).
  - `request.InjectFrom(dictionaryRequest);`
  - `request.ExtensionData = dictionaryRequest;`
  - Delegate to helper service.
- Keep mapping and validation in MVC shim. Core/helper services accept typed DTOs.

Example
- `APBatchRepostService.UpdateMaintenanceInputs`.

Playbook 3: Grid and Sub-Grid Queries
- MVC shim calls repo or helper service that returns `TableQueryResult<T>`.
- Return `SerializeGrid()` to preserve legacy grid behavior.
- Keep branching logic (batch types, sub-grid selection) in MVC shim if it is purely orchestration.

Example
- `GLBatchService.GetIndexPartial` and `GLBatchService.GetIndexSubGrid`.

Playbook 4: Report Generation and File Output
- MVC shim requests report data or report file from helper service.
- Return `ToFVFile()` for legacy download behavior.

Example
- `APAgingReportService.PrintAgingReport`.

Playbook 5: HttpContext and File Uploads
- Keep `HttpContext` and request-file handling in MVC shim.
- Pass streams and file names to helper service for processing.

Example
- `GLBatchService.ValidateBatchImport` uses `HttpContext.Current.Request.Files` and delegates to helper.

Playbook 6: ContextDictionary Updates
- MVC shim may update `context.ContextDictionary` to preserve legacy pipeline behavior.
- Use this only for legacy-specific context propagation.

Example
- `GLBatchService.EditPost` stores `BatchID` and `BatchType` in `ContextDictionary`.

Playbook 7: Cross-Module Helper Calls
- MVC shim can resolve helpers from other modules via `context.GetRequiredService<T>()`.
- Use this for shared logic only; prefer shared abstractions over direct module coupling.

Example
- `APInvoiceAccountService.UpdateDescriptionAndAccountLedgerEntryDescription` uses `GLAccountLedgerEntryHelperService`.

Playbook 8: Helper Service Shape (Refactored Logic)
- Location: `Fast.FundView.<Module>.Services` (GL/AP pattern).
- Attribute: `[ScopedService]`.
- Dependencies: injected via DI (e.g., `I<Module>DbContext`, query services, repositories).
- Behavior: holds business logic, data access, and domain rules to be reused by legacy MVC and new services.
- No `System.Web` or MVC types in helper services.

Example
- `APInvoiceAccountHelperService` uses `IAccountsPayableDbContext` and query services.

Checklist for new modules
- Create helper service in `Fast.FundView.<Module>.Services` with `[ScopedService]`.
- Move logic from legacy MVC service into helper service without behavior changes.
- Keep MVC shim static and thin; only map inputs and serialize outputs.
- Preserve legacy serialization (`SerializeGrid`, `ToEntityData`, `ToFVFile`).
- Keep HttpContext and file IO in MVC shim.
- Add characterization tests around helper service methods.
