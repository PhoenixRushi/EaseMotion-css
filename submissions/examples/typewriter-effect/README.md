# Typewriter Effect

Closes #86793

A monospace typewriter effect that types a headline (and a secondary line)
character by character, with a blinking caret — no JavaScript required.

## What it is

- **Category:** Text
- **Dependency-free:** the typing effect itself is pure CSS (`@keyframes`
  + `steps()`). A small (~25 line) inline vanilla-JS snippet powers the
  interactive controls below — no external library, framework, or build
  step of any kind.

## How it works

### The CSS effect
1. Each text element starts at `width: 0` with `overflow: hidden` and
   `white-space: nowrap`, so its content is fully clipped.
2. A `width: 0 → 100%` animation runs on a `steps(N, end)` timing function
   (where `N` comes from the `--ease-tw-steps` custom property, set per
   element to match that string's character count), which makes the
   reveal jump character-by-character instead of scaling smoothly.
3. A `border-right` acts as the blinking caret, animated on its own,
   independent timing so it keeps blinking after typing finishes.
4. The secondary line's animation is delayed until the heading's typing
   animation completes, so the two lines type in sequence.

### The interactive layer
- Type any headline (up to 60 characters) into the input and press
  **Enter** / **Type it** — the JS updates the text content, recalculates
  `--ease-tw-steps` for the new length, and replays the CSS animation by
  toggling `animation: none` and forcing a reflow (`void el.offsetWidth`)
  before restoring it. This is the standard vanilla-JS technique for
  restarting a CSS keyframe animation without any animation library.
- **↻ Replay** re-runs the current text without changing it.

## Accessibility

- Respects `prefers-reduced-motion: reduce` — text renders instantly at
  full width with no animation and no caret.
- Respects `forced-colors` (Windows High Contrast Mode) — the caret uses
  `CanvasText` instead of a hardcoded color.
- Text remains real, selectable text at all times (not an image or canvas
  rendering), so it's screen-reader and copy/paste friendly throughout.

## Usage

Open `demo.html` directly in a browser — no build step, no server, no
dependencies to install.

```html
<h1 class="ease-typewriter-rushi" style="--ease-tw-steps: 12;">Hello there!</h1>
<p class="ease-typewriter-rushi ease-typewriter-rushi--sub" style="--ease-tw-steps: 8;">
  Nice to meet you
</p>
```

Set `--ease-tw-steps` to the character count of your text so the reveal
lands on whole-character boundaries. The demo's inline script does this
automatically for any text typed into the input field.

## Files

```
typewriter-effect/
├── demo.html
├── style.css
└── README.md
```
