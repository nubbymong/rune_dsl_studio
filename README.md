<p align="center">
  <img src="https://raw.githubusercontent.com/nubbymong/rune_dsl_studio/main/marketplace/hero-banner.png" alt="Rune DSL Studio — a complete workbench for Rune DSL" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/VS%20Code-1.125%2B-4ea3f0" alt="VS Code 1.125+">
  <img src="https://img.shields.io/badge/Open%20VSX-compatible-79DEBD" alt="Open VSX compatible">
  <img src="https://img.shields.io/badge/license-Apache%202.0-C3E455" alt="Apache 2.0">
</p>

# Rune DSL Studio

**A complete workbench for Rune DSL** — model, build, and validate FINOS CDM, DRR, FpML, or any Rune DSL domain model, end to end inside your editor.

Rune DSL Studio takes a `.rosetta` model from first type declaration to running, validated output: author and explore models with full language support, build them with one click — no Maven setup, no toolchain wrangling — author, generate, and execute real Java natively in the editor, and validate against the model's own rules. Working with a reporting model like DRR? Studio carries you the rest of the way, from enriched events to regulatory wire formats. Modelling something else entirely? Rune DSL is domain-agnostic, and so is Studio.

> Any mention of "Rosetta" herein relates to the `.rosetta` file extension and the open-source Rune DSL language, not any licensed platform.

---

## Project / Build Manager

<p align="center">
  <img src="https://raw.githubusercontent.com/nubbymong/rune_dsl_studio/main/marketplace/shot-build.png" alt="The Master Build pipeline" width="88%">
</p>

Building a Rune DSL model normally means hand-managing a complex Maven build — wiring poms, classpaths, and toolchain jars before you can compile a line. Studio removes all of it: create a project from a model source (FINOS CDM, DRR, or your own) and Studio wraps it in a **managed project** with full build management. DRR is a private model, so building it requires a GitHub account with **rosetta-models** authorisation. The **Master Build** downloads the model, compiles the Rosetta DSL **to its Java implementation**, extracts the toolchain, and indexes everything for navigation and search — a nine-step pipeline with a live build log, named steps, and javac diagnostics surfaced straight into the Problems panel.

- **Environment checklist** — Java, Maven, Python, GitHub sign-in and model access verified up front, with one-click fixes and expandable detail drawers for anything that fails.
- **Incremental rebuilds** — edit, override, and rebuild; only what changed is recompiled.
- **Vendor overrides done properly** — unlock a read-only vendored file and Studio creates an `override namespace` copy in your own tree; the vendor base is never touched, the Modifications panel tracks every change with live status pills, and deleting an override heals back to base.

## Coding Suite

<p align="center">
  <img src="https://raw.githubusercontent.com/nubbymong/rune_dsl_studio/main/marketplace/shot-code.png" alt="Rune DSL language support" width="88%">
</p>

Full `.rosetta` language support across the whole model — not just your open file:

- **Syntax highlighting, autocomplete, and hover** documentation for types, enums, functions, and annotations.
- **Namespace tree** — the entire model organised by namespace, with per-file categories and instant navigation.
- **Workspace-wide search and symbol indices** covering thousands of model files, powering go-to, outline, and reference lookups.
- **Live validation** — problems surface as you type, with a warm background toolchain keeping generation and validation responsive in seconds, not JVM cold starts.
- **Native Java, first-class** — generate a model's Java implementation and navigate it as real source, and author your own custom Java against the model; Studio compiles both with the model's own toolchain — no separate Java project to wire up.

## Rune DSL Copilot — Multi-Model AI Integration

![Coming Q3 2026](https://img.shields.io/badge/Coming-Q3%202026-d8b25a)

Deep integration with your AI coding assistant — GitHub Copilot and Claude Code among the options under evaluation — is in development: page and context awareness inside Studio's surfaces, guided setup assistance, and model-aware coding assistance that understands Rune DSL semantics, not just syntax.

## Model Visualisation

<p align="center">
  <img src="https://raw.githubusercontent.com/nubbymong/rune_dsl_studio/main/marketplace/shot-graph.png" alt="3D type graph" width="88%">
</p>

A Three.js-powered type relationship explorer: anchor on the model's root type and watch the structure expand outward — types, attributes, enums, inheritance, and cross-namespace references, navigable in three dimensions. The same graph anchors inside RuneBooks, so you can see a type's shape the moment you author it.

## RuneBook — an interactive model notebook

<p align="center">
  <img src="https://raw.githubusercontent.com/nubbymong/rune_dsl_studio/main/marketplace/shot-runebook.png" alt="The RuneBook editor" width="88%">
</p>

RuneBooks are interactive notebooks for **prototyping and learning Rune DSL and its Java integration points** — author a type, generate its Java, run it, and validate it in one living document you work in directly. Your own **custom Java** and the toolchain's **generated Java** sit side by side, clearly distinguished:

- **Rune layer** — author `.rosetta` source with syntax highlighting, autocomplete, and per-file helper documentation.
- **Java layer** — one click generates real Java through the official toolchain; the **generated** classes open as read-only reference right beside your own editable **custom** Java, so the boundary between what the model produces and what you write is always clear.
- **Run it** — execute functions with parameter input, run full custom pipelines against wired sample data, build object instances interactively, and store them as reusable named inputs.
- **Validate** — run the type validator, cardinality checks, and condition rules with a detailed results view.
- **Templates** — start from pre-built templates with guided steps in the sidebar.

## Training

![Coming 24 June 2026](https://img.shields.io/badge/Coming-24%20June%202026-d8b25a)

<p align="center">
  <img src="https://raw.githubusercontent.com/nubbymong/rune_dsl_studio/main/marketplace/shot-training.png" alt="The DRR training course (coming 24 June 2026)" width="88%">
</p>

The first course — **CDM & DRR end to end** — **arrives in Studio on 24 June 2026**, with more to follow. A full four-module, hands-on walkthrough from first enum to regulator-ready XML:

| Module | What you build |
|---|---|
| **1 · Modelling source data** | Legacy Bank's outbound trade message — types, enums, nesting, inheritance — into a 12-file source schema |
| **2 · Transforming to CDM** | Ingest, fetch, and translate functions plus a Java pipeline producing a validated CDM `WorkflowStep` |
| **3 · The DRR ReportableEvent** | Regulatory enrichment (LEI · MIC · UPI), a real vendor-DRR patch, and a finalised `ReportableEvent` |
| **4 · Reporting to the Regulators** | Per-regime fan-out, three transaction reports (ESMA EMIR REFIT, CFTC Part 45/43), and projection to ISO 20022 and DTCC RDS XML |

Every lesson runs against the real toolchain — real builds, real validation, deliberate-break exercises, and a guided sidebar that tracks your progress.

## Execution Harness

![Coming Q3 2026](https://img.shields.io/badge/Coming-Q3%202026-d8b25a)

<p align="center">
  <img src="https://raw.githubusercontent.com/nubbymong/rune_dsl_studio/main/marketplace/shot-harness.png" alt="The DRR execution harness (coming Q3 2026)" width="78%">
</p>

Run whole-model regression pipelines at scale: ingest event batches, validate ReportableEvents, generate and validate transaction reports per regime, and project to XML — with live dashboards for throughput and findings, per-stage validation rollups, and top-exception triage that takes you straight to what broke.

---

## Getting started

1. **Install** the extension and open the **Rune DSL Studio** sidebar.
2. The welcome view checks your environment — Java 21, Maven, Python 3.10+, GitHub sign-in — with one-click fixes for anything missing.
3. **Create a project** from a model source and run the **Master Build** (first build downloads and compiles the full model — allow a few minutes).
4. Open a `.rosetta` file, a `.runebook`, the graph view — or start the training course from the Training panel.

### Requirements

| | |
|---|---|
| **Editor** | VS Code 1.125+ — or compatible editors (VSCodium, Gitpod, Eclipse Theia) via Open VSX |
| **Java** | JDK 21 — the build toolchain enforces 21.x (17 and 22+ are not supported) |
| **Maven** | 3.6.3+ — a managed Maven 3.9.8 is downloaded automatically if yours is older or missing |
| **Python** | 3.10+ |
| **GitHub** | Signed in for model downloads |

## Feedback & support

All feedback flows through the app itself — the **feedback icon** in the Studio sidebar or the **`Rune DSL: Send Feedback…`** command. Choose **Bug**, **Suggestion**, or **Technical Issue**; diagnostics attach automatically so we can act on it, and your GitHub login rides along so we can follow up.

Releases and release notes: [github.com/nubbymong/rune_dsl_studio](https://github.com/nubbymong/rune_dsl_studio)

### Privacy & data

Rune DSL Studio runs entirely on your machine and collects **no background telemetry**. The only outbound data is **user-initiated**: when you submit feedback, the report — with the diagnostics you can review first, plus any optional screenshot and email you attach — is sent over HTTPS via a maintainer-operated relay (a Cloudflare Worker) and raised as an issue in a **private** repository visible only to the maintainer; it is never published publicly. Those diagnostics are limited to your plugin / VS Code / OS versions, environment-check results, and an anonymised (hashed) workspace identifier so related reports can be correlated — never file contents or model data. GitHub authentication uses VS Code's native OAuth; any local secrets the extension stores are held in the editor's `SecretStorage`. The bundled MCP server exposes read-only model-grounding tools to a locally-configured AI client and makes no autonomous network calls.

## Attribution & licensing

**License:** Rune DSL Studio is licensed under **Apache 2.0**.

So that projects build without external toolchain setup, this extension bundles the open-source [Rune DSL](https://github.com/finos/rune-dsl) build toolchain (~120 third-party Java components). These are redistributed **unmodified** under their own licenses — principally **Apache 2.0** (Jackson, Guava, Apache Commons, SnakeYAML, …) and the **Eclipse Public License (EPL)** (Eclipse EMF, Xtext, LSP4J, JDT), with a small number of MIT, BSD, and CDDL components. Every bundled component and its license is enumerated in the `THIRD-PARTY-NOTICES` file that ships with the extension and the [source repository](https://github.com/nubbymong/rune_dsl_studio); a CycloneDX SBOM and an OSV CVE scan — with accepted-risk notes for the build-time-only advisories — are published alongside it.

**Related projects:** [FINOS](https://www.finos.org/) · [Rune DSL](https://github.com/finos/rune-dsl) · [FINOS Common Domain Model (CDM)](https://github.com/finos/common-domain-model) · Digital Regulatory Reporting (DRR).

## Disclaimer

This extension is provided "as-is" without warranty of any kind. Evaluate suitability before use in production environments.

Rune DSL Studio is an independent, community-built extension. It is not affiliated with, endorsed by, or sponsored by FINOS, ISDA, Regnosys, or any other organisation.

**"Rosetta".** *Rosetta* is a commercial SaaS modelling platform and a trademark of Regnosys. This extension is not affiliated with, endorsed by, or connected to Regnosys or the Rosetta platform. References to "Rosetta" here — the `.rosetta` file extension, historical package/classpath names, and language tooling — refer solely to the open-source FINOS Rune DSL language (formerly named Rosetta DSL — hence package names such as `com.regnosys.rosetta.*`) and its file format, not the Regnosys product.

---

<p align="center"><sub>Built with the Rune DSL toolchain · © Nicholas Moger</sub></p>
