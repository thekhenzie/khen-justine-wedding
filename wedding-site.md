# Khen & Tine Wedding Invitation — Project Notes

Single self-contained file: **`wedding.html`** (HTML + CSS + JS inline, no build step, no dependencies). Open by double-click or deploy as-is.

## Core details
- Couple: **Khen & Tine**
- Date: **February 26, 2027** (Friday) — set in JS as `WEDDING_DATE = new Date(2027, 1, 26, 14, 0, 0)` near the bottom of the file (month is 0-indexed: `1` = Feb)
- Ceremony: Diocesan Shrine and Parish of Nuestra Señora de los Desamparados — 2:00 PM
- Reception: Water Nymph Resort – Event Resort — 5:00 PM
- Hashtag: `#KhenAndTine2027`
- RSVP-by date shown in copy: January 26, 2027

## Theme
- Style inspired by **InviteKo** templates (emerald-gold → then royal-blue-gold), currently: **dusty blue & gold**
- Palette lives in `:root` CSS variables near the top: `--blue-deep`, `--blue`, `--blue-soft`, `--gold`, `--gold-light`, `--gold-dark`, `--cream`, `--ivory`, `--navy-deep` (deep navy used specifically for Entourage/Sponsors cards)
- Fonts: Playfair Display (serif headings), Cormorant Garamond (serif-soft), Great Vibes (script, used for names), Jost (sans body)
- Entourage/Sponsors sections use a deep-navy card layout with dotted separators (`.dot-sep`) and per-name staggered fade-in (`.stagger-list`), modeled on an InviteKo screenshot the user shared

## Section order (top → bottom)
1. Loader (monogram "K&T" + script names, fades out ~1.7s after load)
2. Nav (fixed, sticky-solid on scroll, mobile hamburger)
3. Hero (names, tagline, ceremony location)
4. Countdown (`#countdown-sec`, live ticking JS)
5. **Story** (`#story`) — 3-step timeline, placeholder text/photos
6. **Details** (`#details`) — ceremony/reception cards + placeholder QR codes
7. **Entourage** (`#entourage`) — parents, best man/MOH, groomsmen/bridesmaids, flower girls, bearers (see data below)
8. **Sponsors** (`#sponsors`) — Principal Sponsors / Ninong & Ninang
9. **Attire** (`#attire`) — dress code + color swatches
10. **Gallery** (`#gallery`) — photo grid, placeholder images
11. **Messages** (`#messages`) — guestbook form (visual-only) + sample wall
12. **RSVP** (`#rsvp`) — form (visual-only, shows thank-you on submit)
13. **Gifts** (`#gifts`) — gift guide note
14. **FAQ** (`#faq`) — accordion
15. Footer

## Entourage / sponsors data already filled in
- **Groom's parents:** Arnold Baniqued, Olme Baniqued
- **Bride's parents:** Rolan Gonzales, Analiza Dilag
- **Best Man:** Arvin Kyle Baniqued · **Maid of Honor:** Melanie Alarcon
- **Groomsmen:** Mark Kevin Baniqued, Adrian Karl Baniqued, Ronald John Baniqued, Ryan Ernesto Cañares, Angelo Planta, Ivan Ayala
- **Bridesmaids:** Nerissa Alarcon, Chriza Bauyon, Jessa Banog, Kim Chelze Marra, Kris Madel Alejandria, Mariel Dela Cruz
- **Flower Girls:** Queen Xyrelle Gacias, Ellara Jae Manuel
- **Coin Bearer:** Zane Joey Ayala · **Ring Bearer:** Hayes Caleb Bajar · **Bible Bearer:** *TBA*
- **Veil Sponsors:** Israel Agudo & Christine Joy Agudo · **Cord Sponsors:** *TBA* · **Candle Sponsors:** *TBA*
- **Principal Sponsors (Ninong / Ninang):** Joseph & Somelyn Romano, Christian & Javerlyn Delos Reyes, Roel Arangilan & Marianne Degracia Mahinay, Natz & Bernadeth Degracia Cañares, Rico & Maria Easter Gonzales, Roger Baniqued & Rowena Ubalde

## Still placeholder / open items
- All photos are `picsum.photos` seeded placeholders — swap with real URLs/paths when available
- QR codes in Details section point to a dummy `qrserver.com` data string — needs real venue map links
- Our Story timeline text/years and photos are generic placeholders
- Candle Sponsors, Cord Sponsors, and Bible Bearer names — not yet provided
- RSVP and Messages (guestbook) forms are **visual-only** — they show a thank-you message but don't send/store data anywhere (no backend wired up)
- Ceremony/reception exact street addresses not added (only venue names)

## Deployment
- Currently deployed manually via **Netlify Drop** (drag-and-drop `wedding.html`, ideally renamed to `index.html` for a clean URL)
- Re-deploy after any edit by dragging the updated file into the Netlify dashboard's Deploys tab
- No custom domain set up yet

## How to resume work with minimal context
Just point Claude at `C:\Users\harol\KhenAndTine-Wedding\wedding.html` and this `PLAN.md`. Everything editable is clearly commented in the HTML (easy-edit blocks, `WEDDING_DATE` constant, CSS variable palette at the top).
