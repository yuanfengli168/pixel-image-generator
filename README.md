# 🎮 Pixel Style Image Generator

> *Type a prompt. Pick a style. Watch the pixels come alive.*

A retro-aesthetic, browser-based AI image generator powered by [Pollinations.ai](https://pollinations.ai). No backend. No API key. No install. Just open and create.

---

## 🕹️ Live Demo

**[▶ Launch the app](https://yuanfengli168.github.io/pixel-image-generator/)**

---

## 📸 Screenshot

> *(drop a screenshot or demo GIF here — e.g. `docs/demo.gif`)*

```
[ screenshot placeholder ]
```

---

## ✨ Features

- **Three art styles** — Pixel Art, Anime, and Meme, each with tuned prompt modifiers
- **Live character counter** — 200 char max, warns at 170
- **15-second timeout guard** — clear error message if the API doesn't respond in time
- **One-click PNG download** — saves as `generated-image.png`
- **Regenerate button** — new random seed every time, no duplicates
- **Zero dependencies** — single `index.html`, no npm, no build step
- **Retro pixel UI** — Press Start 2P font, CRT scanlines, neon glow, animated loading bar

---

## ⚙️ How It Works

1. You type a prompt and choose a style
2. A style modifier is appended to your prompt:
   - **Pixel Art** → `pixel art style, 16-bit, retro game`
   - **Anime** → `anime style, manga, cel shading, vibrant colors`
   - **Meme** → `meme format, internet humor, flat style`
3. The combined prompt is URL-encoded and sent to the [Pollinations.ai](https://pollinations.ai) image API:

```
https://image.pollinations.ai/prompt/{encoded_prompt}?width=512&height=512&nologo=true&seed={random}
```

4. The image loads directly in the browser — **no backend, no API key, no cost**

Pollinations.ai is a free, open-access generative image API. No account or authentication required.

---

## 🖥️ Run Locally

No server needed. Just:

```bash
git clone https://github.com/yuanfengli168/pixel-image-generator.git
cd pixel-image-generator
open index.html   # or double-click it in your file manager
```

That's it. It runs entirely in the browser.

---

## 🛠️ Tech Stack

| Layer | Tech |
|---|---|
| UI | Vanilla HTML + CSS + JavaScript |
| Font | [Press Start 2P](https://fonts.google.com/specimen/Press+Start+2P) (Google Fonts) |
| Image API | [Pollinations.ai](https://pollinations.ai) (free, no auth) |
| Hosting | GitHub Pages |
| Dependencies | **None** |

---

## 🧩 How to Extend

### Add a new style

In `index.html`, find the `STYLE_MODIFIERS` object and add a new entry:

```js
const STYLE_MODIFIERS = {
  pixel: 'pixel art style, 16-bit, retro game',
  anime: 'anime style, manga, cel shading, vibrant colors',
  meme:  'meme format, internet humor, flat style',
  // add yours:
  oil:   'oil painting, impressionist, thick brush strokes'
};
```

Then add a matching button in the `.style-grid` section with the same `data-style` value.

### Swap the image API

Replace the URL built in the `generate()` function with any image generation endpoint that accepts a prompt via URL or POST. The rest of the UI — loading bar, timeout, download — will work unchanged.

### Change image size

Find the `imgUrl` line in `generate()` and update `width` / `height`:

```js
const imgUrl = `https://image.pollinations.ai/prompt/${encoded}?width=768&height=768&nologo=true&seed=${seed}`;
```

---

## 🤝 Contributing

Pull requests welcome. Keep it single-file, zero-dependency, and pixel-spirited.

1. Fork the repo
2. Make your changes in `index.html`
3. Open a PR with a short description of what you changed

---

## 📄 License

[MIT](LICENSE) — free to use, remix, and deploy.

---

*Built with 💾 and way too many neon colors.*
