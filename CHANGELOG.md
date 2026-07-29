# Changelog

All notable changes to the Rune DSL VS Code Extension.

## [8.6.0] - 2026-07-29

### Added
- **DRR Data Lineage (beta)** — ask any field of a DRR report where its value comes
  from, and read the answer on one page. Pick a report (twelve reports across nine
  regimes: ASIC, CFTC, CSA, ESMA EMIR, FCA UK EMIR, HKMA, JFSA, MAS and SEC), pick
  a field — or search the full list — and the field's journey renders in four layers:
  - **Declaration** — the regulation's own definition of the field, tier by tier,
    provisions and citations verbatim;
  - **Extraction** — the model rule that computes the value, with the real code lit
    along its route;
  - **Projection & Submission** — where the value lands in the ISO message for each
    action type (new, modification, correction, …), down to the document path and
    the wire legs;
  - **Validation** — the checks that guard the field.

  Everything on the page comes from the compiled model — nothing is hand-written.
  The data pack downloads once per DRR release on first open (~95 MB) and is cached
  locally. Packs are published per DRR release, starting with 7.4.0 — on other
  versions the page tells you which packs exist. Available in built DRR 7.x
  projects; access follows your DRR model access.
- **DRR 8 upgrade-branch builds** — the DRR 8 `-upg.N` release family is now
  first-class in the Build Manager: the upgrade branches appear alongside the GA
  line, each with its own toolchain, and build end-to-end like any release.
- **Updates on the front page** — an Updates reader on the navigator: What's New,
  In the Works and Important Updates in one panel with unread badges, older
  announcements a scroll away, and a ticker line that rotates through what's
  active. Messages appear as they are published — no update needed to receive
  them.

### Changed
- **Version discovery** — the plugin now finds the published data for your version
  automatically, so future releases are picked up without a plugin update.
- **The Designer says what's missing up front** — if a model version has no
  published semantic data, the Designer tells you before you start, instead of
  quietly offering a reduced surface.
- **Builds trust this machine's certificates (Windows)** — on corporate
  networks that inspect HTTPS, Java's bundled trust list rejects the re-signed
  certificates and builds die with a PKIX error even though the machine itself
  trusts them. Model builds now use the Windows certificate store — the same
  trust your browser uses — so inspected connections validate with no IT
  ticket. Setting `RUNE_BUILD_NO_OS_TRUST=1` reverts to Java's bundled list.
- **The Corporate Compatibility Checker is one click away** — the Environment
  panel now explains the checker and runs it directly. The checker also probes
  the Maven repositories a model build uses — including Maven Central itself —
  so a blocked host is named before a build fails, and when a network re-signs
  certificates it names the inspection product in the report.
- **A new permanent backend address** — 8.6 talks to
  `rune-dsl-studio-proxy.nicholas-314.workers.dev`, the plugin's permanent home
  from this release on. Corporate networks that allowlist by exact hostname
  should add it (the checker probes it and will name it if blocked). Earlier
  plugin versions keep using the old address, which continues to serve them.

### Fixed
- **Welcome screen rhythm** — the header spacing matches the Designer's; the
  Regulatory card's beta pill anchors to its button; pressing the Designer button
  no longer disturbs the welcome frame.
- **Sidebar first paint after a build** — the left bar shows its loading state
  immediately after a build instead of sitting blank for ~10 seconds.
- **First open of a freshly built project** — the project's change-tracking
  snapshot now runs in the background: the sidebar paints immediately instead of
  sitting blank for minutes on machines with slow on-access disk scanning.
- **Designer instance lineage** — the viewer pans by drag, exports a PNG, folds
  its instruction strip, and its before/after diff now compares the trade's state
  before and after the event.

## [8.5.0] - 2026-07-08

### Added
- **Designer: Post-Trade Events** — the Designer now builds lifecycle events against a base trade, not only the base trade itself. Pick a built base object and the Designer detects which post-trade events apply to it, offers them in a dropdown (the same idiom as the product picker), and — for the event you select — shows what the event does and the evidence for why it applies before anything runs. Choosing an event walks it through the same five-step flow as a product.
  - **Applicability** — the applicable-event list is derived from the base trade's own shape and the model's event pre-conditions; events the model evidences or that a convention seats are listed directly, and events that neither gate nor seat are gathered under a plain counted line rather than silently dropped.
  - **Event qualification** — the selected event's qualification is shown as the model's own rule with the satisfying evidence lit, and the walk reports what the event changes.
- **Designer: instances and lineage** — executed objects are stored as named instances, and two identical base trades are now kept distinct (auto-numbered labels) instead of collapsing into one. Open the lineage of any instance in a top-down viewer that lays out the chain of states and the events between them, with before/after quantities at each step and the driving instruction inspectable.
- **Designer: a semantic choice layer over qualification** — as you make the choices the model asks for, the Designer now surfaces the meaning behind each one, not just the raw field:
  - **Value choices** — a field whose value changes what the trade *is* (option direction Call / Put, cash vs physical settlement, the booking basis, exercise style) is presented as a labelled choice showing its real enumerated values, read from your model's own CDM enums — and where two enums share a name, the authoritative `cdm.*` one is preferred so the values are never ambiguous.
  - **Alternative shapes** — where a choice reshapes the structure itself (a monetary-notional booking versus a share-count booking; the term-provision families; an averaging feature where the product supports one), the Designer presents the alternatives side by side and carries the one you pick into the object being built.
  - **Written through in the model's own shape** — a resolved semantic choice is set directly into the object under construction using the field shapes the model expects (reference and value wrappers where the model requires them), and is checked against the model's own qualification before it is committed, and rolled back when the check cannot confirm it; the Model view and the executed pipeline then reflect exactly what was committed.
  - Everything here is derived from the model in your project, per major version — nothing is hardcoded to one release.
- **Designer: a five-step flow with an explicit qualification step** — the stepper is now **Setup → Qualify → Model → Execute → Export**. Setup gathers the structural choices the model requires; qualification no longer fires on the last click but waits for an explicit **Qualify this structure** action, so nothing runs until you ask. Once qualified, choices the chosen route rules out fold under a plain **Not applicable — based on your choices** line, each with the reason and a way to re-open it.
  - The Post-Trade Events surface shares the same five-step flow, so the base-trade and event paths read identically.
- **Corporate Compatibility Checker** — a new command, **Rune DSL: Run Corporate Compatibility Checker**, that diagnoses whether a locked-down corporate network can reach everything a build needs. It probes every backend endpoint over both paths the plugin uses — the editor's own requests and the Python build's requests — and reports DNS, TLS, proxy, and timeout results, checks the toolchain (Java, Maven, Git, Python), disk access, and Windows long-path handling. It only diagnoses: nothing is changed, and the local report it writes is never uploaded. It runs without an open project.
- **Latest Updates** — a new command, **Rune DSL: Latest Updates**, backed by a content channel that pulls notices, tips, and What's-New entries live and shows exactly what was published — the page never fabricates content. Designer entries are marked **(beta)**.
- **CDM 5, 6 and 7** — the post-trade, lineage and semantic work is derived per major version from the model in your project; each supported major is exercised against its built-in sample product set before release.

### Changed
- **Designer wording pass** — the panel's copy was tightened throughout: implementation terms no longer surface (qualification output reads as "the model output"); definitional subtitles fold into an info affordance instead of crowding each row; route and branch labels read as English rather than raw model paths.
- **Java-analysis dependency cache** — for a model that embeds another (DRR embeds CDM), a rebuild now reuses the embedded model's Java-analysis indices instead of recomputing them, cutting rebuild time. The cache fails open: on any doubt — a partial dependency closure, a namespace it cannot fully account for, an empty or tampered entry — it recomputes rather than serve an incomplete result, and it is keyed to the toolchain version so a toolchain change never reuses old analysis.
- **8.5 asset line** — the plugin now prefers 8.5 published assets and falls back to the previous line for anything not yet republished, across toolchain, indices, and tools.
- **Project upgrade gate** — opening a project built by an earlier Studio now prompts a one-click upgrade sized to the version gap. Projects built by 8.3/8.4 download and install the 8.5 semantic index for their model version — no rebuild, no toolchain re-download; if no semantic index is published for that version yet, the upgrade records that and completes. Pre-8.3 projects keep the full rebuild path, which now also installs the semantic index. A failed download leaves the project untouched and the upgrade can be retried.

### Fixed
- **Designer configuration axes on CDM 7** — a configuration choice whose value is already fixed by the qualification route no longer appears as an open axis (it was over-offering combinations on CDM 7); the fixed value is seeded instead.
- **Designer Qualify panel** — expanding **Show qualification logic** past the visible height now grows the card and scrolls its body, instead of letting the logic spill past the card's bottom edge.
- **Designer choice-pill alignment** — semantic choice pills in an outstanding gate line up with the route cards beside them instead of hugging the panel edge.
- **Designer wide layouts** — resolved-choice rows no longer strand their value and **Change** control at the far right on wide (1080p and above) displays; the choice surfaces are laid out on a real key/value grid and capped so nothing drifts apart.

## [8.4.0] - 2026-07-03

### Added
- **Designer: the guided CDM workbench, rebuilt as a four-step flow** — pick a product and go from qualification to running code without leaving the panel:
  - **Qualify** — the qualification route and every field-level configuration choice are derived live from the model in your project and presented as interactive choice gates; a qualified banner and recap confirm the result.
  - **Model** — an interactive model view of the qualified product: the structure with your choices in place, per-node detail (sample data, conditions, validation predicates, provenance, attributes and functions), and the projected JSON alongside. Export the view as a crisp PNG or as Mermaid.
  - **Execute** — run the built object through the model's own validation rules, inspect the outcome, and store instances as named objects; a searchable object explorer (with asset-class filters) lives in the left bar.
  - **Code Export** — real builder-pattern Java (a clean skeleton to complete, or seeded with sample data), a runner, and per-product Windows/macOS run scripts; each product exports into its own package so multiple products coexist.
- **Designer coverage across CDM major versions 5, 6 and 7** — the flow reads the model version your project uses (DRR projects participate through their embedded CDM); nothing is hardcoded to one release line. Each supported major is exercised against its full built-in sample product set.
- **Designer identity** — compact brand header above the steps and a refreshed empty state.
- **Code folding for `.rosetta` files** — types, functions, rules and other blocks fold in the editor.
- **RuneBook: pinned Hierarchy and [+ Custom Java]** — the navigation tabs stay pinned while you work through a book.

### Changed
- **Designer visual polish** — theme-consistent scrollbars everywhere (including after switching the in-panel theme), a qualification loading animation, larger code peek, expandable sample-data entries for object arrays, and consistent step-tab styling.
- **Model right bar** — sections start collapsed and remember your expansion per section; the legend reflects only what is actually on screen; JSON views gained line numbers and indent guides.
- **Project cards** — type-tinted bands distinguish model families at a glance.
- **Welcome view** — features grouped into categories.
- **Training** — course entry gating now reflects the model version the course targets; lesson copy tightened.

### Fixed
- **Designer stability** — product selections and the qualified state no longer clear unexpectedly during background index refreshes; the Model view no longer flashes or re-renders in a loop; choice gates always respond to clicks; index reloads are atomic (a failed reload keeps the last good state instead of emptying the panel).
- **Exported run scripts** — generated PowerShell scripts run reliably (correct quoting, honest exit codes, a `run.log` you can read when something fails); two exports never overwrite each other.
- **Install paths with spaces** are handled correctly throughout the build pipeline.
- **Index freshness** — vendored indices no longer report stale immediately after a fresh install; upgrading a project refreshes its indices coherently.

## [8.3.2] - 2026-06-28

### Fixed
- **Rune DSL 10 support** — a change to support the Rune DSL 10 line; projects targeting Rune DSL 10 now build and generate their full source set correctly, including override rebuilds. Earlier versions could leave a Rune DSL 10 project with an incomplete generated-source set.

## [8.3.1] - 2026-06-27

### Fixed
- **End User License Agreement now displays** — the EULA is bundled in the package, so **Review & Accept** opens the full agreement before acceptance (previously the document was not packaged and the review step showed nothing).

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
