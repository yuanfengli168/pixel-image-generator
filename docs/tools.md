# Tools & Development Setup

> Everything used to build and ship this project.

---

## Runtime Tools (Used in the App)

| Tool | Role | Cost | Docs |
|---|---|---|---|
| [Pollinations.ai](https://pollinations.ai) | AI image generation API | Free | [pollinations.ai](https://pollinations.ai) |
| [Press Start 2P](https://fonts.google.com/specimen/Press+Start+2P) | Retro pixel font | Free | Google Fonts |
| Browser `fetch` + Blob API | Force-download generated images | Built-in | MDN |
| CSS `image-rendering: pixelated` | Preserve pixel crispness | Built-in | MDN |

No npm packages. No runtime dependencies. That's the whole list.

---

## Build & Deploy Tools

| Tool | Role |
|---|---|
| `git` | Version control |
| GitHub | Repo hosting |
| GitHub Pages | Static site hosting (auto-deploy from `main`) |
| GitHub REST API | Enable Pages programmatically (`POST /repos/.../pages`) |
| `curl` | API calls during deploy automation |

No CI/CD pipeline. No workflow YAML. GitHub Pages picks up every push to `main` automatically.

---

## Development Workflow

Since there's no build step, the dev loop is as fast as it gets:

```bash
# 1. Clone
git clone https://github.com/yuanfengli168/pixel-image-generator.git
cd pixel-image-generator

# 2. Edit
# Open index.html in your editor of choice

# 3. Preview
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows

# 4. Commit and push — Pages deploys automatically
git add index.html
git commit -m "your change"
git push
```

No `npm install`. No `npm run dev`. No waiting for a bundler.

---

## Recommended Editor Setup

Any editor works. If you want tooling hints:

**VS Code extensions:**
- `esbenp.prettier-vscode` — auto-format HTML/CSS/JS on save
- `ritwickdey.LiveServer` — live reload on save (useful if `open index.html` doesn't auto-refresh)
- `bradlc.vscode-tailwindcss` — not used here, but relevant if you ever add a framework

**Settings worth enabling:**
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```

---

## Browser Compatibility

Tested and works in:

| Browser | Support |
|---|---|
| Chrome / Edge 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Safari 14+ | ✅ Full (`image-rendering: pixelated` supported) |
| Mobile Chrome/Safari | ✅ Responsive layout works |

**Known quirks:**
- `image-rendering: crisp-edges` is the Firefox equivalent of `pixelated` — both are set as a fallback chain in the CSS
- The fetch-blob download may open a new tab instead of downloading on some mobile browsers (OS restriction, not a bug)

---

## Extending the Project

### Add a new style in 2 steps

**Step 1 — Add the modifier:**
```js
// In index.html, find STYLE_MODIFIERS:
const STYLE_MODIFIERS = {
  pixel: 'pixel art style, 16-bit, retro game',
  anime: 'anime style, manga, cel shading, vibrant colors',
  meme:  'meme format, internet humor, flat style',
  oil:   'oil painting, impressionist, thick brushwork, textured canvas'  // ← new
};
```

**Step 2 — Add the button:**
```html
<!-- In .style-grid: -->
<button class="style-btn" data-style="oil" onclick="selectStyle(this)">
  <span class="icon">🎨</span>
  <span class="name">OIL PAINT</span>
  <span class="sub">impressionist</span>
</button>
```

Done. No other changes needed.

### Swap the image API

Replace the URL in `generate()`:

```js
// Current (Pollinations.ai):
const imgUrl = `https://image.pollinations.ai/prompt/${encoded}?width=512&height=512&nologo=true&seed=${seed}`;

// Example swap — any URL that returns a direct image:
const imgUrl = `https://your-api.example.com/generate?prompt=${encoded}&size=512`;
```

The loading bar, timeout guard, error handling, and download button all work with any URL that resolves to an image.

### Change output dimensions

```js
// In generate():
const imgUrl = `https://image.pollinations.ai/prompt/${encoded}?width=768&height=768&nologo=true&seed=${seed}`;
//                                                                      ↑             ↑
```

Square outputs work best for the pixel aesthetic. Widescreen (768×432) works too.

---

## Project File Structure

```
pixel-image-generator/
├── index.html          ← entire app (HTML + CSS + JS)
├── README.md           ← project overview and quickstart
├── LICENSE             ← MIT
└── docs/
    ├── design.md       ← design decisions and what was left out
    ├── resources.md    ← API choices, Pollinations.ai reference, hosting
    └── tools.md        ← this file
```

Flat and readable. No generated files, no lock files, no hidden config.

---

## License

[MIT](../LICENSE) — do whatever you want, just keep the attribution.
