# Terminal UI research: historical toolkit workstream

Status: SUPERSEDED as an execution workstream by the owner's 2026-09-29 decision.
The bubli name, standalone toolkit and permanent OpenTUI-fork assumption are dropped.
Implementation now belongs to Fregat's `apps/tui/src/ui/` on upstream OpenTUI.
The existing backend, shared TypeScript logic, owned Markdown/runtime and Singapore work remain.

## Active coordination

Start with [Fregat PR 198](https://github.com/ShaulLavo/fregat/pull/198),
[Plan 202](https://github.com/ShaulLavo/fregat/blob/docs/tui-ui-upstream-2026-09-29/plans/202-tui-ui.md)
and the [Fregat coordination index](https://github.com/ShaulLavo/fregat/blob/docs/tui-ui-upstream-2026-09-29/docs/tui-research/ui-plan-links.md).
Its [root roadmap](https://github.com/ShaulLavo/fregat/blob/docs/tui-ui-upstream-2026-09-29/PLAN.md#tui-ui-workstream)
owns scheduling. Use those Fregat paths on main after the revision merges.

The [parser plan](https://github.com/ShaulLavo/tree-sitter-md/blob/main/plans/bubli-semantic-markdown.md)
and [Singapore plan](https://github.com/ShaulLavo/singapore/blob/main/plans/bubli-markdown-consumer.md)
retain M0-M4 and S0-S4. Their existing filenames preserve links; neither now depends on a
standalone toolkit release. Fregat directly owns the terminal consumer and its React/profile,
source-copy, streaming, runtime and upgrade verification.

## Research retained

- [Reference specification and source ledger](reference-spec.md): pinned dependencies and
  sources, appearance and interaction recipes, constants, Markdown requirements and validation.
- [83-capability catalog](component-catalog.md): acceptance cases and the original inventory.
- [Plan retirement and transfer map](../../plans/bubli-experience.md): where former B0-B7 work
  now lives in Fregat F0-F5.

The catalog and reference specification are retained research, not a parallel set of implementation
orders. Their fork/package ownership and release assumptions are superseded by Fregat Plan 202.
Use upstream engine capabilities where sufficient, app-local UI for presentation and interaction,
and explicit patches only for demonstrated gaps. The change does not discard the desired look
and feel or make parser/editor compatibility optional.

## Architectural boundary

The app's Markdown path consumes tree-sitter-md on tree-sitter-x and supports trusted React
subtrees in its existing root. It does not evaluate Markdown as JSX/MDX. Marked and its token types
are excluded from that app path; deleting them from upstream OpenTUI's own package is not a gate.
Check actual built-worker JavaScript, WASM subpaths, runtime identity and payload separately from
manifest overrides. Distinct workers retain their own heaps.

No new toolkit namespace, mirrored UI package, renderer/reconciler fork or native-release pipeline
is scheduled. Fregat Plans 203/206 own keymap infrastructure and 207 owns any Editor/mirror move.
A maintained fork may be reconsidered only after evidence that public extensions and focused
patches cannot support essential behavior. Existing release assets remain available for recovery.

## Historical discussion and evidence

Original planning: [toolkit #1](https://github.com/ShaulLavo/bubli/pull/1),
[parser #5](https://github.com/ShaulLavo/tree-sitter-md/pull/5),
[Singapore #62](https://github.com/ShaulLavo/singapore/pull/62) and
[Fregat #194](https://github.com/ShaulLavo/fregat/pull/194).
[Immutable pre-revision index](https://github.com/ShaulLavo/bubli/blob/62ef77483e7e2289b70a5d45e0b90500c7b53776/docs/bubli/README.md)
preserves the earlier organization and handoff history.

This retirement changes documentation only. It neither marks implementation complete nor changes
runtime code, dependencies, repository settings, release assets, deployment or stored app data.
