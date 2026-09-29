# Superseded: standalone terminal toolkit

Status: SUPERSEDED by the owner's 2026-09-29 decision. This file is a historical redirect,
not an active implementation queue. The owner dropped the bubli name, separate toolkit and
permanent OpenTUI-fork assumption. No implementation is claimed complete by this change.

## Active plan

Build terminal UI inside Fregat's `apps/tui/src/ui/` on upstream `@opentui/core` and
`@opentui/react`. [Fregat PR 198](https://github.com/ShaulLavo/fregat/pull/198) owns the revision:

- [Plan 202: terminal UI in Fregat](https://github.com/ShaulLavo/fregat/blob/docs/tui-ui-upstream-2026-09-29/plans/202-tui-ui.md).
- [Root roadmap](https://github.com/ShaulLavo/fregat/blob/docs/tui-ui-upstream-2026-09-29/PLAN.md#tui-ui-workstream).
- [Coordination and retained references](https://github.com/ShaulLavo/fregat/blob/docs/tui-ui-upstream-2026-09-29/docs/tui-research/ui-plan-links.md).

After the revision merges, use those Fregat paths on main. Its roadmap is the scheduler;
this repository does not retain a parallel toolkit-release workstream.

## Work transferred without losing the research

The [previous B0-B7 plan](https://github.com/ShaulLavo/bubli/blob/62ef77483e7e2289b70a5d45e0b90500c7b53776/plans/bubli-experience.md)
remains available in history. Its behavior work moves into Plan 202:

- B0 baseline and fixtures become F0.
- B1 design tokens and B2 controls/focus/overlays become app-local F2 work.
- B3 semantic Markdown and React overrides become F3.
- B4 code/diffs/images and B5 lists/selection/follow become F4.
- B6 adoption becomes upstream integration in F1 and real app-surface proof in F4.
- B7 verification and maintenance become F5.

The [capability catalog](../docs/bubli/component-catalog.md) and
[reference specification](../docs/bubli/reference-spec.md) retain source observations, visual
recipes, constants and acceptance cases. Their old fork, core, package and publication ownership
is historical and superseded by Plan 202. An upstream engine capability is not an instruction to
reimplement it locally.

## Decisions that still apply

Use the owned tree-sitter-md + tree-sitter-x engine. Preserve semantic CommonMark/GFM/selected
Goldmark fidelity, source mapping, streaming and real React component overrides. Singapore and
the parser retain their work units and package gates. Fregat keeps its existing backend, shared
TypeScript logic and terminal-specific layout; Charm/Crush remains the experience reference.

The local Markdown path does not use Marked or its token API. Upstream OpenTUI's built-in Markdown
component and bundled dependencies need not be modified. Installed, bundled and loaded byte
savings require separate measurements.

Start with public APIs, component composition and custom renderables. A demonstrated engine gap
may justify a minimal upstream contribution or tracked version-pinned patch with a regression
test and removal condition. Reopening a maintained fork requires a separate evidence-backed
owner decision. No new UI namespace, reconciler, Solid migration or native release pipeline is
scheduled. Fregat's Plans 203/206 own the command dispatcher; no competing keymap is introduced.

## Repository and recovery boundary

This is a documentation change only. It does not archive or delete this repository, rewrite
history, remove existing release assets, alter package pins or cut Fregat over to upstream.
Existing artifacts remain available while the app verifies its replacement. Runtime ABI changes
in tree-sitter-x are still justified by concrete reproductions, not by this plan's retirement.
