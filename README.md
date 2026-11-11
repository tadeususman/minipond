# Mini Pond

A virtual koi pond for stress relief. One self-contained HTML file: open it in a browser, watch the koi, feed them, dive in underwater.

## Features

- **Top-down pond** (Canvas 2D): koi with natural, randomly varying swim speeds, smooth wake ribbons, lily pads, light on the water, ripples
- **Feeding**: tap to drop food, koi rise to eat it; sweep the water to scatter them
- **Lily pads**: drag to move them
- **Water inlets**: three streams on the left bank that create gentle currents
- **Pond life**: frog, shrimp and crabs (with varied colours)
- **Weather & mood**: Rain (eases in slowly, fades out slowly, with rain sound), Night mode, drifting sakura petals and leaves
- **Dive in**: 3D underwater view (Three.js) with fish, shrimp, crabs and frog. Drag to look around, tap a fish
- **Sound**: water ambience, splashes (random loudness, two samples), rain (Web Audio, mp3s embedded as base64)
- **My fish**: manage your koi
- **Adaptive quality**: automatically lowers rendering load on slow devices such as tablets
- **Persistence**: state is stored in `localStorage` (`kolam:fish`, `kolam:pads`, `kolam:settings`)

## Controls

| Action | Result |
|---|---|
| Tap water | Drop food |
| Sweep over water | Scatter koi |
| Drag a lily pad | Move it |
| Dive in | Switch to the underwater view |
| Rain / Night / Sound | Toggle weather, night, audio |

## Files

- `index.html` – the whole app (about 2.3 MB, audio embedded)
- `manifest.webmanifest`, `sw.js`, `icon-*.png`, `apple-touch-icon.png` – make it installable on a phone (PWA)
- `README.md` – this file

## Install on a phone (PWA)

Host the folder over **HTTPS** (GitHub Pages works), then open the URL on the phone:

- **Android (Chrome)**: menu ⋮ → *Install app* / *Add to Home screen*
- **iPhone (Safari)**: Share → *Add to Home Screen*

It then opens full screen like an app and keeps working offline after the first visit (a service worker caches the page, Three.js and the fonts). After an update is pushed, open the app twice to get the new version.

## Run locally

Just open `index.html` in a browser, or:

```bash
python3 -m http.server 8080
# then visit http://localhost:8080
```

## Deploy to your server

On the server:

```bash
git clone https://github.com/tadeususman/minipond.git /var/www/minipond
# later, to update:
cd /var/www/minipond && git pull
```

It is a static file, so any web server works. Nginx example:

```nginx
server {
    listen 80;
    server_name pond.example.com;
    root /var/www/minipond;
    index index.html;

    # index.html is ~2.3 MB because of embedded audio; compression helps a lot
    gzip on;
    gzip_types text/html;
}
```

Apache: point `DocumentRoot` to the folder and enable `mod_deflate`.

Notes:

- **HTTPS is recommended** (Web Audio and some mobile browsers behave better on secure origins). Certbot / Let's Encrypt works fine.
- **Internet is required for visitors**: the page loads Three.js r128 from `cdnjs.cloudflare.com` (only when entering the dive view) and fonts from Google Fonts. Without them, the dive view won't open and a fallback font is used.
- `localStorage` is per domain, so saved fish and settings do not carry over between domains.

## Tech

Vanilla JavaScript in a single IIFE, Canvas 2D, Three.js r128 (lazy-loaded CDN), Web Audio API. No build step.
