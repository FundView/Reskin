# FundViewGit Refactor Planning TODO

## How to use
- Update this file as the plan is refined; keep one line per task with clear owner and due date where possible.
- Unknown patterns: add services to the Unknown Patterns Backlog and mark for pair-programming.

## Known decisions
- Module scope: entire repo except CodeSmith and tooling folders.
- 1:1 reskin: no behavior, schema, API, or side-effect changes; zero UI tolerance.
- UI translation: Razor and MVC UI flows translated to Angular in FundViewClient with parity.
- Dual-target core libraries; EF6 in legacy adapters, EF Core for new services.
- No feature work during reskin; reskin and bugfix only.
- Legacy retirement after full reskin, SME and QA sign-off, and beta completion.
- Sequencing: pick a legacy screen, refactor the dependent services, then build the FundViewClient screen.
- Test data: production data handling managed outside this project.
- NDepend: architecture-only gates, fail on new violations only; run on PRs and nightly; code-quality rules report-only.
- NDepend suppression: document and suppress narrowly; review suppressions at wave end.
- NDepend baseline: Phase 0 baseline, refresh after each completed wave with ADR.
- NDepend reports: store HTML reports as CI artifacts with long-term GitHub retention; wave-end trend review.

## Planning backlog (generate and refine the plan)
- [x] Confirm any additional repo exceptions beyond CodeSmith.
- [x] Extract GL/AP canonical patterns from `FundView.MVC4.UI/Areas/*/Services`.
- [ ] Define FundViewClient parity checklist and mapping from Razor to Angular.
- [ ] Create NDepend project and baseline report for FundView.sln.
- [x] Implement NDepend architecture ruleset v1 and phased gating plan.
- [ ] Document current branching policy details for CI gates.

## NDepend integration
- [ ] Set up NDepend analysis for FundView.sln and generate baseline reports.
- [x] Draft CQLinq rules at `docs/refactor-plan/ndepend/ndepend-rules.ndrules`.
- [ ] Import rules into the NDepend project and configure fail-on-new violations.
- [ ] Enable optional report-only code-quality rules for visibility.
- [ ] Capture dependency graphs and DSM for each module wave.

## Pattern inventory backlog
- [ ] MVC Areas Services injection pattern (manual injection of refactored core service).
- [ ] Controller pattern: thin controllers delegating to core use cases.
- [ ] HelperService pattern: CRUD and validation with DbContext adapters.
- [ ] Repository pattern: TableQueryResult pagination and filtering.
- [ ] Mapper pattern: Entity_To_*_Mapper classes.
- [ ] Report pattern: data shaping in core, rendering in adapters.
- [ ] Import pipeline pattern: parsing and validation in core, IO in adapters.

## Extraction loop checklist (repeat per service or controller)
- [ ] Identify pattern and fill context packet.
- [ ] Unknown pattern? Document differences and search for all occurrences across the codebase.
- [ ] Define core logic boundary and interfaces (ports).
- [ ] Write characterization tests before refactor; verify tests fail on behavior change.
- [ ] Add xUnit characterization tests with near-100% coverage target.
- [ ] Extract logic to core library using GL/AP naming and placement.
- [ ] Implement adapter in existing service or repository.
- [ ] Update controller or service to call new logic.
- [ ] Add or update docs and ADR entry.
- [ ] Run parity tests and snapshot comparisons.
- [ ] Run NDepend analysis and confirm no new violations.

## AI context packet template
- Service or controller name:
- Module:
- Pattern label:
- Responsibilities:
- Inputs and outputs (DTOs, commands, queries):
- Invariants and business rules:
- Data access (DbContext, tables, stored procs):
- Side effects (files, queues, emails):
- Dependencies (services, repositories):
- Error handling and validation:
- Existing tests (yes or no, location):
- Rollback approach:
- Unknown pattern? (yes or no; if yes, add to backlog)

## Unknown Patterns Backlog
- [ ] <service> - <pattern summary> - <owner> - <notes>
- [ ] ValidationButtonException UI prompt/redirect - see `docs/refactor-plan/unknown-patterns/0001-validation-button-exception-ui-prompt.md`
- [ ] MVC Service direct Repo calls (no helper service) - see `docs/refactor-plan/unknown-patterns/0002-mvc-service-direct-repo-calls.md`
- [ ] ViewData models performing repo queries - see `docs/refactor-plan/unknown-patterns/0003-viewdata-repo-queries.md`
- [ ] MVC Helpers/Tools with direct repo access - see `docs/refactor-plan/unknown-patterns/0004-mvc-helpers-tools-direct-repo-access.md`
- [ ] MVC Controllers with direct repo access - see `docs/refactor-plan/unknown-patterns/0005-mvc-controllers-direct-repo-access.md`
- [ ] EFModel validators mixing repo access + UI exceptions - see `docs/refactor-plan/unknown-patterns/0006-efmodel-validators-repo-access-ui-exceptions.md`
- [ ] Sync-over-async blocking calls - see `docs/refactor-plan/unknown-patterns/0007-sync-over-async-blocking-calls.md`
- [ ] Service layer HttpContext.Current dependencies - see `docs/refactor-plan/unknown-patterns/0008-service-layer-httpcontext-dependencies.md`
- [ ] FundViewReadModel creation outside DI - see `docs/refactor-plan/unknown-patterns/0009-manual-fundviewreadmodel-instantiation.md`
- [ ] Hard-coded file system paths and direct IO - see `docs/refactor-plan/unknown-patterns/0010-hardcoded-file-system-paths.md`
- [ ] Direct ConfigurationManager access - see `docs/refactor-plan/unknown-patterns/0011-direct-configurationmanager-access.md`
- [ ] Fire-and-forget logging tasks - see `docs/refactor-plan/unknown-patterns/0012-fire-and-forget-logging-tasks.md`
- [ ] Direct SMTP email sending - see `docs/refactor-plan/unknown-patterns/0013-direct-smtp-email-sending.md`
- [ ] Direct HTTP client calls in services/helpers - see `docs/refactor-plan/unknown-patterns/0014-direct-http-client-calls.md`
- [ ] Custom TLS certificate validation overrides - see `docs/refactor-plan/unknown-patterns/0015-custom-tls-certificate-validation.md`
- [ ] Web root file access via Server.MapPath - see `docs/refactor-plan/unknown-patterns/0016-web-root-file-access-server-mappath.md`
- [ ] Sequence number generation with process-scoped locks - see `docs/refactor-plan/unknown-patterns/0017-sequence-number-generation-static-locks.md`
- [ ] Shell command execution via cmd.exe - see `docs/refactor-plan/unknown-patterns/0018-shell-command-execution-cmd-exe.md`
- [ ] Runtime assembly loading for reflection registries - see `docs/refactor-plan/unknown-patterns/0019-runtime-assembly-loading-reflection-registry.md`
- [ ] Task.Run async wrappers in repos/helpers - see `docs/refactor-plan/unknown-patterns/0020-task-run-async-wrappers.md`
- [ ] Hard-coded credentials in code - see `docs/refactor-plan/unknown-patterns/0021-hardcoded-credentials-in-code.md`
- [ ] Word interop DocX-to-PDF conversion - see `docs/refactor-plan/unknown-patterns/0022-word-interop-docx-to-pdf-conversion.md`
- [ ] Repositories returning HTML markup / UI actions - see `docs/refactor-plan/unknown-patterns/0023-repository-returns-html-markup.md`
- [ ] Dynamic SQL string concatenation in helpers/repos - see `docs/refactor-plan/unknown-patterns/0024-dynamic-sql-string-concatenation.md`
- [ ] Direct time/guid usage (no abstraction) - see `docs/refactor-plan/unknown-patterns/0025-time-and-guid-direct-usage.md`
- [ ] Recursive file delete retry with Thread.Sleep - see `docs/refactor-plan/unknown-patterns/0026-file-delete-retry-sleep.md`
- [ ] Legacy crypto (MD5 + TripleDES ECB) - see `docs/refactor-plan/unknown-patterns/0027-legacy-crypto-md5-tripledes.md`
- [ ] GDI+ image processing in helpers/services (System.Drawing) - see `docs/refactor-plan/unknown-patterns/0028-gdi-image-processing-system-drawing.md`
- [ ] DataProtection key export to local disk - see `docs/refactor-plan/unknown-patterns/0029-data-protection-key-export.md`
- [ ] PDF watermarking/merge via PdfSharp in MVC helpers - see `docs/refactor-plan/unknown-patterns/0030-pdfsharp-watermark-and-merge.md`
- [ ] DocX template generation/merge via Xceed - see `docs/refactor-plan/unknown-patterns/0031-docx-template-generation-xceed.md`
- [ ] Process-scoped semaphore locks for cross-request workflows - see `docs/refactor-plan/unknown-patterns/0032-process-scoped-semaphore-locks.md`
- [ ] WCF/SOAP service references (ClientBase, BasicHttpBinding) - see `docs/refactor-plan/unknown-patterns/0033-wcf-soap-service-references.md`
- [ ] XML import/export parsing with XmlSerializer/XmlDocument - see `docs/refactor-plan/unknown-patterns/0034-xml-import-export-parsing.md`
- [ ] XLSX report generation and PDF conversion via Fast.Common.Spreadsheets - see `docs/refactor-plan/unknown-patterns/0035-xlsx-pdf-report-conversion.md`
- [ ] jQuery DataTables param parsing and grid serialization in repos/services - see `docs/refactor-plan/unknown-patterns/0036-jquery-datatables-grid-coupling.md`
- [ ] Stringly-typed request parameter bags (Dictionary<string, string>) - see `docs/refactor-plan/unknown-patterns/0037-dictionary-request-parameter-bags.md`
- [ ] Context-based service locator (GetRequiredService / ServiceProviderHelper) - see `docs/refactor-plan/unknown-patterns/0038-context-service-locator.md`
- [ ] FVDataContextFactory global registries and user cache - see `docs/refactor-plan/unknown-patterns/0039-fvdatacontextfactory-global-registries.md`
- [ ] Zip archive creation and download of server-side directories - see `docs/refactor-plan/unknown-patterns/0040-zip-archive-download-of-server-directories.md`
- [ ] ContextDictionary ambient output and parameter injection - see `docs/refactor-plan/unknown-patterns/0041-contextdictionary-ambient-output.md`
- [ ] DynamicModel/Massive direct database access in controllers - see `docs/refactor-plan/unknown-patterns/0042-dynamicmodel-massive-direct-db-access.md`
- [ ] FastScreenBuilder grid/table UI composition in ViewData - see `docs/refactor-plan/unknown-patterns/0043-fastscreenbuilder-grid-ui-composition.md`
- [ ] ServiceAttribute reflection-based dispatch (FVLoad service routing) - see `docs/refactor-plan/unknown-patterns/0044-serviceattribute-reflection-dispatch.md`
- [ ] Reflection-based value injection (ValueInjectHelper/InjectFrom) - see `docs/refactor-plan/unknown-patterns/0045-reflection-based-value-injection.md`
- [ ] Legacy ASP.NET Membership/Identity provider usage - see `docs/refactor-plan/unknown-patterns/0046-legacy-membership-identity-provider.md`
- [ ] Custom MVC model binder reads Request.Form for multi-select - see `docs/refactor-plan/unknown-patterns/0047-custom-mvc-model-binder-request-form.md`
- [ ] FVLoad request multiplexer and dynamic view/service dispatch - see `docs/refactor-plan/unknown-patterns/0048-fvload-request-multiplexer.md`
- [ ] HttpContext.Items used as ambient request state bus - see `docs/refactor-plan/unknown-patterns/0049-httpcontext-items-ambient-state.md`
- [ ] ValidationException-driven UI error pipeline - see `docs/refactor-plan/unknown-patterns/0050-validationexception-error-pipeline.md`
- [ ] Telerik Reporting report pipeline and designer assets - see `docs/refactor-plan/unknown-patterns/0051-telerik-reporting-report-pipeline.md`
- [ ] CopSync citation import pipeline (SOAP + XML cleanup + dynamic mapping) - see `docs/refactor-plan/unknown-patterns/0052-copsync-citation-import-pipeline.md`
- [ ] System ActionSet dynamic multi-action dispatch and docx merge - see `docs/refactor-plan/unknown-patterns/0053-system-actionset-dynamic-multi-action-dispatch.md`
- [ ] Time-queue job scheduling with persisted job entities - see `docs/refactor-plan/unknown-patterns/0054-time-queue-job-scheduling.md`
- [ ] PrintDocument queue + signature/attachment pipeline - see `docs/refactor-plan/unknown-patterns/0055-print-document-queue-signature-pipeline.md`
- [ ] Dynamic SelectList/Select2 pipeline (config JSON + SQL builder + reflection) - see `docs/refactor-plan/unknown-patterns/0056-dynamic-selectlist-pipeline.md`
- [ ] SqlClient compatibility layer for DbCommand + BulkCopy - see `docs/refactor-plan/unknown-patterns/0057-sqlclient-compatibility-bulkcopy.md`
- [ ] Legacy CSV/XLSX import/export pipeline (stringly-typed rows + file staging) - see `docs/refactor-plan/unknown-patterns/0058-legacy-csv-xlsx-import-export-pipeline.md`
- [ ] SystemTemplate search-criteria JSON pipeline (template providers + DB persistence) - see `docs/refactor-plan/unknown-patterns/0059-system-template-searchcriteria-json-pipeline.md`
- [ ] LiveGrid inline editing pipeline (HTML inputs + data-updated AJAX) - see `docs/refactor-plan/unknown-patterns/0060-livegrid-inline-editing-pipeline.md`
- [ ] JsonWrapper JSON payload transport (implicit string conversions) - see `docs/refactor-plan/unknown-patterns/0061-jsonwrapper-payload-transport.md`
- [ ] Conversion tooling and vendor schema mapping (CSV/XLSX caches + validation outputs) - see `docs/refactor-plan/unknown-patterns/0062-conversion-tooling-vendor-schema-mapping.md`
- [ ] Database admin and schema manipulation helpers (backup/restore/drop constraints) - see `docs/refactor-plan/unknown-patterns/0063-db-admin-sqlhelper-operations.md`
- [ ] GlobalDictionaries reflection-based table metadata cache - see `docs/refactor-plan/unknown-patterns/0064-globaldictionaries-reflection-table-metadata.md`
- [ ] Async event JSON payloads with TypeNameHandling + runtime dispatch - see `docs/refactor-plan/unknown-patterns/0065-async-events-typenamehandling-json.md`
- [ ] SSN encryption/decryption via DPAPI + DataProtection fallback - see `docs/refactor-plan/unknown-patterns/0066-ssn-dpapi-data-protection.md`
- [ ] Ambient principal lookup via HttpContext.Current + Thread.CurrentPrincipal - see `docs/refactor-plan/unknown-patterns/0067-ambient-principal-thread-currentprincipal.md`
- [ ] Signature tablet capture via SigWebTablet (biometric + base64 image) - see `docs/refactor-plan/unknown-patterns/0068-signature-tablet-sigweb-capture.md`
- [ ] FVFile base64 file response format (filename//contentType\base64) - see `docs/refactor-plan/unknown-patterns/0069-fvfile-base64-download-format.md`
- [ ] MVC controllers direct LINQ-to-SQL DataContext access - see `docs/refactor-plan/unknown-patterns/0070-mvc-controllers-direct-linq-to-sql-datacontext.md`
- [ ] FundViewResult server-rendered HTML in JSON responses - see `docs/refactor-plan/unknown-patterns/0071-fundviewresult-server-rendered-html-json.md`
- [ ] Global exception filter routes to ErrorController (HTTP 200 + JSON errors) - see `docs/refactor-plan/unknown-patterns/0072-global-exception-filter-errorcontroller-200.md`
- [ ] DataFv UI action protocol (data-fv JSON command bus) - see `docs/refactor-plan/unknown-patterns/0073-datafv-ui-action-protocol.md`
- [ ] Legacy namespace coupling in Fast.* assemblies (FundView.Mvc.UI) - see `docs/refactor-plan/unknown-patterns/0074-legacy-namespace-coupling-fast-assemblies.md`
- [ ] Service layer reads Request.Params/Files (implicit request binding) - see `docs/refactor-plan/unknown-patterns/0075-service-layer-request-params-files-binding.md`
- [ ] User Defined Data (UDD) dynamic field pipeline - see `docs/refactor-plan/unknown-patterns/0076-udd-dynamic-fields-pipeline.md`
- [ ] RequestFormat/Redraw dual-response mode - see `docs/refactor-plan/unknown-patterns/0077-requestformat-redraw-dual-response-mode.md`
- [ ] Jira incident creation via curl + local file spool - see `docs/refactor-plan/unknown-patterns/0078-jira-curl-error-logging.md`
- [ ] Custom MVC binder/validation overrides (decimal/guid + implicit required off) - see `docs/refactor-plan/unknown-patterns/0079-custom-mvc-binder-validation-overrides.md`
- [ ] ExcludeAllModelBinder on grid ViewData - see `docs/refactor-plan/unknown-patterns/0080-excludeallmodelbinder-grid-viewdata.md`
- [ ] Custom MVC client validation attributes + JS adapters - see `docs/refactor-plan/unknown-patterns/0081-custom-mvc-client-validation-attributes.md`
- [ ] Dynamic schedule configuration assembly generation - see `docs/refactor-plan/unknown-patterns/0082-dynamic-schedule-configuration-assembly-generation.md`
- [ ] Reflection-based dynamic type instantiation - see `docs/refactor-plan/unknown-patterns/0083-reflection-based-type-instantiation.md`
- [ ] ViewData persistence + FVLoad request rewrite - see `docs/refactor-plan/unknown-patterns/0084-viewdata-rewrite-fvload-redirect.md`
- [ ] Parallel.ForEach helper conversions and data hydration - see `docs/refactor-plan/unknown-patterns/0085-parallel-foreach-helper-conversions.md`
- [ ] JSON ActionFilter reads Request.InputStream for parameter binding - see `docs/refactor-plan/unknown-patterns/0086-json-actionfilter-inputstream-binding.md`
- [ ] Print document user-queue cancellation gate - see `docs/refactor-plan/unknown-patterns/0087-print-document-user-queue-cancellation.md`
- [ ] Razor view role gating via User.IsInRole/Permissions.IsInRole - see `docs/refactor-plan/unknown-patterns/0088-razor-role-gating-in-views.md`

See the Unknown Patterns register and template at `docs/refactor-plan/unknown-patterns/README.md`.
