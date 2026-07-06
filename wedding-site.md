# Khen & Tine Wedding Invitation — Project Notes

Single self-contained file: **`index.html`** (HTML + CSS + JS inline, no build step, no dependencies). Open by double-click or deploy as-is.

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
5. **Story** (`#story`) — 4-step timeline (2x2 grid), real copy: We First Crossed Paths (2019) → Together (2024) → Our Adventures (2024–2025) → The Proposal (2025); real photos from `assets/story1.jpg`–`story4.jpg`
6. **Details** (`#details`) — ceremony/reception cards + Google Maps QR codes/links
7. **Entourage** (`#entourage`) — dark-navy card: Groom/Bride names at the top, then Parents (with Father/Mother labels), Best Man/MOH, groomsmen/bridesmaids, flower girls, bearers (see data below)
8. **Sponsors** (`#sponsors`) — Principal Sponsors / Ninong & Ninang
9. **Attire** (`#attire`) — dress code + color swatches
10. **Gallery** (`#gallery`) — rotating coverflow-style carousel (dark navy background, matches Entourage/Sponsors theme) — center photo large, prev/next photos peek on either side with a 3D tilt, arrow buttons + click-side-photo-to-jump + touch swipe + autoplay every 4.5s
11. **Messages** (`#messages`) — guestbook form (Firebase-backed) + live message wall
12. **RSVP** (`#rsvp`) — form (Firebase-backed, private responses)
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
- Hero background now uses a themed Unsplash placeholder (elegant garden archway) instead of a random `picsum.photos` image — see `#hero` background in `index.html`; swap for a real couple photo whenever ready
- RSVP background photo is still a `picsum.photos` placeholder — swap with a real URL/path when available
- Gallery carousel has 8 real photos loaded from `assets/gallery1.jpg` through `assets/gallery8.jpg` (in `#car-track` in `index.html`). Add/remove `<div class="car-slide">` blocks to change the photo count — the JS reads however many exist automatically
- Our Story timeline: copy is final (real story) and photos are real, loaded from `assets/story1.jpg` through `assets/story4.jpg` (see `.tl-step .ph` CSS in `index.html`) — one image per step in order (We First Crossed Paths, Together, Our Adventures, The Proposal)
- Candle Sponsors and Cord Sponsors names — not yet provided (Bible Bearer role removed)
- FIREBASE_CONFIG is filled in with the real `khen-tine-wedding` project values; guestbook Firestore rules are published — RSVP rules still need to be added (see "Guestbook + RSVP backend" below) before RSVP submissions will succeed
- Background music file not added yet — see "Background Music" below

## Locations
- Ceremony QR/"Get Directions" links to the Google Maps place page for Diocesan Shrine and Parish of Nuestra Señora de los Desamparados
- Reception QR/"Get Directions" links to the Google Maps place page for Water Nymph Resort - Events Venue

## Guestbook + RSVP backend (Firebase Firestore)
Both the "Leave us a Message" form (public wall) and the RSVP form (private, couple-only) are wired to Firebase Firestore (`index.html`, near `FIREBASE_CONFIG` in the bottom `<script>`, using the `firebase-app-compat` / `firebase-firestore-compat` SDKs loaded via CDN just above it) — `messages` and `rsvps` collections respectively. Setup:

1. Go to https://console.firebase.google.com/ → **Add project** → give it any name (e.g. `khen-tine-wedding`) → finish the wizard (Google Analytics is optional, can skip).
2. In the project, click the **`</>` (Web) icon** to register a web app → nickname it anything → **do not** check "set up Firebase Hosting" → Register app.
3. Firebase shows a `firebaseConfig` object — copy those values into `FIREBASE_CONFIG` in `index.html` (replace all the `YOUR_...` placeholders).
4. In the left sidebar go to **Build → Firestore Database** → **Create database** → start in **production mode** → pick a region close to you (e.g. `asia-southeast1`) → Enable.
5. Go to the **Rules** tab of Firestore and replace the rules with:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /messages/{id} {
         allow read: if true;
         allow create: if request.resource.data.name is string
                       && request.resource.data.name.size() < 80
                       && request.resource.data.message is string
                       && request.resource.data.message.size() < 500;
         allow update, delete: if false;
       }
       match /rsvps/{id} {
         allow read: if false;
         allow create: if request.resource.data.name is string
                       && request.resource.data.name.size() > 0
                       && request.resource.data.name.size() < 80
                       && request.resource.data.attending is string
                       && request.resource.data.attending.size() < 40
                       && request.resource.data.note is string
                       && request.resource.data.note.size() < 500;
         allow update, delete: if false;
       }
     }
   }
   ```
   `messages` stays publicly readable (it's the guestbook wall guests see); `rsvps` blocks all reads from the site so responses are only visible to you in the Firebase console under **Firestore Database → Data → rsvps**. Both block edits/deletes and cap field sizes to reduce abuse. Click **Publish**.
6. Commit the updated `FIREBASE_CONFIG` values and push — GitHub Pages will redeploy, and both forms will start saving real data to Firestore.
- Optional hardening later: add Firebase App Check, or a Cloud Function to moderate/rate-limit submissions.

## Background Music
- An `<audio id="bgm" loop>` element and a floating `#music-toggle` button (bottom-right, speaker icon) are wired up near the top of `<body>` and in the bottom `<script>` in `index.html`.
- On page load it tries to autoplay; if the browser blocks autoplay-with-sound (common on first visit), it silently starts on the visitor's first click/scroll/tap instead. The floating button lets guests mute/unmute anytime, and pulses gently while playing.
- **To activate:** get a royalty-free track (e.g. from [pixabay.com/music](https://pixabay.com/music/) — free, no attribution required — or bensound.com), save it as `assets/song.mp3` in this project folder, and it'll just work. The `<source>` path is set in `index.html` near the `#loader` div (`<audio id="bgm">`) — update it there if you use a different filename/path.

## Deployment
- Deployed via **GitHub Pages**: https://thekhenzie.github.io/khen-justine-wedding/
- Re-deploy by pushing changes to the `main` branch — GitHub Pages publishes `index.html` automatically
- No custom domain set up yet

## How to resume work with minimal context
Just point Claude at `index.html` and this file. Everything editable is clearly commented in the HTML (easy-edit blocks, `WEDDING_DATE` constant, CSS variable palette at the top).
