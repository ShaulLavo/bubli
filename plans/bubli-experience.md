# bubli: a Charm-inspired React terminal toolkit

Status: proposed implementation plan, requested by the owner on 2026-09-28. This PR changes documentation only. Implementation, release and Fregat dependency cutover remain unchecked work.

## Outcome

An ordinary application built with bubli should inherit a cohesive Charm-inspired visual and interaction language. Fregat's existing React chat TUI is the first consumer. Keep its backend, shared TypeScript application logic and terminal-specific product design. Crush is the component/interaction reference, not a second application to port or a backend to rebuild.

Owner decisions: use the OpenTUI fork; replace Marked with `tree-sitter-md` on `tree-sitter-x`; support application-supplied arbitrary React components in Markdown; share renderer-neutral logic with web/editor consumers. Do not recreate Bubble Tea's Go API or add a second renderer/state machine beside React.

## Navigation and connected work

- [Coordination and PR links](../docs/bubli/README.md).
- [Reference specification, dependency/source ledger and validation](../docs/bubli/reference-spec.md).
- [83-capability catalog and implementation ownership](../docs/bubli/component-catalog.md).
- [Parser plan / PR](https://github.com/ShaulLavo/tree-sitter-md/pull/5).
- [Singapore plan](https://github.com/ShaulLavo/singapore/blob/docs/bubli-plans-2026-09-28/plans/bubli-markdown-consumer.md).
- [Fregat Plan 202](https://github.com/ShaulLavo/fregat/blob/docs/bubli-plans-2026-09-28/plans/202-bubli-tui.md) and its root roadmap own application integration and cross-project order.

The review-branch links connect the plans before merge. Retain the coordination page as the entry point and normalize document links to main after the series lands.

## Baseline and existing seams

Inspected bubli `57e5924e4e9813b31af336371b53c26577d46695`, OpenTUI `32d67005d0dd61e63dff24756d5b9061bc504325`, Crush `06e50a330e2b05b677726737d06852a35f5ff93f`. Initial Fregat research used `5a5c512a75ed635e95b0e9dcb71c28823abd71a0`; planning was reconciled with its roadmap at `4f587e90091cb0a74b314276a038b45685da572b`. Source inspection is not a runtime parity result.

- `packages/core/src/renderables/index.ts` already exposes Box, Text, Input, Textarea, Select, TabSelect, ScrollBox, ScrollBar, TextTable, Code, Markdown, Diff, Image and EmbeddedTerminal. Reuse and audit before implementing substitutes.
- `packages/core/src/renderables/Markdown.ts` currently imports Marked types/lexer and exposes a native Renderable callback. `markdown-parser.ts` is part of the cutover surface; trace all Marked imports before removal.
- `packages/react/src/reconciler/renderer.ts` exposes portals. A portal is an integration option, not proof that the Markdown override contract is implemented.
- `packages/keymap` already supplies a host-agnostic engine, terminal/DOM adapters, scoped priorities and React integration. Coordinate it with Fregat's existing command/focus service, not alongside a competing global dispatcher.
- The core manifest already points `web-tree-sitter` at the owned runtime. Preserve the native-package pinning policy established in the baseline commit; a TypeScript package version bump alone does not justify rewriting optional native pins.

## Responsibility boundaries

| Layer | Owns | Must not own |
| --- | --- | --- |
| Core/native | Cells, layout, input normalization, text measurement, selection primitives, terminal lifecycle, portable runtime/asset integration | Fregat session or backend semantics |
| React | Reconciliation, hooks/context, component lifetimes and framework-specific Markdown overrides | A second parser or global application store |
| Reusable UI layer | Semantic themes, control recipes, focus/overlay contracts, rich content presentation, motion defaults | Project/worktree data, approval decisions, filesystem traversal or auth |
| tree-sitter-md | Semantic Markdown and incremental source mapping | Terminal colors, cell geometry or React |
| Fregat adapters | Shared application palette/commands, data models, draft and conversation ownership, host capabilities | Duplicated parser/runtime state or local copies of generic controls |

Use a `packages/ui` package only if it provides the simplest public boundary; an equivalent explicit entry point is acceptable. Public `@bubli/*` naming is a packaging decision, not a prerequisite for the first tested local integration. If renamed, update all internal imports, JSX runtimes, testing exports, native loaders and consumers together without permanent aliases.

## Implementation units

### B0. Reconcile current source and establish reference fixtures

- [ ] Re-read current source, package exports, Fregat consumer and linked editor/runtime revisions. Record drift from the pinned research, retaining exact reference links.
- [ ] Use the catalog to classify every capability as reuse, extend, add, application-owned or deliberately deferred, with an acceptance case. Existing source is not proof of behavioral parity.
- [ ] Capture controlled Crush and existing Fregat screens and event traces without real credentials or paid agent calls. Prefer synthetic messages and local fixtures.
- [ ] Record terminal emulator/version, font/cell metrics, dimensions, color mode, keyboard protocol, locale, runtime and reference commit. Preserve a known-bad control for each automated comparison.
- [ ] Review licenses and source/asset provenance before carrying any implementation across repositories. Keep independent bubli branding.

Exit: reproducible fixtures and a deviation ledger. This unit finishes missing observations; it does not restart a broad research project or block independent theme/control work.

### B1. Design tokens and default recipes

Catalog: B013-B028, B040-B045, B079.

- [ ] Define semantic foreground/background ramps, primary/secondary/accent roles, status roles, contrast pairings, selection roles, six diff bands and ANSI16 mappings.
- [ ] Add a cohesive default preset with explicit overrides and a Fregat palette adapter. Separate terminal defaults/no-color from opaque application surfaces.
- [ ] Implement state recipes for focused, blurred, disabled, inactive-pane, hovered and destructive controls. Preserve geometry across state changes.
- [ ] Compose text, frames, separators, rails, badges, buttons, radios, multi-choice, tabs, inputs, secret inputs, help/status/loading/empty states and working indicators from existing primitives.
- [ ] Use one motion clock and deterministic frame/seed inputs; define reduced/static behavior. Do not impose the reference animation's 20 FPS on the entire renderer.
- [ ] Add examples with minimal props. Theme changes must invalidate Markdown/code/diff/control caches without stale colors or a hidden second theme owner.

Exit: one ordinary controls gallery and one Fregat palette render have consistent defaults, all states and narrow/no-color coverage.

### B2. Keyboard, focus, overlays and editor interaction

Catalog: B002-B005, B029-B033, B037-B044, B057-B058, B080.

- [ ] Define one route from normalized terminal events through app commands to active controls. Text input, popups, dialogs, selections and embedded terminals have explicit precedence and consumed-event behavior.
- [ ] Build a scoped overlay stack with predictable focus return and configurable bindings. The async dialog grace contract is distinct from ordinary immediately responsive dialogs.
- [ ] Implement searchable select, file-picker presentation, completion popup and attachment chips with supplied data. Keep ranking policy injectable and supply the reference basename/prefix/path-segment policy.
- [ ] Build the reusable multiline editor with measured wrapped height, latest-native-value submit, undo/redo, paste events, selection, prefix modes, history boundaries and completion anchors. Preserve Fregat's draft ownership.
- [ ] Derive hints from actual bindings; capability-specific newline keys and legacy aliases must be represented accurately.
- [ ] Supply clipboard and external-editor lifecycle interfaces, with explicit unsupported/error states. OS policy stays in host adapters.

Exit: keyboard/mouse scripts prove focus transfer, no duplicate actions, no lost final typed characters, no accidental async approval and no input stolen from an embedded terminal.

### B3. Semantic Markdown and first-class React rendering

Catalog: B008-B009, B046-B053. Hard dependency for production cutover: parser plan M1-M4.

- [ ] Consume the renderer-neutral semantic document from `tree-sitter-md`. Preserve source revision, coverage, dependency invalidation and source/display mapping. Do not reconstruct semantics using regex or emulate Marked tokens permanently.
- [ ] Build the standard, user, quiet/thinking and plan profiles from the same semantics. User line breaks, plan hierarchy, tables, links, inline code and fences follow the reference specification or an explicit tested deviation.
- [ ] Replace the Marked lexer/incremental-parser path and public Token/MarkedToken types together. Search core, React, Solid, examples, tests, declarations and build scripts; migrate all affected callers and then remove the dependency.
- [ ] Provide a React `Markdown` component with host-supplied component overrides for block and inline kinds. Treat the illustrative `components={{ codeBlock, table, link }}` names as a contract sketch until types are reviewed.
- [ ] Mount overrides as real React components, not direct function calls. Hooks, context, effects, error boundaries, events and state must work in the existing root. Do not create an isolated root per block.
- [ ] Support arbitrary terminal-compatible React subtrees in block overrides. Define inline-compatible output constraints, fallback/error behavior and measurement when a custom block resizes. DOM elements are not terminal host elements.
- [ ] Preserve stable block/component identity under harmless streaming and earlier insertions. Reclassification may replace a node; virtualization unmount/persistence policy is explicit and tested.
- [ ] Reject stale async results after edits, theme switches, document replacement or disposal. Final streamed rendering agrees with a fresh complete parse; nonlocal reference changes update earlier visible links.
- [ ] Keep Markdown data untrusted: no evaluation of supplied JSX/JavaScript/MDX, no implicit URL execution or filesystem/network reads. Add safe link activation and terminal control-character handling.
- [ ] Keep the imperative core path and non-React consumers coherent. Avoid adding React to core or silently abandoning existing Solid surfaces.

Exit: semantic fixtures, same-root React lifecycle tests and rendered profile/copy cases pass in real terminal React rendering. Only then delete the old parser path. No user-facing fallback parser or permanent compatibility shim remains.

### B4. Code, diffs, images and styled output

Catalog: B006-B012, B052-B058, B081, B083.

- [ ] Separate runtime, grammar/query resolution, token-role mapping and theme application. Reuse the existing TreeSitterClient boundary and owned runtime, with explicit language fallback/error behavior.
- [ ] Match code/fence and diff presentation using semantic roles. Use full source context where available; do not parse displayed patch lines as a complete file.
- [ ] Audit existing Diff and Singapore diff models before adding computation. Preserve unified/split, hunk/context, code/sign/gutter backgrounds, synchronized scrolling, horizontal overflow and source-faithful copy.
- [ ] Reuse Image and EmbeddedTerminal primitives with capability-aware protocols, block/text fallback, clipping and cleanup. Do not add another terminal emulator for the same surface.
- [ ] Handle ANSI-styled output through a controlled cell/text path. Remap permitted colors where specified while preventing untrusted content from changing terminal state.

Exit: the same code sample has a deliberate role mapping across Markdown, diff and viewer; protocol and fallback fixtures pass without leaked resources.

### B5. Variable-height content, selection and follow behavior

Catalog: B008-B010, B034-B036, B050, B061.

- [ ] Build on measured existing ScrollBox/list behavior. Maintain item identity, source anchor, intra-item offset and follow intent separately from array position.
- [ ] Render/measure bounded visible content; handle an individual item taller than the viewport and custom React blocks changing height.
- [ ] Preserve anchors across prepend, streaming, expansion, code highlighting, image load and resize. Scrolling away disables follow; returning to the end restores it according to the chosen contract.
- [ ] Separate source copying from rendered decoration removal. Test cross-message selections, Unicode, wraps, code spans, links and hidden content.
- [ ] Avoid full-history height work every resize frame. Cache by content/revision, width, theme and parse/highlight configuration; cancel obsolete work and release unused entries.

Exit: long synthetic conversations pass anchor and bounded-work tests with cold/warm counters. Existing Fregat's estimated 40-item window is an integration baseline, not a required algorithm.

### B6. Fregat adoption and second-consumer proof

- [ ] Publish a coherent package/asset set and verify a fresh install, JSX/runtime/testing exports, one React instance and matching Tree-sitter artifacts in Bun and supported Node.
- [ ] Pair the exact package revision with Fregat Plan 202. Keep backend/client-core contracts intact.
- [ ] Exercise the real Agent screen, then a non-chat surface such as settings or file picking using the same primitives. Generic defaults must work beyond the reference chat composition.
- [ ] Coordinate the shared semantic contract with Singapore without pulling its DOM editor into the TUI or making Fregat adoption wait on unrelated browser redesign.

Exit: reproducible paired package/consumer evidence and no accidental upstream/fork mixture in resolution.

### B7. Closure and maintenance

- [ ] Run focused regression tests, affected package suites/typechecks and relevant build/distribution checks; use repository `AGENTS.md` for current commands.
- [ ] Validate the linked PR/dependency graph, artifact manifest, license notices, behavior deviations and all catalog dispositions.
- [ ] Read back visual/replay evidence; obtain independent correctness and reproduction reviews for actionable findings.
- [ ] Update this plan and Fregat's roadmap with actual landed units and release refs. Keep source-backed contracts in permanent docs and retire completed execution text according to repository conventions.

## Dependency order

B0 establishes fixtures. B1/B2 and parser M1 can proceed independently. B3 production cutover waits for parser semantic/packaging gates; B4/B5 can prototype against the agreed contract in parallel. B6 requires the subset being adopted and its tests, not completion of every optional application composition. B7 closes each delivered slice.

Fregat's existing wave queue is not reprioritized here. No implementation or release is performed by this planning PR. tree-sitter-x and external Charm repositories need no speculative changes; demonstrated upstream/runtime gaps get separate linked work.

## Acceptance matrix and failure handling

Use [the reference validation matrix](../docs/bubli/reference-spec.md#validation-and-release-gates). Every implemented capability has a fixture and an explicit result. A green unit suite does not certify terminal appearance, and a screenshot does not certify keyboard routing or source copying.

On a failed release/integration gate, keep the last working package set while repairing the failure; do not ship mixed native/JS assets, duplicate engines, guessed fallbacks or silent data loss. Reverting an implementation commit is a recovery path, not an instruction to introduce permanent runtime switches. No data reset or backend migration belongs to this work.
