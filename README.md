# Khen & Tine Wedding Invitation

A single-page wedding invitation website for **Khen & Tine's** wedding on **February 26, 2027**.

🔗 `#KhenAndTine2027`

Live site: https://thekhenzie.github.io/khen-justine-wedding/
## Overview

This is a self-contained, static wedding invitation site — no build tools, no dependencies, no backend. Everything (HTML, CSS, and JavaScript) lives in one file: [`wedding-site.html`](./wedding-site.html).

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

No installation required. Simply open `wedding-site.html` in a browser:

```bash
# Windows
start wedding-site.html

# macOS
open wedding-site.html

# Linux
xdg-open wedding-site.html
```

## Deployment

The site is deployed via **GitHub Pages** at `/khen-justine-wedding`:

1. Rename `wedding-site.html` to `index.html` (for a clean root URL)
2. Push to the `main` branch (or whichever branch GitHub Pages is configured to serve from)
3. GitHub Pages will automatically publish the updated file

## Editing

All editable content — names, dates, venues, entourage, photos — is clearly marked with comment blocks inside `wedding-site.html`. Key spots:

| What | Where |
|---|---|
| Wedding date/time | `WEDDING_DATE` constant in the `<script>` near the bottom |
| Color palette | CSS custom properties in `:root` near the top of `<style>` |
| Fonts | Google Fonts `<link>` tags in `<head>` |
| Entourage / sponsors | HTML content within the `#entourage` and `#sponsors` sections |

See [`wedding-site.md`](./wedding-site.md) for full project notes, including entourage names already filled in and open/placeholder items still to finish (real photos, QR codes, TBA sponsor names, exact venue addresses).

## Notes

- The RSVP and Messages (guestbook) forms are currently **visual-only** — they display a confirmation but don't send or store data anywhere.
- Photos are placeholder images and need to be swapped with real ones before launch.
