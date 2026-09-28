# bubli capability catalog

The original 83 research IDs are retained below. They describe capabilities and app compositions, not 83 new widgets, packages or mandatory rewrites. Every row is unverified at runtime until its fixture is executed. Existing source is evidence of a starting point, not equivalent behavior.

[Implementation units B0-B7](../../plans/bubli-experience.md) own reusable toolkit work. [Fregat Plan 201](https://github.com/ShaulLavo/fregat/blob/docs/bubli-plans-2026-09-28/plans/201-bubli-tui.md) owns app work. [Parser PR 5](https://github.com/ShaulLavo/tree-sitter-md/pull/5) owns semantic Markdown. [Reference specification](reference-spec.md) preserves precise constants, profiles, dependencies, 41 original source references and validation conditions. [Coordination](README.md) links the complete series.

Dispositions: **reuse/audit** means start from an existing primitive; **extend/generalize** means an existing app/native component is relevant but the generic/reference contract needs work; **add contract** means investigate the named missing abstraction, not assume no code exists; **app** keeps application meaning in Fregat. Reclassify with source and test evidence during B0.

## Foundation: B001-B012

Evidence: [core exports](https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/core/src/renderables/index.ts), [keymap](https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/keymap/README.md), [Crush architecture](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/AGENTS.md), [chat](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/model/chat.go), [Markdown renderer](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/common/markdown.go).

| ID | Capability | Disposition / owner | Contract and acceptance fixture |
| --- | --- | --- | --- |
| B001 | Cell renderer and layout | Reuse/audit core, B0 | Nested clipped boxes, borders and repeated resize leave no stale cells; native/Yoga engine remains the owner. |
| B002 | Terminal lifecycle | Audit core + host, B2/B4 | Exit, interrupt, suspend/resume and editor handoff restore cursor, input and screen exactly once. |
| B003 | Key normalization | Audit core/keymap, B2 | Test Enter/Ctrl+M and Shift+Enter/Ctrl+J under enhanced and legacy protocols without losing supported distinctions. |
| B004 | Focus coordination | Extend toolkit + Fregat bridge, B2 | Composer, chat, sidebar and front dialog have one input owner; open/close returns focus predictably. |
| B005 | Layered key/command routing | Reuse keymap + app adapter, B2 | Priorities, consumed events and hints agree; Space may expand chat without becoming global in text input. |
| B006 | Cell width and segmentation | Audit core, B4 | CJK, combining accents, ZWJ emoji, NBSP and variation selectors retain correct grapheme/word/cell boundaries. |
| B007 | ANSI-safe wrap/truncate | Audit core, B4 | Long styled paths fit narrow cells without split escapes or invalid text boundaries. |
| B008 | Selection-to-source mapping | Extend core/content, B3/B5 | Copy wrapped links, inline code and cross-message content with no rails, metadata or sentinel leakage. |
| B009 | Theme invalidation/caches | Extend content, B1/B3/B5 | Content, width, theme and tokenizer changes invalidate relevant work; no stale code/Markdown/selection colors. |
| B010 | Shared animation clock | Extend motion, B1/B5 | Deterministic frames, no independent leaking timers; hidden/finished items stop and stale ticks cannot restart them. |
| B011 | Highlighting provider boundary | Extend core + app adapter, B4 | Separate runtime, grammars/queries, semantic tokens and palette; same code has a defined mapping in Markdown/diff/viewer. |
| B012 | Color/capability adapter | Generalize app/native, B1/B4 | Truecolor, indexed and no-color modes preserve readable focus, disabled and selection distinctions. |

## Design system: B013-B020

Evidence: [palette](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/palette.go), [themes](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/themes.go), [recipes](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/quickstyle.go), [Fregat palette bridge](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/theme/utils/theme.ts).

| ID | Capability | Disposition / owner | Contract and acceptance fixture |
| --- | --- | --- | --- |
| B013 | Semantic theme contract | Generalize app roles, B1 | Distinct foreground/background ramps, statuses and contrast pairs; map one app palette to every terminal state. |
| B014 | Default Charm-inspired preset | Add recipe layer, B1 | Ordinary input, picker, dialog and messages cohere with minimal props; independent branding and overridable roles. |
| B015 | State-specific style recipes | Add/generalize, B1 | Focused, blurred, inactive-pane, disabled, hovered and negative states change intended colors without layout movement. |
| B016 | Surface/frame recipes | Generalize Box/Dialog, B1 | Frame/padding count inside allocated width; nested content fits a very small dialog. |
| B017 | Focus rail | Add reusable recipe, B1 | User/assistant/tool text keeps identical x-coordinate when switching thin/no rail to thick focus rail. |
| B018 | Gradient title | Add reusable recipe, B1 | Long title truncates to content cells without wrapping through a frame; reference interpolation is measured. |
| B019 | Icons/indicators | Add token inventory, B1 | Status/radio symbols are distinguishable without color and width assumptions are tested across supported terminals. |
| B020 | Motion preference | Generalize app preference, B1 | Explicit reduced/static behavior with fake-clock tests; no accidental renderer FPS cap. |

## Controls: B021-B045

Evidence: [native controls](https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/core/src/renderables/index.ts), [state recipes](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/quickstyle.go), [dialog stack](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/dialog/dialog.go), [completions](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/completions/completions.go), [animation](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/anim/anim.go), [Fregat Select](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/components/select.tsx), [Dialog](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/components/dialog.tsx) and [Composer](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/agent-stage/components/composer.tsx).

| ID | Capability | Disposition / owner | Contract and acceptance fixture |
| --- | --- | --- | --- |
| B021 | Text, box and divider | Reuse + recipes, B1 | Semantic foreground/spacing/clipping; long text and separators stay within allocated cells. |
| B022 | Badge and pill | Add/generalize, B1 | Fixed padding/contrast; state and delete changes do not move neighbors. |
| B023 | Button | Add recipe/behavior, B1/B2 | Focus, blur, inactive-pane, hover and negative styles; keyboard/mouse share enablement and activation. |
| B024 | Radio group | Add/generalize, B1/B2 | Cursor and selected value are separate; navigation alone does not commit a choice. |
| B025 | Multi-choice group | Add/generalize, B1/B2 | Checked values survive navigation; focus is independent; app submits answers. |
| B026 | Tabs/segmented navigation | Extend TabSelect/frames, B1/B2 | Active tab opens into content and blurred states stay distinct; corners align at narrow widths. |
| B027 | Text input | Extend Input, B1/B2 | Focus/blur text, prompt, placeholder, suggestion and block cursor update together. |
| B028 | Masked input | Extend Input, B1/B2 | Secret text stays masked in visual evidence/helper text; storage and submission remain app-owned. |
| B029 | Multiline prompt editor | Generalize textarea/composer, B2 | Dynamic wrapped height, newline/submit keys and latest native value; fast type-and-send loses no characters. |
| B030 | Prompt prefix modes | Add reusable gutter recipe, B2 | App chooses mode; first-line badge and continuation dots retain equal cell width across focus/mode changes. |
| B031 | Prompt history mechanism | Generalize editor + app data, B2 | Up/down inside multiline input remains navigation until the defined history boundary. |
| B032 | Completion popup | Add/generalize, B2 | At most 10 reference rows, on-screen placement, match spans and staged ranking; exact basename outranks path-only match. |
| B033 | Searchable select | Generalize Fregat Select, B2 | Typing and arrow navigation cooperate; focused row/info column style is consistent; empty result has no invalid selection. |
| B034 | Scrollable container | Reuse/extend ScrollBox, B2/B5 | Only focused region handles keyboard scroll; horizontal/vertical clipping remains coherent. |
| B035 | Scrollbar | Extend ScrollBar/recipe, B5 | Thumb/track, 2-second activity hide and always/never modes; explicit resize behavior. |
| B036 | Variable-height list | Audit/add contract, B5 | Stable item identity, measurement/anchor/expansion; tall Markdown and repeated resize do not jump the reader. |
| B037 | Dialog surface | Generalize Fregat Dialog, B1/B2 | Rounded frame, bounded content/title/help; account for border, padding and input prompt width. |
| B038 | Dialog stack/focus return | Add scoped contract, B2 | Top dialog owns input; nested dismissal returns focus and does not dismiss parent. |
| B039 | Async dialog input grace | Add behavior, B2 | 425 ms quiet / 1,500 ms cap / 500 ms same-ID reopen; in-flight typing cannot approve unseen prompts. |
| B040 | Shortcut help bar | Generalize command hints, B1/B2 | Actual and displayed bindings share a source; rebinding/disabling updates hints. |
| B041 | Status strip/toast | Generalize terminal status, B1 | Separate severity variants and clipping; terminal-native placement, readable long messages/no-color errors. |
| B042 | Loading and empty state | Generalize app loaders, B1 | Loading, empty, error and disabled are distinct; pending list is not definitive no-results. |
| B043 | File picker surface | Generalize app picker, B2 | Supplied directory/file data, disabled styles and bounded preview; no generic server/filesystem ownership. |
| B044 | Attachment chips | Generalize composer chips, B2 | Type icon, label, keyboard/remove state and spacing; stable identity and geometry in delete mode. |
| B045 | Working indicator | Add/reference motion recipe, B1 | Theme gradient, 10-character default scramble and plain-running variant; deterministic fake-clock frames and cleanup. |

## Content: B046-B058

Evidence: [bubli Markdown](https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/core/src/renderables/Markdown.ts), [Diff](https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/core/src/renderables/Diff.ts), [Crush Markdown profiles](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/common/markdown.go), [profile recipes](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/quickstyle.go), [code-span copy marker](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/styles/styles.go), [image handling](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/image/image.go), [clipboard](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/clipboard/clipboard.go).

| ID | Capability | Disposition / owner | Contract and acceptance fixture |
| --- | --- | --- | --- |
| B046 | Standard Markdown | Replace parser + extend rendering, M/B3 | Owned semantics, reference blocks/wrap/spacing/links/headings/code; identical fixtures at multiple widths. |
| B047 | User-authored Markdown profile | Add profile, B3 | Preserve authored single newlines while assistant paragraphs retain ordinary Markdown soft breaks. |
| B048 | Quiet Markdown profile | Add profile, B3 | Muted foreground/background retains structure, links and distinguishable inline-code chips. |
| B049 | Plan Markdown profile | Add profile, B3 | H2-H5 hierarchy uses chosen indentation rather than standard literal prefixes; plan and ordinary headings differ intentionally. |
| B050 | Streaming Markdown reconciliation | Extend core/React, M/B3 | Final equals fresh; stable content does not remount for harmless chunks; test splits inside fences, links and tables. |
| B051 | Inline code/source-aware copy | Extend mapping, M/B3/B5 | Decorative chip padding never leaks; real NBSP is preserved; no required sentinel implementation. |
| B052 | Code block | Extend Code, B3/B4 | Language aliases, unknown-language fallback, theme invalidation and horizontal behavior; full-source context where available. |
| B053 | Markdown table | Extend TextTable + semantics, M/B3 | Rows/cells/all-column alignment, fitting and link behavior; long cell, escaped pipe and incomplete stream cases. |
| B054 | Diff viewer | Extend Diff, B4 | Sign/gutter/code bands, source lines, context and anchors; odd split width, tabs and context expansion remain aligned. |
| B055 | Styled command-output view | Generalize core text, B4 | Intentional ANSI16 mapping; preserve supported output style without treating raw output as Markdown or terminal instructions. |
| B056 | Image view/block fallback | Reuse/extend Image, B4 | Cell-aware aspect ratio, supported protocol/fallback and lifecycle; resize/reopen/remove leaves no graphics behind. |
| B057 | Clipboard host service | Generalize host, B2/B4 | Text write/read, image read and unsupported/failure are distinct; local/remote origin is never guessed. |
| B058 | External editor handoff | Generalize host, B2/B4 | Pause/restore renderer, launch/failure/cancel, return content and retain cancelled draft. |

### Additional owner decisions across the content rows

B046-B053 require a renderer-neutral semantic API with revision/coverage, nested structure, decoded text, resolved URLs/titles, list starts/tightness, definition lists, unlimited-by-old-record table alignment and nonlocal invalidation. The parser's compact decoration API remains supported.

B046/B050/B052/B053 also require actual React component overrides: hooks/context/effects/events/state, same-root ownership, block versus inline layout rules, stable identity, height-change feedback and explicit virtualization disposal. Trusted host components render data; Markdown cannot execute JSX/MDX. These are mandatory cross-cutting additions to the original 83-ID inventory, not an optional follow-up hidden outside the plan.

## Fregat application compositions: B059-B078

These rows keep product meaning in Fregat. bubli may provide a reusable visual surface; it must not absorb session, agent, tool-execution, credential or project/worktree logic. Implement only compositions used by Fregat's actual flows; differences from Crush's app structure are permitted and recorded.

Evidence: [Crush custom UI map](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/AGENTS.md), [tool states and collapse rule](https://github.com/charmbracelet/crush/blob/06e50a330e2b05b677726737d06852a35f5ff93f/internal/ui/chat/tools.go), [Fregat rows](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/agent-stage/components/timeline-row.tsx), [timeline](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/agent-stage/components/timeline.tsx), [layout estimation](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/agent-stage/state/timeline-layout.ts).

| ID | Composition | Owner/unit | Contract and acceptance fixture |
| --- | --- | --- | --- |
| B059 | Application frame/header | App, F2 | Reference proportions inform terminal-native Fregat layout; compact boundaries retain prompt/main content without copying branding. |
| B060 | Sidebar/resource rows | App, F2 | Quiet metadata, status dots and explicit focus; sidebar never steals composer typing. |
| B061 | Conversation viewport | App + B5, F2 | Follow/history/anchors and variable height; reading old content is not pulled into current streaming output. |
| B062 | User message surface | App + B3, F2 | Reference rails and authored line breaks; focus leaves text stationary. |
| B063 | Assistant message surface | App + B3, F2 | Pending/complete/cancelled/error hierarchy; finishing removes working state without jumps. |
| B064 | Reasoning/thinking section | App + B3/B5, F2 | Collapsed/expanded subdued body, visible code; expansion preserves reading anchor. |
| B065 | Plan card | App + B3, F2 | Rounded frame, deliberate padding/plan profile and no unintended full background fill. |
| B066 | Generic tool-call surface | App + generic UI, F3 | Awaiting permission/running/success/error/cancelled and missing-versus-empty result are distinct; 11 lines stay fully visible. |
| B067 | Shell/job output card | App + B4, F3 | Command/output/exit metadata; running, empty success, nonzero exit and cancellation differ. |
| B068 | File operation/change card | App + B4, F3 | Preserve per-operation/file/source-line identity in code/diff; no conflated multi-file edits. |
| B069 | Search result card | App, F3 | Path/title/excerpt/count/truncation; long paths, empty and partial results remain understandable. |
| B070 | Fetch/web result card | App, F3 | Source/title/link/result presentation; malformed URLs do not invoke a new fetch backend. |
| B071 | Subagent/nested task card | App, F3 | Existing nested identity/progress/result; completion leaves no ghost spinner. |
| B072 | Todo/task list | App + generic choices, F3 | Pending/in-progress/completed, ratio and notes remain distinguishable without color. |
| B073 | Diagnostic/reference card | App + B4, F3 | Severity/location/details survive truncation; no duplicate LSP owner. |
| B074 | Unknown extension-tool fallback | App + generic UI, F3 | Safe bounded structured/text content; malformed or unknown payload cannot crash the conversation. |
| B075 | Command/model/session pickers | App + B2, F2/F3 | Shared search/navigation controls, distinct app data/actions; settings bindings remain authoritative. |
| B076 | Approval prompt | App + B2, F3 | Frontend focus/grace protects from in-flight typing; permission decisions remain with app/backend. |
| B077 | Question/form prompt | App + B2, F3 | Single/multi/text/masked values and confirmation are explicit; tab switching preserves answers without submission. |
| B078 | Auth/setup/quit surfaces | App + B2, F3 | Reuse only presentation needed by existing flows; no duplicate account/session architecture. |

## Shared integration: B079-B083

Evidence: [Fregat dependencies](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/package.json), [palette](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/theme/utils/theme.ts), [viewer highlighting](https://github.com/ShaulLavo/fregat/blob/5a5c512a75ed635e95b0e9dcb71c28823abd71a0/apps/tui/src/viewer/state/syntax.ts), [keymap](https://github.com/ShaulLavo/bubli/blob/57e5924e4e9813b31af336371b53c26577d46695/packages/keymap/README.md).

| ID | Capability | Owner/unit | Contract and acceptance fixture |
| --- | --- | --- | --- |
| B079 | Shared palette bridge | Fregat + B1, F1 | One application palette maps into richer terminal roles without importing browser CSS or a second theme owner. |
| B080 | Shared command bridge | Fregat + B2, F1 | One coordinated key/focus route; overrides, disabled actions, hints and text-entry exclusions agree. |
| B081 | Shared highlighting policy | Fregat/Singapore + B4, F4 | Deliberate semantic roles/fallbacks across Tree-sitter and existing Shiki; no silent engine substitution. |
| B082 | Renderer-independent presentation models | Fregat client-core, F2/F3 | The same stored conversation/events drive web and terminal projections; backend contracts do not change. |
| B083 | Embedded terminal surface | Core + Fregat host, B4/F3 | Keep existing emulator/session ownership; inner control bytes survive and global chat keys do not intercept them. |

## Closing a row

For each delivered capability, append implementation commit(s), narrowed fixture path, recorded result, remaining manual/platform checks and any intentional deviation to its owning plan/evidence record. Do not mark all 83 complete because a gallery renders. Parser semantic equivalence, React lifecycle, terminal output, host behavior and actual Fregat use each need their own proof.
