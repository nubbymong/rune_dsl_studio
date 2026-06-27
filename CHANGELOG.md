# Changelog

All notable changes to the Rune DSL VS Code Extension.

## [8.3.0] - 2026-06-27

### Changed
- **Designer panel reshaped** — the Java section is now two tabs, **Builder** and **Output**. Builder has a four-way sub-toggle for the generated Java files and run scripts; Output folds in the old Runner and Object views (the separate Pipeline and Object tabs are removed). The Sample Data pill is now a **Skeleton / Hardcoded** switch inside Builder, and **Add to Project** is a single dialog — Java only or Java plus run scripts, with a script name and a live destination-path preview. Each product lands in its own package under `java/src/custom/designer/<product>/` so multiple products coexist without class-name clashes. The standalone Export button is removed.
- **New project Location pre-fills** with the directory of your most recently used project.
- **Clearer Java build failures** — a failed Java compile now shows a plain-language summary naming the file(s) that failed and what to do, instead of raw compiler output.
- **DRR training access** is now a status-only pill (Unlocked / Locked); unlocking is via the command palette (*Rune DSL: Unlock ISDA Training…*).
- **Product tours scale at 1080p** — tour webviews fit the viewport when docked at 1080p instead of overflowing.
- **CDM override rebuilds** now regenerate the same Java navigation/hover data as DRR rebuilds (previously skipped for CDM, leaving it stale).
- **Consistent source exclusions** — excluded files are now skipped at every compile stage.

### Fixed
- **Failed-upgrade recovery** — a failed upgrade no longer leaves the project stuck needing File → Close Folder; a dialog offers retry / go-back. If an upgrade is interrupted (crash or window close), reopening the project re-prompts the upgrade.
- **False "stale" on CDM overrides** — an overridden file in a CDM project no longer shows as stale after a successful build.
- **Override build completion** — a successful override rebuild now ends with a clear "Build complete" line instead of trailing messages that looked like a failure.
- **CDM model-jar detection** — CDM and other non-DRR projects no longer show a spurious "master model jar not found" warning.
- **Exclusions** — exclusion-only rebuilds complete quickly; re-including a file triggers a real compile (was a false success); excluding a file removes its compiled output and navigation data.
- **Accurate build progress** — step indicators turn green only after all build work finishes.
- **Cancel on Select Model** returns you to the project create/open view.
- **Launch Project** focuses the Develop tab instead of switching to the editor.
- **Designer "Add to Project"** now produces Java that compiles in a real project build, with a notice when code is adapted; the dialog can be dismissed with Esc or click-outside.
- **Training note panel** can no longer be closed by an accidental click outside it.

## [8.1.2] - 2026-06-18

### Fixed
- Packaging fix: removed a stale compiled file left over from the screen-capture removal in 8.1.1. The feature, its dependencies, and native capture components were already gone in 8.1.1, but one now-unused compiled artifact still referenced them and has been removed from the package. No functional change — feedback and the training modal continue to attach images by paste, drop, or browse only.

## [8.1.1] - 2026-06-18

### Changed
- **Feedback screenshots are now attach/paste only.** The automatic VS Code window screen-capture was removed — feedback (and the training modal) take an image you drop, browse, or paste. This also drops the native capture components from the package and makes it considerably smaller.
- Clearer attribution of the bundled **FINOS** open-source **Rune DSL** build toolchain.

### Added
- `SUPPORT.md` — help and feedback channels.

## [8.1.0] - 2026-06-18

A major release. Rune DSL Studio becomes a complete, managed workbench — take a `.rosetta` model from first type declaration to running, validated Java without hand-wiring Maven.

### Added
- **Managed projects & one-click Master Build** — create a project from a model source (FINOS CDM, DRR, or your own) and Studio wraps it in a managed Maven build. One click downloads the model, compiles the Rosetta DSL to its Java implementation, extracts the toolchain, and indexes everything for navigation and search — a nine-step pipeline with a live build log, named steps, and `javac` diagnostics surfaced straight into the Problems panel. No hand-managed poms, classpaths, or toolchain jars.
- **Studio workbench** — a single home for your projects, RuneBooks, type graph, and help. Recent projects show model, version, RuneBook count, and build status at a glance, with an environment checklist (Java 21, Maven, Python, GitHub sign-in, model access) and one-click fixes for anything missing.
- **Native Java, first-class** — generate a model's Java implementation and navigate it as real source, and author your own custom Java against the model; Studio compiles both with the model's own toolchain. Generated (read-only) and custom (editable) Java sit side by side, so the boundary between what the model produces and what you write is always clear.
- **Type graph** — explore a type and its neighbourhood as an interactive graph; node shapes encode the kind, edges encode mandatory vs optional. Export to Mermaid.
- **RuneBook editor** — interactive notebooks for prototyping and learning Rune DSL and its Java integration points: author a type, generate its Java, run it, and validate it in one living document. Start from a template.
- **Regulatory reporting (DRR)** — for reporting models, carry data from enriched events through to regulatory wire formats. DRR access uses your GitHub account's rosetta-models authorisation.
- **Light theme** — full light-mode support across the workbench, type graph, and RuneBook, following your VS Code theme.
- **In-app feedback** — send feedback with the diagnostics you can review first, plus an optional screenshot and email, over an encrypted relay. Nothing is published publicly.
- **Open VSX** — available for VSCodium, Gitpod, and Eclipse Theia.

### Coming soon (in-product previews)
- **Guided training** — a hands-on CDM & DRR course inside Studio (24 June 2026).
- **Execution Harness** — run reporting pipelines and inspect their results (Q3 2026).
- **Rune DSL Copilot** — model-aware AI integration; GitHub Copilot and Claude Code are among the options under evaluation (Q3 2026). AI features ship disabled at launch.

### Changed
- Requires **VS Code 1.125+**, **Java 21** (the toolchain enforces 21.x; 17 and 22+ are not supported), and **Maven 3.6.3+** (a managed Maven is downloaded automatically if yours is older or missing).
- The bundled build toolchain (~120 third-party Java components) is redistributed unmodified under its own licenses; every component is enumerated in `THIRD-PARTY-NOTICES`, with a CycloneDX SBOM and an OSV CVE scan published alongside it.

### Privacy & security
- Runs entirely on your machine with no background telemetry; the only outbound data is user-initiated feedback. GitHub authentication uses VS Code's native OAuth, and any local secrets are held in the editor's SecretStorage. Requires a trusted workspace.

## [7.0.4] - 2026-04-14

### Fixed
- Python build scripts now included in VSIX (was excluded by .vscodeignore)
- Fixed ingest template validator namespace resolution in plugin

## [7.0.0] - 2026-04-14

### Added
- **RuneBook Editor** — Interactive notebook-style editor for Rune DSL types with Rune, Java, and Graph layers
- **Code Generation** — Generate Java from Rune DSL source via official toolchain JAR
- **Validation** — Three validator types: Validator, TypeFormatValidator, OnlyExistsValidator with detailed results modal
- **3D Graph Visualization** — Three.js type relationship explorer integrated into RuneBook editor
- **Template System** — Create RuneBooks from pre-built templates (Basic Type, JSON Ingest)
- **Build Manager** — Download and manage Rune toolchains from GitHub releases
- **Template Guide** — Contextual help panel when working with templates
- **Default Instances** — Pre-populated test data from templates
- **Ingestor Support** — JSON/CSV/XML ingestion with sample trade data

### Changed
- Complete architecture rewrite: genuine RuneBook engine running in webviews with adapter layer
- Replaced D3 graph visualization with Three.js 3D renderer
- Project Manager sidebar redesigned with environment status, build management, and RuneBook list

### Removed
- D3-based Graph Explorer (replaced by Three.js)
- Mermaid diagram export
- Inline ASCII type diagrams on hover
- Rune Explorer sidebar tree (replaced by Project Manager)
- Offline LSP mode with pre-built indices

## [6.0.2] - 2026-01-23

- Added language for Open VSX registry

## [6.0.0] - 2026-01-23

### Added
- Graph Explorer Panel with D3 visualization
- Inline Type Diagrams on hover
- Mermaid Export for documentation
- CodeLens Graph Links
- Rune Explorer Sidebar
- Offline Mode with pre-built indices

### Changed
- Complete rewrite with improved architecture
- Enhanced syntax highlighting with TextMate grammar from Rune DSL 9.75.1

## [5.1.12] - 2025-09-08

- Previous release (see version history)

## Attribution

This extension incorporates components from the open-source [Rune DSL](https://github.com/finos/rune-dsl) project maintained under the Fintech Open Source Foundation (FINOS) and is licensed under Apache License 2.0.
