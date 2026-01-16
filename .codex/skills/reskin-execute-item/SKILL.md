---
name: reskin-execute-item
description: Execute the legacy reskin workflow for a single, already-selected checklist item (service or repository); use after selection to perform extraction, adapter wiring, tests, and checklist updates.
---

# Reskin Execute Item

Use this skill to carry out the reskin for a single item that was already selected by the reskin-next-item skill or an equivalent manual choice.

## Workflow

### 1) Load reskin constraints and definition
- Read `docs\refactor-plan\reskin-definition.md` for what qualifies as reskinned.
- Read `docs\refactor-plan\refactor-plan.md` for sequencing rules and constraints.
- Read `docs\refactor-plan\refactor-todo.md` for the extraction loop checklist.
- Reconfirm EF6 -> EF Core strategy and core purity constraints in `docs\refactor-plan\refactor-plan.md` (EF6 stays in adapters; EF Core lives in new adapters; core logic is EF-free).

### 2) Scope the work to the selected item
- Confirm the selected checklist item name and path from `docs\progress\checklist.md`.
- Identify direct dependencies referenced by the item (services, repositories, helpers).
- If required dependencies are unchecked checklist items, recommend reskinning them first.

### 3) Build the context packet
Use the "AI context packet template" in `docs\refactor-plan\refactor-todo.md` for the selected item and populate it before coding.
The packet can be written as markdown located next to the source service/repository with a name `<item-name>-context-packet.md`.

### 4) Execute the extraction loop
Follow the extraction loop checklist exactly, in order:
1. Identify the pattern and fill the context packet.
2. If the pattern is unknown, document it and add to the Unknown Patterns Backlog.
3. Define core logic boundaries and interfaces (ports).
4. Write characterization tests before refactor.
5. Extract logic into dual-targeting core library using GL/AP naming.
6. Update callers to delegate to the new core logic.
7. Update docs and create (Architecture Decision Record) entries if needed.
8. Run parity tests and snapshot comparisons.
9. Run NDepend and confirm no new violations.

### 5) Repository-specific rules
- Repositories must be moved out of MVC4.UI into dual-targeting projects.
- Ensure EF6 stays in legacy adapters; core libraries must be EF-free and ready for EF Core adapters.
- Capture EF6 -> EF Core mapping decisions in the context packet (queries, mappings, or provider behaviors that must be preserved).
- Update any repository consumers to use the new location without behavior changes.

### 6) Service-specific rules
- Legacy static service remains, delegating to a non-static helper service in a dual-targeting project.
- Add CancellationToken where required by reskin standards.
- Keep behavior identical and ensure any dependent repositories are reskinned or delegated correctly.

### 7) Update the checklist and report
- Mark the item as reskinned in `docs\progress\checklist.md` only when it meets the reskin definition.
- In the response, include:
  - Files touched and why.
  - Tests run and results.
  - NDepend results.
  - Checklist update confirmation.
