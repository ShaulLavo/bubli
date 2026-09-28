# bubli cross-repository planning set

Workstream: `bubli-2026-09-28`. Status: proposed implementation plans, opened at the owner's request on 2026-09-28. All four PRs are documentation-only. No runtime implementation, package release, dependency cutover, backend change or deployment is represented as complete.

## Start here

The goal is to run Fregat's existing React terminal app on bubli, with cohesive Charm-inspired controls and interactions, while keeping the existing backend and shared TypeScript application logic. The owned Markdown engine is tree-sitter-md on tree-sitter-x. Marked and its public token types are removed in the implementation after conformance and consumer gates pass. Markdown supports trusted application-defined React components in the existing React tree; Markdown data never executes JSX/MDX.

| Repository | Planning PR | Plan | Responsibility |
| --- | --- | --- | --- |
| bubli | [ShaulLavo/bubli#1](https://github.com/ShaulLavo/bubli/pull/1) | [Toolkit plan](../../plans/bubli-experience.md) | Core/React integration, themes, controls, focus, rich content, motion, lists and distribution |
| tree-sitter-md | [ShaulLavo/tree-sitter-md#5](https://github.com/ShaulLavo/tree-sitter-md/pull/5) | [Semantic Markdown](https://github.com/ShaulLavo/tree-sitter-md/blob/docs/bubli-plans-2026-09-28/plans/bubli-semantic-markdown.md) | Semantics, source correspondence, compatibility profiles, streaming and packaged parser artifacts |
| Singapore | [ShaulLavo/singapore#62](https://github.com/ShaulLavo/singapore/pull/62) | [Editor consumer](https://github.com/ShaulLavo/singapore/blob/docs/bubli-plans-2026-09-28/plans/bubli-markdown-consumer.md) | Existing Markdown authoring/worker consumer, shared semantic results and paired package pins |
| Fregat | [ShaulLavo/fregat#194](https://github.com/ShaulLavo/fregat/pull/194) | [Plan 201](https://github.com/ShaulLavo/fregat/blob/docs/bubli-plans-2026-09-28/plans/201-bubli-tui.md) | Existing TUI adoption, shared palette/commands/data, host adapters and app validation |

Fregat's [root PLAN.md](https://github.com/ShaulLavo/fregat/blob/docs/bubli-plans-2026-09-28/PLAN.md#bubli-tui-workstream) records the workstream and cross-project order. Its pre-existing wave-2 queue remains unchanged. The plan index registers 201; Singapore's README links its consumer work package.

## Research retained in this PR

- [Reference specification and source ledger](reference-spec.md): pinned repository/manifest evidence, 30 relevant dependency/support modules across 23 dependency repositories, palette and syntax roles, interaction recipes, 32 source-defined constants, 41 original file-level references, Fregat gaps, licensing and validation.
- [83-capability catalog](component-catalog.md): foundation, design system, controls, content, application compositions and shared integration, with disposition, owner and acceptance fixture for each ID.
- [Toolkit execution plan](../../plans/bubli-experience.md): units B0-B7, dependencies and evidence gates.

The research's original suggestion to keep Marked is superseded by the owner's parser choice. The parser now reports 676/676 normalized cases and package-consumer gates; earlier 674/676 notes are historical. Full semantic/rendering compatibility is still a separate gate. Existing Fregat Plan 189's required extensions remain with their owner and are not cancelled or made optional here.

## Documentation merge order versus implementation order

These PRs may be reviewed and merged independently as one connected planning set. They do not depend on implementation code from another planning PR, and merging one does not certify any implementation gate. Until merge, document links use the common review branch. Retain the permanent PR links and normalize cross-repository document links to main after the series merges, before deleting review branches used by those links.

Implementation follows producer contracts, not an artificial serial queue:

| Stage | Work that can proceed | Required handoff |
| --- | --- | --- |
| Baseline | bubli B0, parser M0, Singapore S0, Fregat F0 | Current source/asset refs, consumer map and controlled fixtures |
| Defaults and interaction | bubli B1/B2 and relevant Fregat F1 adapters | Existing native controls and one coordinated command/focus contract; independent of new parser semantics |
| Semantic producer | Parser M1-M3 | Renderer-neutral revisioned data, source mapping, semantic/profile and stream evidence |
| Consumer prototypes | bubli B3, Singapore S1/S2 | Agreed M1 contract; may develop in parallel, cannot claim production conformance before producer gates |
| Rich content and lists | bubli B4/B5 | Relevant theme, source/highlighting and measurement contracts; exact subset dependencies recorded per unit |
| Production adoption | parser M4, bubli B6, Singapore S3/S4 as relevant, Fregat F1-F4 | Coherent packaged artifacts plus consumer tests; Marked cutover waits for its semantic/React/profile gates |
| Closure | bubli B7 and Fregat F5 | Runtime/visual/interaction/performance/distribution evidence and updated plan status |

The initial verified TUI slice need not wait for unrelated browser UI or every 099/198 unit. Any document publication, highlighting, analysis or isolation contract it actually consumes remains a real prerequisite. Existing Plans 108/111/171/176/179/189/197/198/200 retain their responsibilities. Plan 189 extensions use explicit profiles and do not all gate the initial Crush profile.

## Cross-repository handoff record

Each implementation PR in this series links this index, its owning plan/unit, and the producer/consumer PRs it depends on. Before a package/consumer cutover, record:

1. Parser semantic schema/profile and source revision; built tree-sitter-x package revision separately from native runtime source; grammar/resolver artifacts and hashes.
2. bubli core/native/React package versions and exact native pin policy; runtime/worker asset resolution and deduplication evidence.
3. Singapore package/ref when affected; Fregat catalog, lockfile and CI setup/linked-source refs.
4. Shared semantic/stream fixtures and expected results; actual package, rendering, focus/copy/anchor, lifecycle and cold/warm performance checks.
5. Deliberate behavior deviations, missing platform observations and license/provenance disposition.

Use the existing repository owners and APIs. Shared contracts do not mean one heap across separate workers, one application store inside the toolkit, or a browser editor inside the TUI.

## Repositories not receiving speculative PRs

Crush, Bubble Tea, Bubbles, Lip Gloss, Glamour, Goldmark, Chroma, Ultraviolet and other supporting repositories are read-only references for this work. OpenTUI changes land in bubli unless a concrete independently useful upstream fix is identified.

tree-sitter-x already provides the extension mechanism used by tree-sitter-md and is the selected shared runtime. No missing runtime capability was established by this research. Reuse it first. If implementation exposes an ABI/export/lifetime defect, open a separately linked minimal reproduction and targeted runtime PR, update this index, and gate only the affected consumer. Do not introduce a runtime rewrite as a prerequisite by assumption.

## Evidence status

The planning set was prepared from repository source, current plan conventions and the prior research artifacts. Repository-reported test results are attributed; no application tests, terminal replays, benchmarks or releases were run while writing these plans. A screenshot cannot certify input routing, and a normalized parser score cannot certify displayed Markdown, clipboard text or React lifecycle. Those explicit implementation checks remain open in the plans.
