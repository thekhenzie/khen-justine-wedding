# Khen & Tine Wedding Invitation

A single-page wedding invitation website for **Khen & Tine's** wedding on **February 26, 2027**.

🔗 `#DesTINEdForKHEN`

Live site: https://thekhenzie.github.io/khen-justine-wedding/
## Overview

This is a self-contained, static wedding invitation site — no build tools, no dependencies, no backend. Everything (HTML, CSS, and JavaScript) lives in one file: [`index.html`](./index.html).

## Features

- Animated loader with monogram intro
- Sticky nav with mobile hamburger menu
- Live countdown to the wedding day
- Our Story timeline
- Ceremony & reception details with venue info and QR codes
- Entourage & Principal Sponsors sections
- Attire / dress code guide with color swatches
- Photo gallery
- Guestbook / messages wall
- RSVP form
- Gift guide
- FAQ accordion
- Scroll-reveal animations throughout

## Getting Started

No installation required. Simply open `index.html` in a browser:

```bash
# Windows
start index.html

# macOS
open index.html

# Linux
xdg-open index.html
```

## Deployment

The site is deployed via **GitHub Pages**: https://thekhenzie.github.io/khen-justine-wedding/

1. Push changes to the `main` branch (or whichever branch GitHub Pages is configured to serve from)
2. GitHub Pages will automatically publish the updated `index.html`

> If GitHub Pages isn't enabled yet: repo **Settings → Pages → Build and deployment → Source: Deploy from a branch**, then select `main` / `(root)`.

## Editing

All editable content — names, dates, venues, entourage, photos — is clearly marked with comment blocks inside `index.html`. Key spots:

| What | Where |
|---|---|
| Wedding date/time | `WEDDING_DATE` constant in the `<script>` near the bottom |
| Color palette | CSS custom properties in `:root` near the top of `<style>` |
| Fonts | Google Fonts `<link>` tags in `<head>` |
| Entourage / sponsors | HTML content within the `#entourage` and `#sponsors` sections |

See [`wedding-site.md`](./wedding-site.md) for full project notes, including entourage names already filled in and open/placeholder items still to finish (real photos, QR codes, TBA sponsor names, exact venue addresses).

## Notes

- The Messages (guestbook) and RSVP forms both save to **Firebase Firestore** — see `wedding-site.md` for setup steps, config, and the security rules needed for each collection.
- Photos are placeholder images and need to be swapped with real ones before launch.

All rights reserved