# 0053 - System ActionSet dynamic multi-action dispatch and docx merge

Pattern label:
- ActionSet pipeline: URL list parsing + dynamic service invocation + per-entity request splitting + DocX merge

Service/controller:
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemActionSetService.cs (ApplyFASTPost)

Difference summary (vs known playbooks):
- Executes multiple service methods based on an encoded ActionApplicationURL string, not a fixed controller route.
- Splits request dictionaries by SingleEntityID to execute each action per entity, then invokes service methods via reflection.
- Merges returned DocX documents into a single PDF output, coupling action execution to document composition.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/SystemSetup/Services/SystemActionSetService.cs

Candidate solutions (core boundary, adapter shape, tests):
- Define a core ActionSet runner interface that accepts structured action descriptors and per-entity payloads.
- Keep dynamic reflection and DocX merge in adapters; emit typed action results for .NET 9.
- Characterization tests: action ordering, per-entity parameter splitting, and merged PDF output naming/content.
