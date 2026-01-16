# Resource Guide: Asynchronous AI-Agent-Assisted Large-Scale Code Refactoring (250k+ LOC)

## Overview

Refactoring a 250k+ LOC codebase with asynchronous AI agents is best treated as a socio-technical program where architecture, quality engineering, security, and governance are coupled. Asynchrony increases throughput but also increases coordination risk through non-determinism, partial work, conflicting edits, and tool failures that resemble distributed-systems failure modes. Human-in-the-loop (HITL) control remains central because many refactors require intent preservation, domain judgment, and accountable approvals beyond test outcomes. Agent systems are more than “better prompting”: they are software systems that combine models, tools, policies, traces, and evaluation harnesses that drift over time. This guide prioritizes primary sources (standards, official docs, benchmark definitions, and reproducible repos) and labels preprints where relevant. This guide is informational only and is not legal, security, or financial advice.

## How to Use This Guide

Sections are grouped by decision area (governance, refactoring mechanics, agents, orchestration, assurance, and security) rather than by vendor. Links within each subcategory are intended to stand alone, with specifications and benchmark definitions placed before secondary explanations. Preprints are included when influential but are best interpreted alongside repositories, benchmarks, and versioned specifications.

## Source Quality Notes

Primary sources (NIST, OWASP projects, standards bodies, official SDK documentation, and benchmark repositories) are emphasized because they define requirements, interfaces, or evaluation targets. Secondary sources are included when they consolidate widely adopted terminology or cross-organization experience. Preprints and fast-moving repositories can be informative but unstable; they are labeled and paired with reproducibility artifacts when available. Social media sources are excluded even when they host primary announcements.

## Table of Contents

* [Strategic program design and governance](#strategic-program-design-and-governance)
* [Large-scale refactoring mechanics](#large-scale-refactoring-mechanics)
* [Agentic software engineering foundations](#agentic-software-engineering-foundations)
* [Multi-agent orchestration and asynchrony](#multi-agent-orchestration-and-asynchrony)
* [Human-in-the-loop controls and traceability](#human-in-the-loop-controls-and-traceability)
* [Codebase understanding, retrieval, and transformation tooling](#codebase-understanding-retrieval-and-transformation-tooling)
* [Quality assurance and release safety](#quality-assurance-and-release-safety)
* [Security, privacy, and supply chain integrity](#security-privacy-and-supply-chain-integrity)
* [Cross-cutting themes](#cross-cutting-themes)
* [Keeping Current](#keeping-current)
* [Glossary](#glossary)
* [References and Link Count Summary](#references-and-link-count-summary)

---

## Strategic program design and governance

At this scale, refactoring is usually a portfolio of coordinated changes rather than a single technical task, and organizational coupling can dominate technical coupling. Governance defines what “acceptable change” means, how risk is communicated, and how accountability is preserved when automation accelerates change volume. Asynchronous agent systems introduce additional governance needs: durable records of decisions, measurable quality gates, and clear ownership boundaries. The sources below focus on program framing, reliability posture, and measurement vocabulary that support executive and audit conversations.

* [Software Engineering at Google (online book)](https://abseil.io/resources/swe-book) — Engineering governance and scaling software change work.
* [Site Reliability Engineering (online book)](https://sre.google/sre-book/table-of-contents/) — Reliability framing for change risk and operations.
* [DORA metrics overview (Atlassian)](https://www.atlassian.com/devops/frameworks/dora-metrics) — Four delivery metrics commonly used for reporting.

### Modernization and incremental change patterns

Incremental modernization patterns often mediate between “refactor” and “rewrite” when availability, compliance, or market timing limits disruptive work. These patterns are naturally compatible with asynchronous agents because they tend to localize risk and produce reviewable diffs. They also provide stable vocabulary for discussing partial migration, controlled exposure, and reversibility. The links describe widely cited change-isolation approaches rather than agent-specific techniques.

* [Strangler Fig Application (Martin Fowler)](https://martinfowler.com/bliki/StranglerFigApplication.html) — Gradual replacement pattern for legacy functionality.
* [Branch by abstraction (AWS Prescriptive Guidance)](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-decomposing-monoliths/branch-by-abstraction.html) — Isolation via abstraction layers during replacement.
* [Feature Toggles (Martin Fowler)](https://martinfowler.com/articles/feature-toggles.html) — Controlled exposure of changes via feature flags.

### Program risk, maturity, and reporting frameworks

Risk frameworks help translate agent capability into accountable controls, particularly when tools can execute actions against repositories and CI systems. Maturity models and SDLC frameworks provide language for governance outcomes, evidence collection, and audit scope. These sources are commonly used as references for secure development programs and AI governance. They can be mapped to evidence artifacts such as approvals, traces, and evaluation results.

* [AI Risk Management Framework (NIST)](https://www.nist.gov/itl/ai-risk-management-framework) — NIST governance framing for AI risk decisions.
* [Secure Software Development Framework (NIST SP 800-218)](https://csrc.nist.gov/pubs/sp/800/218/final) — SDLC outcome language for security practices.
* [OWASP SAMM](https://owaspsamm.org/) — Software assurance maturity model and practices.

---

## Large-scale refactoring mechanics

Refactoring at scale depends on repeatable, semantics-preserving transforms and disciplined separation between mechanical change and behavior change. Large-scale change work also benefits from refactoring-aware diffs and measurement systems that reduce review friction. When AI agents propose code edits, these foundations remain the baseline for correctness, reviewability, and rollback plausibility. The sources below emphasize catalogs, detection tools, and large-scale refactoring experience reports.

* [Refactoring.com](https://www.refactoring.com/) — Refactoring catalog context and references.
* [Code smells and refactoring (open textbook)](https://open.oregonstate.education/setextbook/chapter/code-smells/) — Open educational overview of smells and refactors.
* [RefactoringMiner (GitHub)](https://github.com/tsantalis/RefactoringMiner) — Refactoring detection and refactoring-aware diffs.

### Refactoring catalogs and change taxonomies

Catalogs and taxonomies provide a shared language for what counts as a refactor and what risks are typical for each change shape. They also help reviewers interpret agent-produced diffs as known transformations rather than novel rewrites. This subcategory includes widely used catalogs and a refactoring-aware analysis tool that supports evidence generation. These references support consistent classification across teams and over time.

* [Refactoring catalog (Refactoring.com)](https://refactoring.com/catalog/) — Named refactorings and intent-level descriptions.
* [Refactoring Guru: Refactoring](https://refactoring.guru/refactoring) — Refactoring concepts and code smell taxonomy.
* [RefactoringMiner: AST diff features](https://github.com/tsantalis/RefactoringMiner#ast-diff-features) — Refactoring-aware AST differencing capabilities.

### Large-scale change practices and experience reports

Published large-scale change experience clarifies the social and technical mechanics that dominate at scale: ownership boundaries, batching trade-offs, tooling constraints, and communication patterns. These sources offer a stable vocabulary for “program-level refactoring” that is independent of any single AI tool. Two sources are preprints and are labeled as such. They are useful for constraints and failure-mode awareness even when implementation choices differ.

* [Large-Scale Changes (SWE book, Chapter 22)](https://abseil.io/resources/swe-book/html/ch22.html) — Google framing of large-scale change programs.
* [ClangMR (preprint, arXiv)](https://arxiv.org/abs/1908.02136) — Large-scale automated refactoring for C/C++.
* [Experiences with large-scale refactoring (preprint, arXiv)](https://arxiv.org/abs/1705.03435) — Industry experiences and scale trade-offs.

---

## Agentic software engineering foundations

Agentic refactoring systems combine models, tools, policy constraints, traces, and evaluation into workflows that behave like software systems. This section focuses on the primitives that define agent behavior (tools, handoffs, context boundaries) and on the evaluation artifacts that prevent quality drift. Official SDK documentation is emphasized because it defines interface and telemetry semantics. Benchmark sources are included because they provide stable targets for regression tracking across model versions.

* [Agents guide (OpenAI API)](https://platform.openai.com/docs/guides/agents) — Agent concepts, tools, and platform capabilities.
* [Agents SDK guide (OpenAI API)](https://platform.openai.com/docs/guides/agents-sdk) — SDK primitives, handoffs, and tracing concepts.
* [Build with Claude (Anthropic)](https://www.anthropic.com/learn/build-with-claude) — Official developer guidance for tool-using models.

### Tool-use primitives and agent-computer interfaces

Tool use defines the agent’s action surface, which directly affects risk: repository access, CI execution, secret exposure, and environment mutation. Agent-computer interface design also shapes observability because tool calls and intermediate state are the evidence trail for why a patch was produced. The sources below cover prominent platform primitives and an open protocol for tool and resource integration. These references are relevant even when the underlying model family changes.

* [New tools for building agents (OpenAI)](https://openai.com/index/new-tools-for-building-agents/) — Tool-use primitives and platform building blocks.
* [Advanced tool use (Anthropic Engineering)](https://www.anthropic.com/engineering/advanced-tool-use) — Dynamic tool discovery and execution concepts.
* [Model Context Protocol specification](https://modelcontextprotocol.io/specification/2025-03-26) — Open protocol for tools, resources, and prompts.

### Benchmarks, datasets, and evaluation harnesses

Benchmarks make agent changes comparable across time by controlling tasks, scoring, and datasets, which helps prevent regressions masked by subjective impressions. Software engineering benchmarks also clarify what is being measured (patch correctness, test passing, task completion under constraints). These references provide benchmark definitions and reproducibility artifacts widely used for coding agents. Evaluation frameworks complement benchmarks by supporting custom regression suites that match a specific codebase’s risk profile.

* [SWE-bench (website)](https://www.swebench.com/) — Benchmark overview and dataset documentation.
* [SWE-bench (GitHub)](https://github.com/princeton-nlp/SWE-bench) — Dataset, scripts, and evaluation tooling.
* [OpenAI Evals (GitHub)](https://github.com/openai/evals) — Evaluation framework and benchmark registry.

---

## Multi-agent orchestration and asynchrony

Asynchronous multi-agent systems reduce latency and increase throughput, but they also introduce coordination issues similar to distributed systems (ordering, retries, idempotency, partial failure). Orchestration determines how dependencies are expressed, how state is persisted, and how conflicts are surfaced for resolution. Refactoring programs benefit from durable execution semantics and observable workflows because work spans many changes and long horizons. The sources below cover agent frameworks, workflow engines, and durable distributed execution primitives.

* [Microsoft AutoGen (GitHub)](https://github.com/microsoft/autogen) — Agentic AI programming framework and ecosystem.
* [LangGraph overview (LangChain docs)](https://docs.langchain.com/oss/python/langgraph/overview) — Graph-based orchestration concepts for agents.
* [Temporal platform documentation](https://docs.temporal.io/) — Durable workflow orchestration documentation.

### Agent orchestration frameworks and runtimes

Frameworks shape coordination patterns (graphs, multi-agent conversations, handoffs) and influence traceability and failure behavior. For large refactoring programs, frameworks with explicit state and inspectable events can simplify governance and incident analysis. This subcategory includes prominent open frameworks and official SDK documentation that define orchestration primitives. The focus is on interfaces and semantics rather than preferred implementations.

* [OpenAI Agents SDK (reference site)](https://openai.github.io/openai-agents-python/) — SDK reference documentation and core concepts.
* [AutoGen documentation](https://microsoft.github.io/autogen/stable/index.html) — Official docs including asynchronous architecture notes.
* [LangGraph reference (LangChain)](https://reference.langchain.com/python/langgraph/) — Unified reference for LangGraph APIs.

### Durable execution and distributed asynchronous work

Durable execution supports long-running refactoring jobs through persistent state, retries, and explicit execution history, which are valuable for auditability and reliability. Distributed execution primitives support parallel work when many modules or repositories are refactored concurrently. These sources cover durable workflow semantics and distributed compute abstractions that are frequently used to operationalize asynchronous work. They provide terminology for execution identity, history, and coordination.

* [Temporal workflows](https://docs.temporal.io/workflows) — Durable workflow concepts and reliability constraints.
* [Ray actors](https://docs.ray.io/en/latest/ray-core/actors.html) — Stateful distributed workers for parallel workloads.
* [Ray tasks](https://docs.ray.io/en/latest/ray-core/tasks.html) — Asynchronous task execution and object passing.

---

## Human-in-the-loop controls and traceability

HITL control is both a risk-reduction strategy and a knowledge-capture mechanism that preserves intent and accountability. In asynchronous systems, HITL also functions as concurrency control by resolving conflicts, prioritizing changes, and rejecting unsafe edits. Traceability provides the evidence chain connecting agent actions, tool calls, and accepted code changes. The sources below emphasize review guidance, change-control semantics, and trace standards used for audit-quality telemetry.

* [Code review (Google Engineering Practices)](https://google.github.io/eng-practices/review/) — Review principles for scalable engineering organizations.
* [Pull request reviews (GitHub Docs)](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews) — Platform review states and collaboration semantics.
* [Documenting architecture decisions (Cognitect)](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions) — Architecture Decision Records concept and rationale.

### Change control: reviews, policies, and protected surfaces

Change-control policy defines which changes are reviewable, which checks are required, and which branches or paths are protected from unreviewed edits. In agentic refactoring, these controls determine how agent output becomes mergeable change while preserving accountability. This subcategory includes guidance on change descriptions and platform policy constructs. These references help connect governance intent to enforceable repository rules.

* [Change descriptions (Google Engineering Practices)](https://google.github.io/eng-practices/review/developer/cl-descriptions.html) — Guidance for reviewer-facing change narratives.
* [Protected branches (GitHub Docs)](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches) — Branch protection concepts and enforcement controls.
* [Code scanning (GitHub Docs)](https://docs.github.com/en/code-security/code-scanning) — Platform scanning features tied to policies.

### Traces, observability standards, and audit logs

Traces provide a causal record of what occurred during an agent run: model calls, tool calls, handoffs, and intermediate events. In asynchronous settings, traces also clarify retries, partial completions, and failure propagation across workflows. This subcategory focuses on widely used observability standards and official agent-tracing documentation. These sources support audit, debugging, and post-incident analysis.

* [Trace Context (W3C Recommendation)](https://www.w3.org/TR/trace-context/) — Standard format for distributed trace propagation.
* [Traces concepts (OpenTelemetry)](https://opentelemetry.io/docs/concepts/signals/traces/) — Trace/span terminology for observability systems.
* [Tracing (OpenAI Agents SDK docs)](https://openai.github.io/openai-agents-python/tracing/) — Agent run event capture and tracing semantics.

---

## Codebase understanding, retrieval, and transformation tooling

Large refactoring depends on accurate code understanding, dependency awareness, and consistent interpretation of symbols and types. Agent systems often require structured representations (parsing, language services, refactoring-aware diffs) to reduce hallucinated APIs and incoherent edits. Transformation tooling provides determinism and repeatability that support review and rollback when agents propose high volumes of changes. The sources below cover parsing and language intelligence primitives plus transformation frameworks used for repeatable refactors.

* [tree-sitter (GitHub)](https://github.com/tree-sitter/tree-sitter) — Incremental parsing library supporting many languages.
* [Language Server Protocol](https://microsoft.github.io/language-server-protocol/) — Standard protocol for editor language intelligence.
* [OpenRewrite recipe catalog](https://docs.openrewrite.org/recipes) — Catalog of automated refactoring recipes.

### Parsing, semantic diffs, and refactoring-aware comparisons

Semantic diffs help separate formatting noise from meaningful structural edits, which becomes important when automation increases change volume. Refactoring-aware diffs can classify changes as known transformations and reduce reviewer effort for mechanical edits. Parsing ecosystems provide the foundation for feature extraction, structured context retrieval, and consistent symbol resolution. The sources below cover a structural diff tool, a refactoring-aware diff system, and a parser documentation hub.

* [difftastic (GitHub)](https://github.com/Wilfred/difftastic) — Structural diff tool for syntax-aware comparisons.
* [RefactoringMiner: AST diff features](https://github.com/tsantalis/RefactoringMiner#ast-diff-features) — Refactoring-aware AST differencing capabilities.
* [tree-sitter documentation](https://tree-sitter.github.io/tree-sitter/) — Parsing concepts and grammar ecosystem overview.

### Deterministic transformation frameworks and language-specific refactoring

Transformation frameworks can encode change as recipes/templates, supporting repeatability, controlled scope, and testable semantics. Language-specific tooling can add strong guarantees through compiler services and analyzers, which is especially relevant when agents propose edits. This subcategory includes an open transformation framework plus official .NET compiler-platform documentation. These references are useful as “guardrail layers” that complement free-form model output.

* [Recipes concepts (OpenRewrite docs)](https://docs.openrewrite.org/concepts-and-explanations/recipes) — Lifecycle and semantics of transformation recipes.
* [Authoring recipes (OpenRewrite docs)](https://docs.openrewrite.org/authoring-recipes) — Recipe structure and testing concepts reference.
* [Roslyn SDK overview (Microsoft Learn)](https://learn.microsoft.com/en-us/dotnet/csharp/roslyn-sdk/) — Compiler platform APIs for analyzers and code fixes.

---

## Quality assurance and release safety

Refactoring programs depend on confidence mechanisms that detect regressions, reduce rollback cost, and provide stable signals to reviewers. Agent acceleration increases change volume, which raises the value of automated checks and disciplined integration practices. Release safety also depends on change isolation and exposure control that limits blast radius when regressions occur. The sources below focus on reliability framing, integration discipline, and quality signal systems that scale.

* [Site Reliability Engineering (online book)](https://sre.google/sre-book/table-of-contents/) — Reliability concepts for operating evolving systems.
* [Continuous Integration (Martin Fowler)](https://martinfowler.com/articles/continuousIntegration.html) — Integration discipline and failure-mode framing.
* [Trunk Based Development](https://trunkbaseddevelopment.com/) — Branching model overview and references.

### Static analysis and security scanning as scalable signal

Static analysis provides broad coverage for bug classes and vulnerability patterns that tests may miss, especially when large volumes of code are changing. For AI-assisted refactoring, static analysis can serve as an independent validator for generated patches. This subcategory includes widely used static analysis and code scanning references. These sources support consistent policy-driven detection rather than ad hoc review.

* [CodeQL documentation](https://codeql.github.com/docs/) — Query-based static analysis documentation and releases.
* [CodeQL code scanning overview (GitHub Docs)](https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning-with-codeql) — Code scanning concepts and alert semantics.
* [Semgrep documentation](https://semgrep.dev/docs/) — Static analysis engine documentation and concepts.

### Release safety, change isolation, and controlled exposure

Release safety depends on limiting exposure of risky changes and preserving rapid recovery options, especially when many small changes are merged continuously. Change isolation patterns provide vocabulary for separating mechanical refactors from behavior changes and for sequencing modernization. Controlled exposure mechanisms reduce risk when behavior changes are unavoidable. The sources below describe widely cited patterns and reliability perspectives relevant to large refactoring programs.

* [Feature Toggles (Martin Fowler)](https://martinfowler.com/articles/feature-toggles.html) — Controlled exposure of changes via feature flags.
* [Branch by abstraction (AWS Prescriptive Guidance)](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-decomposing-monoliths/branch-by-abstraction.html) — Isolation strategy during incremental replacement.
* [Release Engineering (SRE book)](https://sre.google/sre-book/release-engineering/) — Reliability perspectives on release processes.

---

## Security, privacy, and supply chain integrity

Agentic refactoring expands the attack surface because agents can access repositories, execute tools, and potentially expose secrets or sensitive code through prompts and logs. Security and compliance constraints influence orchestration choices through permission models, tool boundaries, and evidence retention requirements. Supply chain transparency standards help defend change lineage to enterprise customers and regulators. The sources below cover secure development frameworks, LLM-specific security risks, and supply chain integrity standards.

* [Secure Software Development Framework (NIST SP 800-218)](https://csrc.nist.gov/pubs/sp/800/218/final) — Secure SDLC practices and outcome language.
* [SLSA (Supply-chain Levels for Software Artifacts)](https://slsa.dev/) — Supply chain vocabulary and integrity levels.
* [CycloneDX specification overview](https://cyclonedx.org/specification/overview/) — SBOM standard for components and dependencies.

### GenAI/LLM application security

LLM-enabled developer workflows introduce risks such as prompt injection, data leakage, and unsafe tool invocation, which differ from traditional web threat models. These risks become more acute when agents can run commands, modify repositories, or access proprietary information. This subcategory includes OWASP’s official LLM risk project materials and an engineering reference on tool boundary design. The goal is risk vocabulary and reference baselines rather than implementation guidance.

* [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — Official risk list and project context.
* [OWASP Top 10 for LLMs (2025 PDF)](https://owasp.org/www-project-top-10-for-large-language-model-applications/assets/PDF/OWASP-Top-10-for-LLMs-v2025.pdf) — 2025 risk descriptions and mitigation categories.
* [Writing tools for agents (Anthropic Engineering)](https://www.anthropic.com/engineering/writing-tools-for-agents) — Tool schema and boundary design considerations.

### Provenance, SBOM, and VEX transparency

Provenance standards address “what was built from what inputs under which controls,” which becomes central when automation increases change velocity. SBOM standards describe software composition, while VEX standards describe vulnerability impact status in context. Together they support evidence-based remediation and customer transparency during refactoring and dependency changes. The references below include signing/provenance ecosystems, SBOM specifications, and a CISA VEX minimum-requirements reference.

* [Sigstore documentation](https://docs.sigstore.dev/) — Artifact signing and verification ecosystem references.
* [SPDX specification (v3 web)](https://spdx.github.io/spdx-spec/v3.0.1/) — SPDX web specification for software transparency.
* [CISA: Minimum requirements for VEX (PDF)](https://www.cisa.gov/sites/default/files/2023-04/minimum-requirements-for-vex-508c.pdf) — Minimum elements for VEX documents.

---

## Cross-cutting themes

Accountability concerns cut across all sections because agents can change code, trigger CI, and generate artifacts that influence security, compliance, and customer trust. Governance frameworks help distinguish capability from acceptable operational risk, particularly when refactoring is business-critical. Supply chain standards provide a shared vocabulary for provenance, integrity, and transparency that can be communicated externally. LLM security guidance highlights risks (prompt injection, unsafe tool use) that are distinct from traditional application threat models. These themes are best interpreted as risk lenses rather than technology preferences.

* [ISO/IEC 42001:2023 overview (ISO)](https://www.iso.org/standard/42001) — AI management system standard scope and intent.
* [AI RMF 1.0 (NIST PDF)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf) — Primary NIST publication for AI risk framing.
* [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — LLM application threat categories and mitigations.
* [SLSA specification overview](https://slsa.dev/spec/v1.0/about) — SLSA concepts and threat model framing.
* [Secure Software Development Framework (NIST SP 800-218)](https://csrc.nist.gov/pubs/sp/800/218/final) — Secure SDLC outcomes across engineering programs.

---

## Keeping Current

Agent frameworks, benchmarks, and security guidance evolve quickly, so currency benefits from tracking benchmark repositories, standards bodies, and project pages rather than vendor announcements alone. Peer-reviewed venues and systematic reviews help identify shifts in evaluation methodology and recurring failure modes across organizations. Security projects like OWASP and publications like NIST provide stable anchors for terminology even when tools change.

* [arXiv: Computer Science (recent)](https://arxiv.org/list/cs/recent) — Preprints across software engineering and LLM research.
* [SWE-bench (GitHub)](https://github.com/princeton-nlp/SWE-bench) — Benchmark updates, scripts, and evaluation changes.
* [OWASP GenAI Security Project](https://genai.owasp.org/llm-top-10/) — Updates to OWASP LLM risk materials.
* [NIST AI RMF page](https://www.nist.gov/itl/ai-risk-management-framework) — RMF updates and related companion resources.

---

## Glossary

* **Asynchronous agent** — An agent that runs independently and coordinates via messages or persisted state.
* **HITL (Human-in-the-loop)** — Human review/approval integrated into an automated workflow.
* **Large-scale change (LSC)** — Coordinated change spanning many files, modules, or repositories.
* **Agent orchestration** — Control logic coordinating multiple agents, tools, and state transitions.
* **Trace** — Correlated telemetry describing a distributed workflow execution.
* **Provenance** — Evidence describing how an artifact was produced and by whom.
* **SBOM** — Software Bill of Materials describing components and dependencies.
* **VEX** — Vulnerability Exploitability eXchange indicating vulnerability impact status.
* **SLSA** — Supply-chain Levels for Software Artifacts integrity framework.
* **MCP** — Model Context Protocol for tools/resources integration.
* **Transformation recipe** — Declarative or typed specification describing a code change shape.

---

## References and Link Count Summary

* [Resource Guide Creating Resource (internal)](sandbox:/mnt/data/%23%20Resource%20Guide%20Creating%20Resource.md) — Internal template for consistent guide structure. 
* Link count summary (bulleted links):

  * Categories: 8 × (3 category links + 2 subcategories × 3 links) = 72 links
  * Cross-Cutting Themes: 5 links
  * Keeping Current: 4 links
  * Internal reference: 1 link
  * **Total: 82 links**
