# 0065 - Async event JSON payloads with TypeNameHandling + runtime dispatch

Pattern label:
- Async event persistence uses JSON `$type` metadata and `TypeNameHandling.Objects` for polymorphic serialization/deserialization.

Service/controller:
- FundViewUniverse/FundViewGit/Fast.FundView.AsyncEvents.Services/AsyncEventContextAsyncEventRecorder.cs (serializes events with TypeNameHandling)
- FundViewUniverse/FundViewGit/Fast.FundView.AsyncEvents.Services/ReexecuteAsyncEventService.cs (deserializes + publishes via runtime type)
- FundViewUniverse/FundViewGit/Fast.FundView.AsyncEvents.Services/AsyncEventQueryResponse.cs (reads `$type` from JSON)

Difference summary (vs known playbooks):
- Persists raw JSON with `$type` metadata and relies on `TypeNameHandling` for polymorphic reconstruction.
- Runtime dispatch uses service locator + closed generic publisher types, which is outside the GL/AP core service extraction pattern.
- Creates a tight coupling to Newtonsoft JSON and assembly-qualified type names, which complicates .NET 9 core reuse.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.AsyncEvents.Services/AsyncEventContextAsyncEventRecorder.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AsyncEvents.Services/ReexecuteAsyncEventService.cs
- FundViewUniverse/FundViewGit/Fast.FundView.AsyncEvents.Services/AsyncEventQueryResponse.cs

Candidate solutions (core boundary, adapter shape, tests):
- Define an explicit async-event envelope (`EventType`, `PayloadJson`, `SchemaVersion`) and map to known handlers.
- Replace `TypeNameHandling` with a whitelist-based type registry and explicit deserialization in adapters.
- Keep legacy adapter on Newtonsoft JSON while core uses System.Text.Json with custom converters and tests for round-trip parity.
- Add characterization tests to lock event replay behavior (ID → payload → handler selection).
