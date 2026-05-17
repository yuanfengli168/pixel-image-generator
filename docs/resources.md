# Resources & API Choices

> Why Pollinations.ai, and what else is out there.

---

## Why Pollinations.ai

### vs. Replicate

| | Pollinations.ai | Replicate |
|---|---|---|
| **Cost** | Free, no limits | Pay-per-run (~$0.0023–0.014 per image) |
| **API key** | Not required | Required |
| **Frontend-safe** | ✅ Call directly from browser JS | ❌ Exposes key if called from frontend |
| **Backend needed** | No | Yes (to proxy the key) |
| **Setup time** | Zero | Account + billing + key management |
| **Rate limits** | Generous, community-shared | Tied to your account credits |
| **Model choice** | Curated defaults | Hundreds of models |

For a zero-dependency static app, Replicate is a non-starter. Any API key embedded in frontend JS is public. You'd need a backend proxy, which breaks the "just open index.html" promise of this project.

Pollinations.ai requires nothing. The URL is the API. That's the right fit.

### vs. Stability AI (DreamStudio)

Same problem as Replicate — requires an API key, credits, and a backend. Also has a more complex request format (JSON POST, not a URL). Overkill.

### vs. Hugging Face Inference API

Free tier exists, but:
- Still requires an account and token
- CORS restrictions can block direct browser calls
- Rate limits are tighter on free tier
- Response format requires parsing JSON, not just loading a URL

### vs. OpenAI DALL·E

- Paid (no free tier for image generation)
- Requires API key
- Backend-only safe usage
- Higher latency at comparable quality for pixel/anime styles

### Why Pollinations.ai Works Here

The API is a plain URL:

```
https://image.pollinations.ai/prompt/{encoded_prompt}?width=512&height=512&nologo=true
```

You can paste it in a browser tab and get an image. That means:

- No `fetch()` complexity — just set `img.src`
- No CORS issues (the API serves images with open headers)
- No auth headers to manage
- Error handling is just `img.onerror`
- The URL itself is shareable

It's the simplest possible interface for an image generation API.

---

## Pollinations.ai API Reference

**Base URL:** `https://image.pollinations.ai/prompt/{prompt}`

| Parameter | Type | Default | Description |
|---|---|---|---|
| `width` | int | 1024 | Output image width in pixels |
| `height` | int | 1024 | Output image height in pixels |
| `seed` | int | random | Fixed seed for reproducible results |
| `model` | string | `flux` | Model to use (flux, turbo, etc.) |
| `nologo` | bool | false | Remove Pollinations watermark |
| `enhance` | bool | false | Auto-enhance the prompt |
| `negative` | string | — | Negative prompt (things to avoid) |

**Example:**
```
https://image.pollinations.ai/prompt/a%20cat%20in%20pixel%20art%20style%2C%2016-bit?width=512&height=512&nologo=true&seed=42
```

Full docs: [https://pollinations.ai/docs](https://pollinations.ai)

---

## Other Resources Used

### Press Start 2P (Font)

- **Source:** [Google Fonts](https://fonts.google.com/specimen/Press+Start+2P)
- **Why:** The canonical retro pixel-game typeface on the web. Immediately communicates "8-bit" without any other visual support. Free, no license issues.
- **Loaded via:** `@import url(...)` in CSS — no JS, no render blocking beyond the font itself.

### CSS-Only Visual Effects

All visual effects (scanlines, glow, grid, flicker) are pure CSS — no canvas, no WebGL, no images.

| Effect | Technique |
|---|---|
| Scanlines | `repeating-linear-gradient` on `body::after` |
| Neon glow | `box-shadow` + `text-shadow` with color match |
| Grid background | Two overlapping `linear-gradient` backgrounds |
| CRT flicker | `@keyframes` opacity animation |
| Loading bar | CSS `animation` on width, `linear` timing |
| Pixel corners | `::after` pseudo-element with fixed width/height |

---

## Alternatives Considered for Hosting

| Option | Verdict |
|---|---|
| **GitHub Pages** | ✅ Chosen — free, zero config, auto-deploys from `main` branch |
| Netlify | Works great, but overkill for a single static file with no build step |
| Vercel | Same — better suited for Next.js / serverless functions |
| Cloudflare Pages | Excellent CDN, but GitHub Pages is simpler for a repo-native project |
| Raw GitHub URL | `raw.githubusercontent.com` doesn't serve HTML with correct MIME types |

GitHub Pages was the obvious choice: it's built into the repo, requires one API call to enable, and deploys automatically on every push to `main`.
