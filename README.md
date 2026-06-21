<p align="center">
  <img src="screenshots/haiku-2.png" alt="pi-haiku — footer + theme in action" width="100%">
</p>

# pi-haiku

**A warm, minimal theme plus a structured footer and keymap header replacement for [Pi coding agent](https://pi.dev).** Replaces Pi's built-in startup banner and status line with dense, color-coded layouts that stay readable at any terminal width — and ships with the matching **`haiku`** theme to tie it all together.

[![npm version](https://img.shields.io/npm/v/pi-haiku?style=for-the-badge)](https://www.npmjs.com/package/pi-haiku)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![Pi package](https://img.shields.io/badge/pi--dev-package-7bc4b4?style=for-the-badge)](https://pi.dev/packages)

## Preview

The startup header — a tight, grouped keymap that wraps gracefully:

<p align="center">
  <img src="screenshots/haiku-1.png" alt="pi-haiku startup header" width="100%">
</p>

The full layout in action — theme, footer with live stats, working timer, and context bar:

<p align="center">
  <img src="screenshots/haiku-2.png" alt="pi-haiku footer in use" width="100%">
</p>

## Install

```bash
pi install npm:pi-haiku
```

That's it. On the next `pi` launch the theme switches to `haiku` and the footer / keymap header take over. No settings to flip, no flags to pass.

If you'd rather install without touching your user config (current run only):

```bash
pi -e npm:pi-haiku
```

## Features

### Footer — three lines, dense but readable

The built-in footer gets out of the way. The haiku footer keeps the same vertical real estate and triples the signal.

- **Line 1 — where you are, what you're talking to**
  - **Left:** `cwd` (with `$HOME` collapsed to `~`), then `branch` and session `name` when available.
  - **Right:** `provider` (color-coded per provider — Anthropic teal, OpenAI green, Google gold, Bedrock purple, Copilot link-blue, DeepSeek low-thinking, xAI/Groq red, OpenCode Go teal) ▪ `model-id` ▪ `effort` (when the model reasons — color-coded across `minimal → xhigh`).
- **Line 2 — what it's costing**
  - **Left:** live `working` timer (ms → s → m → h format) while the agent runs, or `worked for …` after the turn finishes.
  - **Right:** aggregated `in ↑N` / `out ↓N` / `cache hit N%` / `cost $X.XXX` (with `(sub)` flag for OAuth subscriptions) and a context-usage bar `[████░░░░] N.N% · Nk/Nk` whose color shifts from accent → warning → error as you approach the context window.
- **Line 3 (optional):** raw `extensionStatuses` — anything other extensions push to the footer is surfaced underneath, pipe-separated and sorted.

Both sides of each line are right-aligned with a graceful left-truncation fallback — if the metadata would overflow, the right side wins, the left gets an ellipsis, and nothing wraps or breaks the layout.

### Header — startup keymap that breathes

Replaces Pi's default version banner + keybinding wall with a tight, color-coded, wrapping layout.

- **Identity line:** bold accent `pi` + muted version, no leading blanks.
- **Key map, grouped by intent:** `control` / `models` / `view` / `input` — gold eyebrows, accent keys, dim verbs, dim dot separators.
- **Wraps to width:** pairs flow greedily into the content column, wrapping under the eyebrow when they would overflow. Continuation lines align with the first content column so the block reads as a tidy table.
- **Hint line:** `Pi can explain its own features and look up its docs.` — an invitation to ask the agent about itself.

### `haiku` theme — warm dark, low eye-burn

A Gruvbox-adjacent dark palette tuned for the footer's color coding.

- **Base:** deep blue-violet backgrounds (`#181a25` → `#323552`) with warm off-white text (`#d1cec0`).
- **Accent:** muted teal `#7bc4b4` — readable on both bright and dim surfaces.
- **Synergy:** gold for warnings / numbers, soft red for errors / removed, soft green for success / added, pink for inline code, blue for low thinking, purple for high thinking, xhigh at pink. The thinking-level ramp flows from `bg4 → textDim2 → blue → teal → purple → pink` so the editor border tells you the reasoning level at a glance.
- **Code blocks:** warm cream content (`#d4be98`) on a slightly lighter background with a muted border — no more razor-white blocks floating in the dark.

### Toggle command

```text
/haiku
```

Switches the footer and header back to Pi's defaults. Run it again to re-enable. The theme is left as-is on toggle (it's an independent setting under `/settings`).

### Clean startup

On initial launch the extension clears the visible screen (not scrollback) so the shell prompt and any pre-Pi terminal output are out of view — but the normal screen buffer is preserved, so the mouse wheel still scrolls the page.

### Per-event reactivity

- `thinking_level_select` → re-renders footer so the effort label color updates immediately.
- `model_select` → re-renders footer so the new provider/model colors are correct.
- `agent_start` / `agent_end` → manages the working timer with a 250 ms tick, surfaces the final duration as a `Worked for Ns` notification and working message, and clears timers on shutdown.

## Compatibility

| Pi version | Status |
|------------|--------|
| 0.79.x     | ✅ Tested |

The extension imports `ExtensionAPI`, `ExtensionContext`, `Theme`, `ThemeColor`, and `VERSION` from `@earendil-works/pi-coding-agent`, `AssistantMessage` and `ThinkingLevel` from `@earendil-works/pi-ai`, and `truncateToWidth` / `visibleWidth` from `@earendil-works/pi-tui` — all core pi packages, no external runtime dependencies.

## Files

```
pi-haiku/
├── package.json        # pi-package manifest, peer deps, gallery image
├── README.md
├── LICENSE             # MIT
├── extensions/
│   └── haiku.ts        # the footer + header + /haiku toggle
├── themes/
│   └── haiku.json      # the warm dark theme
└── screenshots/
    ├── haiku-1.png     # startup header preview
    └── haiku-2.png     # footer + theme in action
```

## License

MIT
