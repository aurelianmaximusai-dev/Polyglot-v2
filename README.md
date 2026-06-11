# Polyglot PWA — deployment

A language-learning web app (Chinese, Japanese, Korean, Russian, Spanish, A0 → ≈N2)
that installs to the iPhone home screen and works fully offline.

## Files
- `index.html` — the entire app
- `manifest.webmanifest` — app name, icons, standalone display
- `sw.js` — service worker (offline cache)
- `icons/` — home-screen icons

## Deploy in 3 minutes (GitHub Pages — free, HTTPS, no Apple involvement)
1. Create a new GitHub repo (e.g. `polyglot`), upload these files to the repo root.
2. Repo → Settings → Pages → Source: `Deploy from a branch` → branch `main`, folder `/ (root)` → Save.
3. After ~1 minute your app is live at `https://<username>.github.io/polyglot/`

Any other static host works too (Cloudflare Pages, Netlify, your own nginx with HTTPS).
The only hard requirement is **HTTPS** — iOS refuses service workers on plain http.

## Install on iPhone
1. Open the URL in **Safari** (must be Safari — other iOS browsers can't install PWAs).
2. Tap **Share** (square with arrow) → **Add to Home Screen** → Add.
3. Launch from the icon: fullscreen, no browser chrome, works in airplane mode.

## Notes
- Progress is stored on-device (localStorage). Use Progress → Export/Import to move
  between devices. iOS may clear storage of *web pages* unused for weeks, but
  home-screen-installed apps are exempt as long as the icon stays installed.
- AI Tutor: works out of the box pointed at the Claude API (needs a key when
  self-hosted — set it in Tutor → Engine settings), or point it at any
  OpenAI-compatible endpoint, e.g. vLLM:
  `http://<host>:8000/v1/chat/completions` (start vLLM with CORS open, and note
  an installed PWA served over HTTPS can only call http:// LAN endpoints if you
  put the vLLM behind HTTPS too, e.g. caddy/nginx with a self-signed or
  Tailscale cert).
- Updating the app: replace files in the repo, then bump `CACHE = "polyglot-v1"`
  in `sw.js` (v2, v3…) so installed clients fetch the new version.
