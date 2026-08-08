# resource-board-marketing

Marketing site for **resourceboard.io** — the public-facing landing page for the Resource Board product (a labor scheduling tool for self-perform contractors).

## Stack

- Static HTML / CSS / JS
- No build system, no framework, no package manager
- Fonts loaded from Google Fonts CDN

## Deploy

Netlify, auto-deploy from `main`. `netlify.toml` sets the publish directory to the repo root and pins basic security headers (X-Frame-Options, X-Content-Type-Options, Referrer-Policy).

## Local development

Open `index.html` directly in a browser, or serve the folder with any static file server (e.g. `python3 -m http.server`).
