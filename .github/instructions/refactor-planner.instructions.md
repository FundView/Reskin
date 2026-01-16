---
applyTo: "**/*.cs,**/*.csproj,**/*.sln,**/*.fs,**/*.vb,**/*.props,**/*.targets,**/*.config,**/*.json,**/*.yml,**/*.yaml"
---

When working on these code/config files:

- Read the repo-local guides first and treat them as primary context:
  - [Resource Guide: Asynchronous AI-Agent-Assisted Large-Scale Code Refactoring (250k+ LOC)](../../%23%20Resource%20Guide%20Asynchronous%20AI-Agent-Assisted%20Large-Scale%20Code%20Refactoring%20%28250k%2B%20LOC%29.md)
  - [Resource Guide: Refactoring .NET C# and Migrating .NET Framework 4.8 to .NET 9](../../%23%20Resource%20Guide%20Refactoring%20NET.md)
  - [Resource Guide: SaaS Project Management Planning and Strategy for Refactoring a .NET Framework 4.8 Application to .NET 9](../../%23%20Resource%20Guide%20SaaS%20Project%20Management%20Planning%20and%20Strategy%20for%20Refactoring%20a%20.NET%20Framework%204.8%20Application%20to%20.NET%209.md)
  - [Resource Guide: NDepend](../../%23%20Resource%20Guide%20NDepend.md)
  - [Resource Guide: Zoran Horvat (Coding Helmet)](../../%23%20Resource%20Guide%20Zoran%20Horvat.md)

- Follow the repository-wide guidance in [../copilot-instructions.md](../copilot-instructions.md), especially:
  - Prefer incremental refactors over rewrites unless explicitly asked.
  - Include safety nets (tests/CI), rollout/rollback, and change controls.
  - Keep changes small, reviewable, and compatible with continuous integration.
  - Integrate security practices into the workflow (not deferred).

- If the user asks for a refactor plan, produce it in the exact A→G structure defined in the repository-wide instructions.
