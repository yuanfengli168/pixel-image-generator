# Design Decisions

> Why things are the way they are.

---

## Single HTML File

The entire app lives in one `index.html`. No build step, no bundler, no `node_modules`.

**Why:**

- The app is fundamentally a form + an image. That doesn't justify a framework.
- A single file is trivially deployable — drag it anywhere, open it in any browser, host it on any static server.
- Zero build tooling means zero tooling rot. This file will still work in 5 years.
- It's easier to read, fork, and learn from. The whole implementation fits in one scroll.

The tradeoff: no component reuse, no hot reload, no TypeScript. For an app this size, that's a good deal.

---

## No Framework

No React, Vue, Svelte, or any JS framework.

**Why:**

- The interactivity is minimal: a textarea, three toggle buttons, a button, and a dynamic output div. Vanilla DOM manipulation handles this in ~80 lines.
- Frameworks add 40–300 KB of runtime for problems we don't have.
- No framework = no version lock, no breaking changes, no security advisories on dependencies.

If this grew into a multi-page app with shared state, a framework would make sense. It doesn't, so it doesn't.

---

## Dark / Neon Pixel Aesthetic

The visual design is intentional, not decorative.

**Why this aesthetic:**

- The app generates pixel and retro-style images. The UI should feel like the thing it makes — a dark CRT monitor, glowing neon, 8-bit grid. The medium matches the message.
- Press Start 2P is the most recognizable retro-game font on the web. Users immediately understand the vibe before reading a word.
- Scanline overlay, pixel grid background, and box-glow borders are CSS-only — no images, no canvas, no overhead.

**Deliberate choices:**

- `image-rendering: pixelated` on the output image — prevents browser smoothing from blurring the pixel art aesthetic
- CRT flicker animation on the title — subtle enough to be atmospheric, not annoying
- Accent corner square (pure CSS `::after`) — a small pixel-art flourish that breaks the otherwise plain card border

---

## UX Choices

### Style Selector (buttons, not a dropdown)

Three full-width toggle buttons instead of a `<select>`.

- Dropdowns hide their options. Buttons show all three choices at once, which helps users who don't know what "Anime" or "Meme" style means — the emoji and subtitle give context.
- Visual active state (glow + background) makes the current selection unambiguous.
- Touch-friendly. No tiny dropdown arrow to tap.

### Character Counter

Live counter, warns at 170/200.

- Pollinations.ai encodes the prompt into a URL. Very long prompts can cause request issues. The cap protects against that.
- Warning color at 170 (not 200) gives the user time to edit before hitting the wall — same pattern as Twitter's character counter.

### Loading State

Animated progress bar + blinking status text during generation.

- Image generation takes 3–12 seconds depending on server load. A static "loading…" message feels broken. Motion confirms the app is alive.
- The progress bar uses a CSS animation that reaches ~95% and stops — it doesn't fake completion. This is honest UX: we don't know when the image will arrive, so we don't pretend we do.
- The 15-second timeout fires an explicit error message. Users shouldn't stare at a spinner forever.

### Download Button

Fetch-then-blob approach, saves as `generated-image.png`.

- `<a download href="...">` doesn't reliably trigger a download for cross-origin URLs (Pollinations.ai is a different domain). The fetch → blob → object URL approach forces a true file download in all modern browsers.
- Filename is fixed as `generated-image.png` — predictable, no timestamp clutter.

---

## What Was Left Out of MVP (and Why)

| Feature | Why excluded |
|---|---|
| **Prompt history / gallery** | Requires localStorage or a backend. Adds complexity, raises privacy questions. Not needed for a single-session tool. |
| **Image size selector** | More controls = more cognitive load. 512×512 is the Pollinations.ai sweet spot for speed vs quality. |
| **Negative prompt field** | Power-user feature. Most users don't know what a negative prompt is. Keep the primary path clean. |
| **Share button** | The image URL is already shareable. Adding a "copy link" button would require explaining why the URL changes on every generate. |
| **User accounts / saved images** | Requires a backend. Out of scope for a zero-dependency static app. |
| **Multiple image results** | Would require parallel API calls and a grid UI. Adds latency and complexity. Start with one, do it well. |
| **Mobile-specific layout** | The layout is responsive via `clamp()` and `grid`. A separate mobile UI wasn't needed. |

---

*Good design is mostly knowing what to leave out.*
