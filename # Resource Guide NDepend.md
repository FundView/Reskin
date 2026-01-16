# Resource Guide: NDepend

## Overview

NDepend is a commercial static analysis and code intelligence product for .NET codebases that combines metrics, rule-based findings, dependency visualization, and reporting. Its analysis model is queryable, using CQLinq (a LINQ-flavored query language) to express queries, rules, dashboards, and quality gates. In addition to an interactive Windows UI (Visual Studio extension and standalone), NDepend supports CI/CD reporting through a console runner, an Azure DevOps extension, and a GitHub Action. NDepend is commonly used to quantify maintainability, surface architectural risks, and track change over time in complex or legacy codebases. Outputs typically include interactive web reports, dependency diagrams (graph and matrix), and trend charts designed for team visibility. This guide curates primary documentation plus a small set of independent commentary for additional perspective.

## How to Use This Guide

The table of contents is organized by common adoption concerns: rules and queries, metrics and gates, architecture visualization, CI/CD integration, and governance topics. Link descriptors indicate each source’s focus so primary documentation can be prioritized before secondary commentary.

## Source Quality Notes

First-party NDepend documentation and marketplace listings are treated as authoritative for supported platforms, integration surfaces, and feature semantics. Third-party posts are included only as supplemental context and may describe older releases or organization-specific setups. Social-media-first sources are excluded; video materials are referenced via NDepend’s own documentation hub where possible. Standards and ecosystem references (e.g., NIST SSDF, SARIF) are linked to originating bodies or major vendor documentation; this guide is informational and not legal, security, or compliance advice.

## Table of Contents

* [Official Product Orientation](#official-product-orientation)

  * [Product overview and evaluation artifacts](#product-overview-and-evaluation-artifacts)
  * [Documentation entry points and use cases](#documentation-entry-points-and-use-cases)
  * [Release history and change awareness](#release-history-and-change-awareness)
* [CQLinq, Rules, and Issue Management](#cqlinq-rules-and-issue-management)

  * [CQLinq and the code model](#cqlinq-and-the-code-model)
  * [Default rules and rule browsing](#default-rules-and-rule-browsing)
  * [Custom rules and shared rule sets](#custom-rules-and-shared-rule-sets)
  * [Issue suppression and noise control](#issue-suppression-and-noise-control)
* [Metrics, Quality Gates, and Technical Debt](#metrics-quality-gates-and-technical-debt)

  * [Metrics definitions and catalogs](#metrics-definitions-and-catalogs)
  * [Quality gates and gate strategy](#quality-gates-and-gate-strategy)
  * [Technical debt concepts](#technical-debt-concepts)
  * [Trends](#trends)
* [Architecture and Visualization](#architecture-and-visualization)

  * [Dependency graph](#dependency-graph)
  * [Dependency structure matrix](#dependency-structure-matrix)
  * [Metric view and treemap](#metric-view-and-treemap)
  * [Code diff and build comparison](#code-diff-and-build-comparison)
* [CI/CD and Integrations](#cicd-and-integrations)

  * [Visual Studio extension and navigation](#visual-studio-extension-and-navigation)
  * [Azure DevOps and TFS extension](#azure-devops-and-tfs-extension)
  * [GitHub Action and marketplace](#github-action-and-marketplace)
  * [Console and analysis/report customization](#console-and-analysisreport-customization)
  * [Imports: coverage, Roslyn, ReSharper, SARIF](#imports-coverage-roslyn-resharper-sarif)
* [Extensibility and API Surface](#extensibility-and-api-surface)

  * [NDepend API entry points](#ndepend-api-entry-points)
  * [OSS Power Tools and examples](#oss-power-tools-and-examples)
  * [Related repositories](#related-repositories)
* [Licensing, Privacy, and Support](#licensing-privacy-and-support)

  * [Editions and purchasing](#editions-and-purchasing)
  * [Privacy, connectivity, and confidentiality](#privacy-connectivity-and-confidentiality)
  * [Support and community Q&A](#support-and-community-qa)
* [Independent Commentary](#independent-commentary)
* [Cross-Cutting Themes](#cross-cutting-themes)
* [Keeping Current](#keeping-current)
* [Glossary](#glossary)
* [References and Link Count Summary](#references-and-link-count-summary)

---

## Official Product Orientation

NDepend’s first-party pages establish what the product is meant to do, where it runs, and what outputs it produces. These sources also provide “look at the result” artifacts such as sample reports, which can help align expectations without relying on third-party screenshots. For teams comparing tools, these pages are a baseline for terminology and supported integration points.

### Product overview and evaluation artifacts

NDepend’s website describes .NET static analysis focused on reporting, diagrams, and query-driven checks. Sample reports are concrete artifacts for understanding the reporting surface and data visualizations. The feature index provides a map for deeper documentation.

* [NDepend (homepage)](https://www.ndepend.com/) — Product overview and links to current resources.
* [Product Features](https://www.ndepend.com/features/) — Feature index across analysis, reports, and integrations.
* [Sample Web Reports](https://www.ndepend.com/sample-reports/) — Public examples of generated reports and diagrams.
* [Interactive Web Report feature page](https://www.ndepend.com/features/integration-report) — Report outputs, sharing model, and integration framing.
* [Download NDepend](https://www.ndepend.com/download) — Trial download entry and current build identifier.

### Documentation entry points and use cases

NDepend documentation is organized around getting started, feature deep-dives, and focused use cases. The walkthrough video hub is a first-party index that avoids reliance on external video platforms. The use-cases index acts as a directory for role- and scenario-specific documentation pages.

* [Getting Started on Windows](https://www.ndepend.com/docs/getting-started-with-ndepend) — Entry documentation for Windows UI workflow.
* [Getting Started on Linux and macOS](https://www.ndepend.com/docs/getting-started-with-ndepend-linux-macos) — Cross-platform console getting-started entry documentation.
* [NDepend Walkthrough Videos](https://www.ndepend.com/docs/videos) — First-party video index and walkthrough catalog.
* [Introduction to NDepend Use Cases](https://www.ndepend.com/docs/ndepend-use-cases) — Directory of scenario-based documentation pages.

### Release history and change awareness

Release notes help reconcile third-party examples with current behavior and supported platforms. “What’s new” pages highlight feature bundles and major additions across versions. These are the most reliable sources for identifying recently added capabilities.

* [Release Notes](https://www.ndepend.com/release-notes) — Dated release history and notable changes.
* [What’s New](https://www.ndepend.com/whatsnew) — Major version highlights and feature themes.

---

## CQLinq, Rules, and Issue Management

NDepend’s “code as data” positioning comes from a queryable code model and the ability to express checks as CQLinq rules. This section focuses on core references for the query language, default rule inventory, and operational management of findings. It also includes sources for sharing rule sets across projects and validating checks in the IDE.

### CQLinq and the code model

CQLinq is presented as a LINQ experience over an internal code model, enabling queries that return structured results and drive rules. Primary references cover conceptual framing as well as syntax and execution characteristics. These sources also help clarify where CQLinq complements other analyzer ecosystems.

* [Query Code with C# LINQ](https://www.ndepend.com/docs/query-code-with-csharp-linq) — Conceptual introduction to CQLinq and querying.
* [CQLinq Syntax](https://www.ndepend.com/docs/cqlinq-syntax) — Syntax reference and NDepend-specific extensions.

### Default rules and rule browsing

NDepend ships a substantial default ruleset organized into categories such as architecture, code smells, coverage, and security. The rules explorer provides a browsable inventory of included rules, quality gates, and trend metrics. This inventory can be used as a taxonomy for discussing rule coverage and scope.

* [NDepend Rules Explorer](https://www.ndepend.com/default-rules/NDepend-Rules-Explorer.html) — Browse default rules, gates, and trends.
* [CQLinq feature page](https://www.ndepend.com/features/cqlinq) — Product framing of rules and custom queries.

### Custom rules and shared rule sets

Custom rules are represented as CQLinq queries and can be shared across projects through rule files. Documentation also covers declaring rules in source code and validating rule/gate status within Visual Studio. These references are most relevant where organizations standardize checks across repositories.

* [Write Your Own Code Rules](https://www.ndepend.com/docs/write-your-own-code-rules) — Rule authoring resources and starter references.
* [NDepend Rule File (.ndrules)](https://www.ndepend.com/docs/ndepend-rule-file) — Shared rule files and cross-project reuse model.
* [Declare CQLinq rules in source code](https://www.ndepend.com/docs/declare-cqlinq-rules-in-csharp-or-vbnet-code) — In-code declaration approach and attributes.
* [Validating CQLinq code rules in Visual Studio](https://www.ndepend.com/docs/validating-cqlinq-code-rules-in-visual-studio) — IDE surfacing of rule and gate status.

### Issue suppression and noise control

Issue suppression supports handling false positives, special cases, or deliberate exceptions to a policy. Suppression documentation clarifies mechanisms and scoping for suppressing findings. Storage documentation helps locate where rules and results are persisted for auditability and portability.

* [Suppress Issues](https://www.ndepend.com/docs/suppress-issues) — Suppression mechanisms, scope, and troubleshooting notes.
* [NDepend Storage and Files](https://www.ndepend.com/docs/ndepend-storage-and-files) — Where project, rules, and results are stored.

---

## Metrics, Quality Gates, and Technical Debt

NDepend includes a large set of code metrics and uses query-driven quality gates to represent pass/warn/fail checks. This section collects key references for metric definitions, gate semantics, and debt modeling. Trend and baseline concepts are included because they connect metrics and gates to change over time.

### Metrics definitions and catalogs

The metrics documentation enumerates metric names and explains their meaning at different scopes. The metrics feature page provides a product-oriented lens useful for non-developer stakeholders. NDepend tips pages offer curated topical references anchored in specific metrics.

* [Code Metrics Definitions](https://www.ndepend.com/docs/code-metrics) — Definitions for 100+ metrics across scopes.
* [Code Metrics (feature overview)](https://www.ndepend.com/code-metrics) — High-level positioning and metric examples.
* [NDepend Tips](https://www.ndepend.com/tips) — Curated notes tied to specific metrics.

### Quality gates and gate strategy

Quality gates are described as query-driven criteria with PASS/WARN/FAIL outcomes, distinct from rules that produce issue lists. Documentation clarifies how gate outcomes can be surfaced in CI environments and connected to dashboards and reports. The DevOps strategy page frames how gates can be organized and monitored.

* [Quality Gates and Build Failure](https://www.ndepend.com/docs/quality-gates) — Semantics of gates versus rules.
* [DevOps Quality Gate Strategy](https://www.ndepend.com/docs/devops-quality-gate-strategy) — Strategy framing for gate-driven CI usage.
* [Quality Gates feature page](https://www.ndepend.com/features/quality-gates) — Product framing and gate customization context.

### Technical debt concepts

NDepend includes a technical-debt estimation model, including “annual interest” and debt rating concepts. The technical debt documentation describes how these ideas appear in the product and reports. Debt also appears as a major theme across the metrics documentation.

* [Technical Debt](https://www.ndepend.com/docs/technical-debt) — Debt estimation model and “annual interest” framing.
* [Code Metrics Definitions (debt sections)](https://www.ndepend.com/docs/code-metrics) — Debt metrics relationship to other metrics.

### Trends

Trend monitoring connects repeated analyses to charts and historical comparisons. Documentation explains trend stores, predefined trend metrics, and query-driven extension points. The monitoring use-case page provides a compact overview of which trends are typically tracked.

* [Trend Monitoring](https://www.ndepend.com/docs/trend-monitoring) — Trend store concepts and trend chart outputs.
* [Monitor Code Trend](https://www.ndepend.com/docs/monitor-code-trend) — Trend monitoring use-case framing and examples.
* [NDepend OSS Power Tools (trend tools)](https://www.ndepend.com/docs/ndepend-oss-power-tools) — Examples for programmatic trend access.

---

## Architecture and Visualization

NDepend’s visualization tooling is used to understand structure, coupling, and layering patterns in complex systems. The dependency graph and dependency structure matrix (DSM) provide complementary representations of relationships across assemblies, namespaces, types, and members. Metric view and treemap visualizations aim to surface hotspots and concentration of complexity, debt, or coverage.

### Dependency graph

Dependency graph materials focus on navigation, scaling to large graphs, and focused graph modes (e.g., callers/callees, inheritance). The use-case page “Ramp Up” frames why dependencies are central for onboarding and architecture comprehension. The feature page provides an evaluation-friendly summary of the dependency graph’s intent.

* [Dependency Graph documentation](https://www.ndepend.com/docs/visual-studio-dependency-graph) — Graph concepts, navigation, scalability, and exports.
* [Ramp Up in a New Code Base](https://www.ndepend.com/docs/ramp-up-in-a-new-code-base) — Use-case framing for dependency-based exploration.
* [Visual Studio dependency graph (feature page)](https://www.ndepend.com/visual-studio-dependency-graph) — Summary of dependency graph positioning.

### Dependency structure matrix

DSM documentation explains the matrix representation and its role in identifying cycles, clustering, and high-level component coupling. The “Improve architecture” use case provides additional framing for entanglement and maintainability concerns. These sources are central for NDepend’s architectural analysis narrative.

* [Dependency Structure Matrix (DSM)](https://www.ndepend.com/docs/dependency-structure-matrix-dsm) — DSM concepts, mapping to graph semantics.
* [Improve Code Architecture](https://www.ndepend.com/docs/improve-code-architecture) — Use-case framing for entanglement analysis.

### Metric view and treemap

The treemap and metric view features visualize code elements sized and colored by metrics. This is often used to locate hotspots and clusters, such as high complexity concentration or low coverage. Coverage monitoring documentation provides additional examples tied to metric visualizations.

* [Treemap visualization of code metrics](https://www.ndepend.com/docs/treemap-visualization-of-code-metrics) — Metric view concepts and treemap perspectives.
* [Monitor Test Coverage](https://www.ndepend.com/docs/monitor-test-coverage) — Coverage visualization examples in treemap context.

### Code diff and build comparison

NDepend includes build comparison and code diff capabilities that focus attention on changes since a chosen baseline. Documentation covers IDE-facing comparisons and report-oriented diffs. The refactoring impact analysis page provides broader context on using diffs and dependency views when planning change.

* [Code Diff in Visual Studio](https://www.ndepend.com/docs/code-diff-in-visual-studio) — IDE-oriented baseline comparisons and views.
* [Reporting Code Diff](https://www.ndepend.com/docs/reporting-code-diff) — Report-oriented diff views and outputs.
* [Refactoring Impact Analysis](https://www.ndepend.com/docs/refactoring-impact-analysis) — Change-impact framing for refactoring planning.

---

## CI/CD and Integrations

NDepend supports interactive workflows and CI/CD reporting, with first-party integration points for Visual Studio, Azure DevOps/TFS, and GitHub Actions. Console tooling is a bridge for cross-platform CI systems outside those integrations. This section also links documentation on importing coverage and external analyzer outputs into the NDepend model.

### Visual Studio extension and navigation

The Visual Studio Marketplace listing summarizes the extension’s scope and supported versions, while documentation explains how NDepend’s UI integrates into the IDE. Navigation docs describe how code elements can serve as entry points to queries and visualizations. These sources describe the interactive workflow surface without relying on environment-specific configurations.

* [NDepend Visual Studio Marketplace listing](https://marketplace.visualstudio.com/items?itemName=PatrickSmacchia.NDepend) — Extension overview and supported Visual Studio versions.
* [Visual Studio code navigation](https://www.ndepend.com/docs/code-navigation) — IDE navigation menus and query entry points.

### Azure DevOps and TFS extension

The Azure DevOps integration documentation describes how analysis outputs are exposed in dashboards and stored as build artifacts. It also includes statements about whether analysis uses external endpoints or keeps data server-side. The marketplace listing provides integration metadata.

* [Azure DevOps / TFS integration docs](https://www.ndepend.com/docs/azure-devops-tfs-vsts-integration-ndepend) — Build task behavior and dashboard surfaces.
* [Azure DevOps Marketplace listing](https://marketplace.visualstudio.com/items?itemName=ndepend.ndependextension) — Marketplace metadata for the extension.

### GitHub Action and marketplace

NDepend’s GitHub Action documentation includes confidentiality statements and references to the marketplace entry. The marketplace page and the action repository provide visibility into wrapper implementation and change history. These sources are the main reference set for GitHub-based CI integration.

* [NDepend GitHub Action documentation](https://www.ndepend.com/docs/ndepend-github-action) — Action behavior and confidentiality statement.
* [GitHub Marketplace: NDepend Action](https://github.com/marketplace/actions/ndepend) — Marketplace listing and parameters overview.
* [ndepend/ndepend-action](https://github.com/ndepend/ndepend-action) — Action source repository and releases.

### Console and analysis/report customization

Console documentation details supported arguments and distinguishes between Windows console and the multi-OS runner. Analysis/report customization documentation covers options related to saving results, trend logging, and coverage file ingestion. These references are relevant when NDepend is used in CI systems outside first-party extensions.

* [NDepend Console command line arguments](https://www.ndepend.com/docs/ndepend-console) — Switch reference for console and MultiOS runner.
* [Customize NDepend Analysis and Report](https://www.ndepend.com/docs/ndepend-analysis) — Analysis options, report outputs, and data paths.

### Imports: coverage, Roslyn, ReSharper, SARIF

NDepend can incorporate coverage results and import findings from Roslyn analyzers and ReSharper code inspections. Documentation emphasizes that imported issues become first-class citizens in reporting and queries. Because SARIF is a common interchange format, GitHub’s SARIF expectations are included as ecosystem context.

* [Code Coverage Results Import](https://www.ndepend.com/docs/code-coverage) — Coverage import formats and report surfaces.
* [Roslyn analyzer issue import](https://www.ndepend.com/docs/roslyn-analyzer-issue-import) — Import and reporting of Roslyn analyzer issues.
* [ReSharper inspection issue import](https://www.ndepend.com/docs/resharper-code-inspection-issue-import) — Import of ReSharper inspections and SARIF storage.
* [SARIF support for code scanning](https://docs.github.com/en/code-security/code-scanning/integrating-with-code-scanning/sarif-support-for-code-scanning) — GitHub SARIF subset and ingestion requirements.

---

## Extensibility and API Surface

NDepend exposes a .NET API that can be used to build custom tools, automate reporting, and integrate NDepend’s code model into internal workflows. The Power Tools documentation provides a curated set of open-source examples illustrating API usage patterns. This section links the primary API references and the first-party GitHub organization.

### NDepend API entry points

The API quickstart clarifies how the API relates to the rest of NDepend’s assemblies and what it exposes. It also points to default queries and rules as concrete examples over the code model. These references are most relevant for custom reporting, automation, and internal tooling.

* [NDepend API Quickstart](https://www.ndepend.com/docs/ndepend-api-quickstart) — API scope, entry patterns, and constraints.

### OSS Power Tools and examples

The OSS Power Tools page lists open-source utilities published by the NDepend team and indicates which API areas they use. It serves as a curated “sample set” for seeing API usage patterns across tasks such as trend access and analysis result manipulation. This is the primary first-party reference for open-source examples.

* [NDepend OSS Power Tools](https://www.ndepend.com/docs/ndepend-oss-power-tools) — Power tools catalog and API coverage mapping.
* [NDepend Resources](https://www.ndepend.com/docs/ndepend-resources) — Posters, slides, and reference materials list.

### Related repositories

The NDepend GitHub organization provides a single place to browse first-party repositories related to integrations and OSS components. Repository content can be useful when evaluating integration maintenance and release cadence. This index also helps locate the source for the GitHub Action wrapper.

* [NDepend on GitHub](https://github.com/ndepend) — First-party repositories and OSS components list.

---

## Licensing, Privacy, and Support

NDepend’s licensing is organized around editions aligned to developer use, build machines, and CI platform integrations. Privacy and connectivity topics matter for teams with restrictive environments, especially for CI runners and server-hosted dashboards. Official FAQ and support pages are the authoritative references for these questions.

### Editions and purchasing

The editions page compares licensing models across Developer, Build Machine, Azure DevOps/TFS extension, and GitHub Action. Purchase and renewal/discount pages provide commercial context relevant to procurement. These sources should be relied on over third-party pricing summaries.

* [Editions comparison](https://www.ndepend.com/editions) — Developer vs Build Machine vs DevOps vs Action.
* [Purchase NDepend](https://www.ndepend.com/purchase) — Purchase entry point and SKU framing.
* [Renewal and discount policy](https://www.ndepend.com/renewal-and-discount) — Renewal rates, volume, and multi-year discounts.
* [End-User License Agreement](https://www.ndepend.com/eula) — License terms and subscription details.

### Privacy, connectivity, and confidentiality

NDepend provides a privacy policy for the website and includes integration-specific statements about data handling. Azure DevOps integration docs include discussion of where analysis occurs and where results are stored. GitHub Action documentation includes confidentiality notes relevant to repository code and generated reports.

* [Privacy Policy](https://www.ndepend.com/privacy-policy) — Site privacy policy and stated data handling.
* [Azure DevOps integration (data locality)](https://www.ndepend.com/docs/azure-devops-tfs-vsts-integration-ndepend) — Statements about analysis execution and data storage.
* [NDepend GitHub Action documentation](https://www.ndepend.com/docs/ndepend-github-action) — Confidentiality statement and report link behavior.
* [NDepend Manual Server Access](https://www.ndepend.com/manual) — Manual/offline server access and activation reference.
* [FAQ](https://www.ndepend.com/faq) — Operational, licensing, and connectivity clarifications.

### Support and community Q&A

NDepend’s official contact page is the primary support entry point. Stack Overflow provides a searchable public Q&A history; the tag info page clarifies scope and expectations. Community answers should be treated as non-authoritative unless confirmed by first-party docs.

* [Contact](https://www.ndepend.com/contact) — Official contact page for support and sales.
* [Stack Overflow tag: ndepend](https://stackoverflow.com/tags/ndepend/info) — Public Q&A tag description and navigation.

---

## Independent Commentary

Independent sources can add perspective on evaluation, adoption patterns, and integration narratives, but they often reflect the NDepend version current at publication. These references are included for breadth and historical context, and they should be checked against current release notes and first-party docs. The Hanselman posts are early but influential in the .NET community’s discourse about metrics and static analysis.

* [Analyzing Code Quality with LINQ and NDepend](https://fuqua.io/blog/2021/08/analyzing-code-quality-with-linq-and-ndepend/) — Practitioner discussion of CQLinq and query patterns.
* [Static Code Analysis with NDepend](https://www.dotnetnakama.com/blog/static-code-analysis-with-ndepend/) — Overview emphasizing reports and integrations.
* [Improving .NET code quality with NDepend](https://www.kristhecodingunicorn.com/post/dotnet-code-quality-with-ndepend/) — Narrative with Azure DevOps usage context.
* [Using NDepend Console](https://blog.ladeak.net/posts/ndepend-console) — Notes emphasizing console and pipeline execution.
* [NDepend report feature overview](https://anthonygiretti.com/2024/02/11/ndepend-is-the-must-have-tool-for-net-applications-discovering-the-report-feature-at-a-glance/) — Report-focused tour and practical impressions.
* [Helpful improvements in NDepend v2023.2](https://improveandrepeat.com/2024/01/helpful-improvements-in-ndepend-v2023-2/) — Commentary on release changes and platform support.
* [Exiting the Zone of Pain](https://www.hanselman.com/blog/exiting-the-zone-of-pain-static-analysis-with-ndepend) — Early review focusing on framing and usability.
* [Hanselminutes #163: Software Metrics](https://www.hanselman.com/blog/hanselminutes-podcast-163-software-metrics-with-patrick-smacchia) — Metrics interview with NDepend’s lead developer.
* [Managing Change with .NET Assembly Diff Tools](https://www.hanselman.com/blog/managing-change-with-net-assembly-diff-tools) — Historical discussion of diff tooling concepts.

---

## Cross-Cutting Themes

NDepend is often used within broader software quality and security programs where static analysis results feed governance, risk, and modernization discussions. Its security-oriented rules (including software composition analysis) can be contextualized within supply-chain vulnerability management and secure software development frameworks. When results are integrated with code scanning platforms, interchange formats such as SARIF can matter even if NDepend’s native web reports remain the primary artifact.

* [Software Composition Analysis (SCA)](https://www.ndepend.com/docs/software-composition-analysis) — NDepend SCA rules and advisory data source framing.
* [GitHub Advisory Database (NuGet view)](https://github.com/advisories?query=type%3Areviewed+ecosystem%3Anuget) — Reviewed advisories for NuGet ecosystem packages.
* [NuGet vulnerability auditing](https://learn.microsoft.com/en-us/nuget/concepts/auditing-packages) — Microsoft overview of NuGet vulnerability auditing.
* [NIST SSDF (SP 800-218)](https://csrc.nist.gov/pubs/sp/800/218/final) — Secure Software Development Framework practices reference.
* [OWASP SAMM](https://owaspsamm.org/) — Software security maturity model reference site.
* [Working with GitHub security advisories](https://docs.github.com/en/code-security/security-advisories) — GitHub advisory lifecycle and management concepts.
* [SARIF Home](https://sarifweb.azurewebsites.net/) — SARIF standard hub and version references.

---

## Keeping Current

NDepend’s capabilities evolve alongside Visual Studio and .NET releases, so release notes and “what’s new” summaries are the most reliable references for currency. The NDepend blog provides additional context on major features and broader .NET tooling topics. Marketplace listings and public repositories offer signals about integration-specific changes.

* [Release Notes](https://www.ndepend.com/release-notes) — Dated version history and notable changes.
* [What’s New](https://www.ndepend.com/whatsnew) — Major version highlights and feature themes.
* [NDepend Blog](https://blog.ndepend.com/) — Ongoing posts by NDepend team and guests.
* [NDepend Visual Studio Marketplace listing](https://marketplace.visualstudio.com/items?itemName=PatrickSmacchia.NDepend) — Extension metadata and update cadence signals.
* [ndepend/ndepend-action](https://github.com/ndepend/ndepend-action) — Action repository commits and releases.

---

## Glossary

* **Baseline** — A saved analysis snapshot used for diffs and trends.
* **CQLinq** — NDepend’s LINQ-based query language over a code model.
* **Dependency Graph** — A directed graph visualization of dependencies.
* **Dependency Structure Matrix (DSM)** — A matrix visualization of dependencies and cycles.
* **Issue** — A finding produced by a rule or imported analyzer.
* **Quality Gate** — A PASS/WARN/FAIL criterion expressed as a query.
* **Rule** — A query that typically produces issue instances.
* **SARIF** — Static Analysis Results Interchange Format (OASIS standard).
* **SCA** — Software Composition Analysis for dependency risk and vulnerabilities.
* **Technical Debt** — NDepend’s estimate of remediation effort and interest.
* **Trend Store** — Historical storage of metrics and gate results over builds.

---

## References and Link Count Summary

* External resource links: 91 total (83 unique).
* Internal table-of-contents anchors: 38.
* Total links in document (external + internal): 129.
