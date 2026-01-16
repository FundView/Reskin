# 0054 - Time-queue job scheduling with persisted job entities

Pattern label:
- Time-based job scheduling via IQueueJobBroker (PublishToTimeQueueAsync) with DB-backed job tracking

Service/controller:
- FundViewUniverse/FundViewGit/Fast.FundView.Correspondence.MunicipalCourt/ViolationNotificationJobScheduler.cs
- FundViewUniverse/FundViewGit/Fast.FundView.Correspondence.MunicipalCourt/PaymentPlanInstallmentNotificationJobScheduler.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs (RedisQueueJobBroker wiring)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/MunicipalCourtHelper.cs (enqueue calls)
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtPaymentPlanService.cs

Difference summary (vs known playbooks):
- Schedules jobs into a time queue using Redis-backed broker, then persists job IDs in notification tables.
- Applies time adjustments (TIME_AFTER_NOTIFICATION, HOUR_OF_DAY, RACE_CONDITION_FIX) before enqueueing.
- Uses DB lookups to de-dupe or requeue jobs, creating side effects across queue + database.

Search results (other occurrences, paths):
- FundViewUniverse/FundViewGit/Fast.FundView.Correspondence.MunicipalCourt/ViolationNotificationJobScheduler.cs
- FundViewUniverse/FundViewGit/Fast.FundView.Correspondence.MunicipalCourt/PaymentPlanInstallmentNotificationJobScheduler.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/ServiceProviderHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Libraries/Helpers/MunicipalCourtHelper.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtPaymentPlanService.cs
- FundViewUniverse/FundViewGit/FundView.MVC4.UI/Areas/MunicipalCourt/Services/CourtCitationService.cs

Candidate solutions (core boundary, adapter shape, tests):
- Keep queue broker and persistence in adapters; expose a core scheduling port with idempotent semantics.
- Add characterization tests around scheduled time calculations and requeue behavior (ignorePast, allowDoubleHit).
- Provide a mock IQueueJobBroker in tests to assert job IDs and DB writes.
