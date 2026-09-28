# bubli reference specification and source ledger

Research date: 2026-09-28. Implementation owner: [bubli experience plan](../../plans/bubli-experience.md). [Coordination](README.md) links all companion PRs. [Catalog](component-catalog.md) keeps the 83 stable capability IDs.

## Evidence and decisions

This document consolidates the research dossier, dependency ledger, measurable reference data, validation brief and subsequent Markdown decision. It is a source-backed target, not a claim of pixel-perfect or behavioral parity. Source and test code were inspected; no terminal application, replay, benchmark or test suite was executed during planning. Documentation, manifest dependencies, inspected implementation and proposed acceptance tests are distinct evidence classes.

The target is Fregat's existing TUI on bubli, with shared React/TypeScript logic and an unchanged backend. The owner selected an owned Markdown stack: tree-sitter-md + tree-sitter-x replaces Marked; React overrides are first-class. Older research suggestions to retain Marked are superseded. Other Charm repositories remain references, not repositories to modify or Go APIs to port mechanically.

## Version lock

| Repository | Revision and role |
| --- | --- |
| [Crush](https://github.com/charmbracelet/crush/tree/06e50a330e2b05b677726737d06852a35f5ff93f) | `06e50a330e2b05b677726737d06852a35f5ff93f`, experience reference |
| [OpenTUI](https://github.com/anomalyco/opentui/tree/32d67005d0dd61e63dff24756d5b9061bc504325) | `32d67005d0dd61e63dff24756d5b9061bc504325`, upstream foundation |
| [bubli](https://github.com/ShaulLavo/bubli/tree/57e5924e4e9813b31af336371b53c26577d46695) | `57e5924e4e9813b31af336371b53c26577d46695`, fork baseline |
| [Fregat research](https://github.com/ShaulLavo/fregat/tree/5a5c512a75ed635e95b0e9dcb71c28823abd71a0) | `5a5c512a75ed635e95b0e9dcb71c28823abd71a0`, original UI audit |
| [Fregat planning](https://github.com/ShaulLavo/fregat/tree/4f587e90091cb0a74b314276a038b45685da572b) | `4f587e90091cb0a74b314276a038b45685da572b`, roadmap reconciliation |
| [Singapore](https://github.com/ShaulLavo/singapore/tree/79895646ec626d03e88aca148b60b8a05d884970) | `79895646ec626d03e88aca148b60b8a05d884970`, existing Markdown editor consumer |
| [tree-sitter-md](https://github.com/ShaulLavo/tree-sitter-md/tree/5dd917ae70eea5f6a38a2ba8825ce6d3baca697d) | `5dd917ae70eea5f6a38a2ba8825ce6d3baca697d`, current planning baseline |
| [tree-sitter-x built package](https://github.com/ShaulLavo/tree-sitter-x/tree/e2985e082d9f3f137ca20af6d7cee9d1a1c8ddda) | `e2985e082d9f3f137ca20af6d7cee9d1a1c8ddda`, original common runtime pin; recheck consumer manifests before cutover |

Dependency versions below come from Crush's selected manifest [C01], not independently resolved immutable commits for every tag. The x repository contains independently versioned modules. Resolve tags, artifacts and licenses before implementation/release; do not compare a historical screenshot with a different source revision silently.

## Dependency corpus

Thirty UI-relevant/support modules across 23 dependency repositories were identified. This is a curated corpus, not a complete platform-specific `go list -deps` census. UI imports establish use more strongly than a root manifest alone.

| Module/repository | Selected version/ref | Role and disposition |
| --- | --- | --- |
| [Bubble Tea](https://github.com/charmbracelet/bubbletea/tree/v2.0.9) | v2.0.9 | Lifecycle/events; retain OpenTUI/React and reproduce observable contracts |
| [Bubbles](https://github.com/charmbracelet/bubbles/tree/v2.2.1) | v2.2.1 | Evidenced textarea, textinput, filepicker, spinner, help, key; reusable React controls |
| [Lip Gloss](https://github.com/charmbracelet/lipgloss/tree/v2.0.6) | v2.0.6 | Styles, frames, layout, gradients and tree formatting; tokens/recipes over native primitives |
| [Ultraviolet](https://github.com/charmbracelet/ultraviolet/tree/006e29f97886) | v0.0.0-20260811164956-006e29f97886 | Cells, rectangles and drawing; use the existing engine |
| [Glamour](https://github.com/charmbracelet/glamour/tree/v2.0.1) | v2.0.1 | Markdown terminal rendering; reference oracle and profile behavior |
| [Chroma](https://github.com/alecthomas/chroma/tree/v2.27.0) | v2.27.0 | Code tokens/formatting; owned highlighting contract, not API emulation |
| [Goldmark](https://github.com/yuin/goldmark/tree/v1.7.17) | v1.7.17 | Markdown semantics; test-only comparison for owned parser |
| [goldmark-emoji](https://github.com/yuin/goldmark-emoji/tree/v1.0.5) | v1.0.5 | Optional Glamour extension; dependency presence does not prove Crush enables it |
| [go-udiff](https://github.com/aymanbagabas/go-udiff/tree/v0.4.1) | v0.4.1 | Diff computation; audit existing JS diff and Singapore diff first |
| [x/exp/charmtone](https://github.com/charmbracelet/x/tree/exp/charmtone/v0.1.0/exp/charmtone) | v0.1.0 | Reference palette |
| [x/ansi](https://github.com/charmbracelet/x/tree/ansi/v0.11.8/ansi) | v0.11.8 | ANSI width/manipulation/control and Kitty protocol |
| [colorprofile](https://github.com/charmbracelet/colorprofile/tree/v0.4.3) | v0.4.3 | Terminal color capability handling |
| [x/term](https://github.com/charmbracelet/x/tree/term/v0.2.2/term) | v0.2.2 | OS/terminal support; lifecycle adapter |
| [displaywidth](https://github.com/clipperhouse/displaywidth/tree/v0.11.0) | v0.11.0 | Cell-width reference |
| [uax29](https://github.com/clipperhouse/uax29/tree/v2.7.0) | v2.7.0 | Word and grapheme segmentation |
| [uniseg](https://github.com/rivo/uniseg/tree/v0.4.7) | v0.4.7 | Grapheme/width behavior |
| [fuzzy](https://github.com/sahilm/fuzzy/tree/v0.1.3) | v0.1.3 | Matching; preserve context-specific ranking above matching |
| [go-colorful](https://github.com/lucasb-eyer/go-colorful/tree/v1.4.1) | v1.4.1 | Color blending and motion support |
| [clipboard](https://github.com/golang-design/clipboard/tree/v0.9.0) | v0.9.0 | Local text/image access; explicit host service |
| [x/editor](https://github.com/charmbracelet/x/tree/editor/v0.2.0/editor) | v0.2.0 | External editor handoff |
| [imaging](https://github.com/disintegration/imaging/tree/v1.6.2) | v1.6.2 | Image fitting/resampling |
| [go-ansi-paintbrush](https://github.com/jordanella/go-ansi-paintbrush/tree/b7ad996ecf3d) | v0.0.0-20240728195301-b7ad996ecf3d | Block image rendering support |
| [beeep](https://github.com/gen2brain/beeep/tree/v0.11.2) | v0.11.2 | Notification host support; confirm active paths before adopting |
| [browser](https://github.com/pkg/browser/tree/5ac0b6a4141c) | v0.0.0-20240102092130-5ac0b6a4141c | URL opening; user-action host boundary |
| [go-humanize](https://github.com/dustin/go-humanize/tree/v1.0.1) | v1.0.1 | Metadata formatting |
| [xxh3](https://github.com/zeebo/xxh3/tree/v1.1.0) | v1.1.0 | Cache keys and deterministic animation seeds |
| [x/exp/golden](https://github.com/charmbracelet/x/tree/exp/golden/v0.1.0/exp/golden) | v0.1.0 | Golden tests; use equivalent native/React fixtures |
| [x/exp/ordered](https://github.com/charmbracelet/x/tree/exp/ordered/v0.1.0/exp/ordered) | v0.1.0 | Numeric helpers |
| [x/exp/strings](https://github.com/charmbracelet/x/tree/exp/strings/v0.1.0/exp/strings) | v0.1.0 | String helpers |
| [x/exp/slice](https://github.com/charmbracelet/x/tree/exp/slice/v0.1.0/exp/slice) | v0.1.0 | Collection helpers |

Read [D01]-[D04] for dependency relationships and [C02] for the custom UI architecture. Do not add model SDKs, agent loops, databases, MCP servers, authentication services or telemetry backends to bubli. A transitive library unused by the selected UI controls does not automatically require a JavaScript counterpart.

## Visual reference

The reference theme function is `CharmtonePantera`; the built-in key is `charmtone-panther`. Do not silently normalize that spelling in a reference fixture. Colors are from theme construction [C05] and the separately versioned palette [D06]. They are reference data, not a final public theme API.

| Roles | Named values / hex |
| --- | --- |
| Primary / secondary / accent / keyword | Charple #6B50FF / Dolly #FF60FF / Bok #68FFD6 / Blush #FF84FF |
| Foreground base / subtle / more subtle / most subtle | Sash #ECEBF0 / Smoke #BFBCC8 / Squid #858392 / Oyster #605F6B |
| On-primary | Butter #FFFAF1 |
| Background base / least visible / less visible / most visible | Pepper #201F26 / BBQ #2D2C36 / Char #3A3943 / Iron #4D4C57 |
| Separator | Char #3A3943 |
| Destructive / error | Coral #FF577D / Sriracha #EB4268 |
| Warning / warning-subtle / attention / busy | Mustard #F5EF34 / Zest #E8FE96 / Tang #FF985A / Citron #E8FF27 |
| Info / more subtle / most subtle | Malibu #00A4FF / Sardine #4FBEFE / Damson #007AB8 |
| Success / more subtle / most subtle | Julep #00FFB2 / Bok #68FFD6 / Guac #12C78F |
| Plan / plan-more-subtle / yolo | Charple #6B50FF / Hazy #8B75FF / Zest #E8FE96 |
| Button / subtle / inactive / hovered | Dolly #FF60FF / Char #3A3943 / Iron #4D4C57 / Oyster #605F6B |
| Diff insertion / deletion foreground | #629657 / #A45C59 |

ANSI16 mapping in normal index order: BBQ, Coral, Guac, Mustard, Charple, Dolly, Malibu, Smoke. Bright: Iron, Tuna (#FF6DAA), Julep, Zest, Guppy (#7272FF), Blush, Sardine, Salt (#F7F6FB). This is for intentionally remapping raw command output, not permission to execute its control sequences. [C05][C06]

### Component recipes and geometry

- **Selection:** general content uses on-primary foreground on primary background; textarea selection uses on-primary on secondary. A single selection color loses a reference distinction. Input cursor is a blinking block in secondary. [C06]
- **Message rails:** user blurred content has a thin primary left border plus one cell padding; focused uses `▌` plus one. Assistant/tool blurred content has two cells padding; focused uses a `▌` success-most-subtle rail plus one. Text x-position remains unchanged. [C06]
- **Frames:** rounded primary dialog frame; width includes frame/padding. Content uses the inner width; titles truncate rather than wrap accidentally. A plan card has rounded plan-colored border, padding 1 row/2 columns and no full background fill, while code/H1 chips may carry backgrounds. [C02][C06]
- **Buttons:** focused secondary-button background/on-primary ink; blurred, inactive-pane, hovered and negative variants differ. Inactive-pane is not interchangeable with disabled. [C06]
- **Lists/completions:** selected dialog row has primary background/on-primary ink and one cell horizontal padding; info column is most-subtle when blurred, base foreground when focused. Normal completion uses less-visible background; selected uses primary; matched spans are underlined. [C06]
- **Prompt gutter:** normal first line uses `  > `, continuation `::: `. Mode badges occupy a four-cell column; verify actual glyph width, including the pause-symbol variant, rather than trusting a nearby comment. Focus changes colors without shifting text. [C06]
- **Tabs:** rounded upper frame; active bottom opens into the content, inactive bottom closes; joins and bottom corners change intentionally. Blurred pane uses more-subtle border color. [C06]
- **Attachment chips:** icon, label and remove slot form a compound chip. Remove and delete-mode slots have identical padding and trailing gap, so delete mode never shifts adjacent chips. [C06]
- **Status/help:** key labels and descriptions use different muted roles; hints come from the binding model. Status tag/body colors differ by success/info/warning/error. Fregat retains terminal-native placement rather than copying browser toast behavior. [C06][F04]

Reference symbols [C03]: focused rail `▌`, thin rail `│`, scrollbar thumb `┃`, track `│`, pending `●`, success `✓`, error `×`, radio `◉`/`○`, image `■`, text `≡`, skill `▲`, remove `✕`. Status must remain interpretable without color. These are terminal-cell fixtures, not typography screenshots alone.

### Syntax and diff color details

The default theme overrides generic syntax roles: preprocessing Bengal #FF6E63; reserved/namespace keywords Pony #FF4FBF; type keywords Guppy #7272FF; operators Salmon #FF7F90; tags Mauve #D46EFF; attributes Hazy #8B75FF; classes Salt #F7F6FB; strings Cumin #BF976F. Markdown links use Zinc #10B1AE and images Cheeky #FF79D0. Preserve the difference between a generic token mapping and theme-specific overrides. [C05][D06]

Diffs have six distinct insert/delete roles: foreground, code/sign background and gutter background. Derived backgrounds use CIELAB blending via Lip Gloss, with insertion code/gutter fractions 0.20/0.13 and deletion code/gutter 0.15/0.08 over base background. RGB alpha is not equivalent. Equal/missing/divider/file-name rows have separate styles. Capture exact derived colors through a licensed/approved reference fixture before claiming a match. [C06][C15]

## Measurable reference constants

These 32 values are source findings, not universal bubli limits. Application state and call-site overrides still matter.

| Constant | Value | Qualification/source |
| --- | --- | --- |
| Compact width breakpoint | 120 columns | Layout reference, not toolkit minimum [C07] |
| Compact height breakpoint | 30 rows | Combined layout predicate needs state audit [C07] |
| Sidebar width | 32 columns | Source-search finding, not complete layout extraction [C07] |
| Textarea minimum | 3 rows | Main composer [C07] |
| Textarea maximum | 15 rows | Main composer [C07] |
| Editor surrounding allowance | 2 rows | Attachment/top and bottom allowance [C07] |
| Large-paste newline threshold | More than 10 | Strict greater-than [C07] |
| Large-paste width threshold | More than 1,000 columns | Not bytes or UTF-16 length [C07] |
| Standard dialog maximum width | 70 columns | Specialized dialogs may differ [C13] |
| Standard dialog height | 20 rows | Default, clamp to available area [C13] |
| Async dialog quiet grace | 425 ms | Only grace-opened dialogs [C13] |
| Async dialog absolute cap | 1,500 ms | Measured from opening [C13] |
| Same-ID immediate reopen window | 500 ms | Skips grace [C13] |
| Completion minimum width | 10 columns | Placement still clamps [C14] |
| Completion maximum width | 100 columns | Configured bound [C14] |
| Completion minimum height | 1 row | Configured bound [C14] |
| Completion maximum height | 10 rows | Configured bound [C14] |
| Chat item gap | 1 row | Custom list [C09] |
| Multi-click threshold | 400 ms | Double/triple click detection [C09] |
| Click tolerance | 2 cells per axis | Coordinate tolerance [C09] |
| Default scrollbar hide | 2,000 ms | After activity; always/never distinct [C09] |
| Resize settle delay | 120 ms | Before deferred cache warming [C09] |
| Warm batch size | 25 messages | Implementation reference, not required algorithm [C09] |
| Animation clock | 20 FPS | Not renderer FPS [C12] |
| Ellipsis interval | 400 ms | Eight animation steps [C12] |
| Default scramble width | 10 characters | Used when requested size is below one [C12] |
| Maximum birth steps | 20 frames | Approximately one second at reference rate [C12] |
| Collapsed tool body | 10 lines | Show all 11 if only one would be hidden [C16] |
| Tool body indentation | 2 columns | Reference body padding [C16] |
| Markdown list-level indentation | 2 columns | Profile constant [C03][C06] |
| Markdown code-block margin | 2 columns | Profile constant [C03][C06] |
| Diff default tab width | 8 columns | Constructor default; audit callers [C15] |

## Interaction contracts

**Input routing.** Reference keys include Enter send; Shift+Enter/Ctrl+J newline, with capability-aware help; Tab changes focus, Shift+Tab switches mode; Ctrl+P commands, Ctrl+L models (Ctrl+M alias), Ctrl+S sessions, Ctrl+O editor, Ctrl+Z suspend. Chat navigation includes arrows/j/k, shifted item movement, paging, home/end, copy, expansion and horizontal scroll. A key table alone does not define precedence: Space/space aliases and Escape have overlapping meanings, so trace the active-state dispatch and test it. Fregat command choices remain app-owned and rebindable. [C08]

**Overlays.** Only the top dialog consumes input. Ordinary dialog opening is immediate; async grace absorbs keystrokes until a 425 ms quiet period or 1,500 ms cap. Same-ID reopen within 500 ms skips the guard. Closing a child must not also close its parent. Cursor/focus return and pointer interception need replay evidence. [C13]

**Scroll/follow.** Scrolling up stops following new messages; reaching the bottom enables following under the reference tests. Streaming growth and composer growth retain a bottom-following anchor. Readers above the end keep their place during updates, prepends and expansion. Resize must avoid full-history work every frame; the reference defers cache warming and suppresses exact scrollbar scans during resize. These are observable/performance goals, not a requirement to clone its cache implementation. [C09][C10]

**Completions.** Exact basename/stem, basename prefix and matching path segment rank ahead of fallback candidates. The popup uses a reverse filterable list with no row gap. Matching algorithm, ranking, placement, insert-without-close and typing/navigation remain distinct behaviors to validate. [C14]

**Clipboard/host.** Read text, read image, write text, unsupported and failed outcomes are distinct. A local TUI and remote server cannot silently exchange responsibility for clipboard or filesystem paths. External-editor launch, cancel and failure restore terminal modes and draft state. Kitty image transmission, block fallback and aspect-ratio fitting share one image identity/lifetime. [C17][C18][D07]

## Markdown and React contract

Production engine: owned tree-sitter-md + tree-sitter-x. Parsing provides semantic/source data; styling, terminal layout and React remain downstream. See [the parser plan](https://github.com/ShaulLavo/tree-sitter-md/pull/5) for the full semantic/profile gate.

- **Standard:** normal Markdown soft-break behavior and rich syntax.
- **User:** preserve intentional single newlines from the textarea.
- **Quiet/thinking:** muted body and subtle background, while inline code still has visible contrast.
- **Plan:** H2 has no literal hash prefix, H3/H4/H5 use 2/4/6-space hierarchy; H1 retains its separate badge. [C06][C11]

Reference details: blockquotes use `│ ` with indentation; unordered lists use `• `; task markers are `[✓] ` / `[ ] `; H2-H5 in standard Markdown retain their heading prefixes; H1 has padded colored badge treatment. Links, images, code, tables, definition lists, hard/soft breaks and preserved user newlines require semantic tests, not just color tests. Glamour enables Goldmark GFM and definition lists, and automatic heading IDs. Goldmark's GFM bundle includes linkify, tables, strikethrough and task lists. [D05][C06][C11]

The reference pads inline code with NBSP plus text-presentation selector U+FE0E to distinguish decoration from real text when copying. Reproduce source-faithful copy, not necessarily this sentinel implementation. Real NBSP must survive; UI padding, rails and metadata must not leak. Emoji-presentation selector U+FE0F is not interchangeable in cell measurement. [C03]

The parser's current README reports 676/676 normalized cases. That does not prove decoded text, list tightness, URLs/titles, cell layout or rendered HTML parity. Extend fixtures beyond the old normalizer. `Definition` is not definition-list support; packed decoration metadata is not a complete semantic API. Table alignment must work beyond the earlier 12-column packed representation. [Parser types](https://github.com/ShaulLavo/tree-sitter-md/blob/5dd917ae70eea5f6a38a2ba8825ce6d3baca697d/js/index.d.ts), [methodology](https://github.com/ShaulLavo/tree-sitter-md/blob/55ce090f6ac195bceb314d7cb29760295ce8df2d/bench/constructs.mjs).

Arbitrary React means application-supplied components mounted through the existing reconciler, with hooks, context, effects, events and state. Prefer normal composition or same-root portals; no isolated React root per block. Block overrides may be interactive terminal subtrees; inline overrides obey inline layout rules. Stable semantic identity, stale-result rejection, asynchronous measurement and virtualization unmount policy are explicit. Never evaluate JSX/MDX from model content. Remove Marked public types with its parser implementation. [B02], [React renderer](https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/react/src/reconciler/renderer.ts).

Existing Plan 189's footnotes, math, CJK flanking, wiki links, callouts and highlights remain required in that plan. They are separate configurable profiles, not silently cancelled and not all prerequisites for initial Crush-profile adoption.

## Fregat gap and reuse map

- Shared palette conversion and no-color/reduced-motion capabilities exist [F02]. Extend role granularity without splitting the app palette owner.
- Select already owns wrap selection and search-input arrow routing [F03]. Generalize and restyle; do not call it absent.
- Dialog already portals/clamps and integrates app command hints [F04]. Extract generic surface/stack behavior while retaining an app command adapter.
- Composer already handles native textarea state, paste, completion and draft integration [F05]. Improve wrapped sizing and visuals without replacing application draft semantics.
- Message rows already render streaming Markdown [F06]. Replace their content engine/profile wiring, not the backend event model.
- Timeline layout estimates height from source text and caps windows at 40 items [F07]. Test actual rendered heights, tall blocks and anchors before choosing a replacement algorithm; no untested bug claim.
- Viewer highlighting uses Shiki/Oniguruma and dark-plus/light-plus [F08], separate from Tree-sitter Markdown/code. Shared semantic role policy is required; replacing every highlighter is not automatically required.
- The fork already contains native components [B01], configurable Diff bands [B03], runtime pins [B04] and a keymap package [B05]. Existing capabilities remain assets; exports are not proof of parity.

## Validation and release gates

| Gate | Fixture and evidence |
| --- | --- |
| Visual hierarchy | Controls gallery and Agent screen at 40x12, 80x24, 100x30, 120x30 and 160x50; test both sides of 120-column/30-row reference breakpoints |
| Theme/capability | Reference preset, Fregat palette, light/dark, truecolor, 256/16-color and no-color; visible focus/selection without relying on color alone |
| Keyboard/focus | Text-entry exclusions, enhanced/legacy keys, popup/dialog precedence, nested dismissal, dynamic hints, terminal-owned input |
| Editor | Fast typing immediately followed by submit, wrapping, selection, undo/redo, history boundaries, large paste and attachments; cancellation preserves drafts |
| Semantics | Official CommonMark/GFM plus pinned Goldmark/Glamour profiles, decoded URLs/text, numbering/tightness, definition lists, table cells/alignment and newline policy |
| Streaming | All single split points for selected fixtures plus seeded multi-chunk streams, incomplete structures, Unicode boundaries and nonlocal references; final equals fresh |
| React | Hooks/context/effects, event handling, state survival, stable keys, error boundaries, custom block resizing and explicit virtualization disposal |
| Scroll/copy | Prepend, streaming growth, image load, expansion and repeated resize without anchor loss; tall item; source-faithful Unicode/code/link/cross-message copying |
| Motion | Deterministic seeds/fake clock, no tick storms, no hidden/finished spinners, reduced/static mode and stale timer rejection |
| Diff/media | Odd-width split, tabs, code/sign/gutter bands, context expansion, sync scroll, unknown languages, Kitty/fallback resize and cleanup |
| Host lifecycle | Exit, interrupt, suspend/resume, external-editor fail/cancel, clipboard local/remote distinction, supported/unsupported capabilities |
| Performance | Separate parse, semantic conversion/transfer, React reconciliation, layout and terminal output; cold/warm history, bounded cache/memory and large-input counters |
| Distribution | Fresh install and packed artifacts, one React instance, compatible Tree-sitter module instances per realm, worker/WASM paths, matching native assets, Bun/supported Node |

Attach terminal-cell snapshots including attributes/cursor, input traces and real terminal captures where protocol behavior matters. Screenshots depend on font/emulator; compare like-for-like. Do not mistake a text-only golden for appearance parity. Fake timers remove random motion from snapshots. Use synthetic local fixtures; no model credentials, spending or private conversation data is needed.

Every deliberate departure gets: capability ID, source/version, reference behavior, chosen behavior, reason and regression test. A missing runtime observation remains open evidence, not an invented exact value. A failed known-bad control invalidates its gate. Independent correctness/reproduction review is required before claiming broad parity.

## Provenance and license boundary

Crush's inspected snapshot carries FSL-1.1-MIT terms [C19]; Bubbles' inspected version has MIT terms [D08]. Do not assume all custom Crush code, tests or assets inherit a dependency's license. Record per-source licenses and attribution, treat custom code as reference unless reuse is explicitly cleared, and review distribution implications before mechanically translating implementation. Independent behavior fixtures and bubli branding are the default. This plan is not legal clearance.

## File-level source ledger

The 41 reference IDs below preserve the original audit's sources. Anchored ranges identify inspected portions, not claims that every branch in a large file was traced. The full-file links for ui.go and quickstyle.go are navigation anchors; specific constants/recipes above were inspected. New parser/editor planning links are listed inline separately.

[C01]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/go.mod
[C02]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/AGENTS.md
[C03]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/styles.go#L1-L260
[C04]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/palette.go
[C05]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/themes.go#L1-L270
[C06]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/quickstyle.go
[C07]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/model/ui.go
[C08]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/model/keys.go
[C09]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/model/chat.go#L1-L230
[C10]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/model/layout_test.go#L1-L220
[C11]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/common/markdown.go
[C12]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/anim/anim.go#L1-L260
[C13]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/dialog/dialog.go#L1-L250
[C14]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/completions/completions.go#L1-L220
[C15]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/diffview/diffview.go#L1-L180
[C16]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/chat/tools.go#L1-L190
[C17]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/clipboard/clipboard.go
[C18]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/image/image.go#L1-L150
[C19]: https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/LICENSE.md
[D01]: https://github.com/charmbracelet/bubbletea/blob/v2.0.9/go.mod
[D02]: https://github.com/charmbracelet/bubbles/blob/v2.2.1/go.mod
[D03]: https://github.com/charmbracelet/lipgloss/blob/v2.0.6/go.mod
[D04]: https://github.com/charmbracelet/glamour/blob/v2.0.1/go.mod
[D05]: https://github.com/charmbracelet/glamour/blob/v2.0.1/glamour.go#L1-L230
[D06]: https://github.com/charmbracelet/x/blob/exp/charmtone/v0.1.0/exp/charmtone/charmtone.go
[D07]: https://github.com/golang-design/clipboard/blob/v0.9.0/go.mod
[D08]: https://github.com/charmbracelet/bubbles/blob/v2.2.1/LICENSE
[O01]: https://github.com/anomalyco/opentui/blob/32d67005d0dd61e63dff24756d5b9061bc504325/packages/core/src/index.ts
[B01]: https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/core/src/renderables/index.ts
[B02]: https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/core/src/renderables/Markdown.ts#L1-L260
[B03]: https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/core/src/renderables/Diff.ts#L1-L145
[B04]: https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/core/package.json#L1-L115
[B05]: https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/keymap/README.md#L1-L88
[F01]: https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/package.json
[F02]: https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/theme/utils/theme.ts#L1-L90
[F03]: https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/components/select.tsx
[F04]: https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/components/dialog.tsx
[F05]: https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/agent-stage/components/composer.tsx#L1-L190
[F06]: https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/agent-stage/components/timeline-row.tsx
[F07]: https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/agent-stage/state/timeline-layout.ts
[F08]: https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/viewer/state/syntax.ts
