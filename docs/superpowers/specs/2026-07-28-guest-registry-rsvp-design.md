# Guest-Registry RSVP — Design

## Purpose

Replace the current free-form RSVP (name + attending dropdown + guest-count dropdown) with a guest-registry lookup flow: guest types their name exactly as printed on the invitation, the site looks it up, and — if found — shows one row per invited seat with an editable name and an individual accept/decline toggle. This enforces "strictly no plus one" and lets each named guest confirm/decline individually instead of one blanket answer for the whole party.

Visual reference for layout/interaction only (screenshots provided by user): hero seat-count number, "CONFIRM ATTENDANCE" list with roman-numeral rows, per-row toggle labeled "JOYFULLY ACCEPTS" / "REGRETFULLY DECLINES", "CONFIRM RESPONSE" button, RSVP deadline banner, "no plus one" notice. Colors/fonts stay the site's existing dusty-blue-and-gold theme (`:root` variables already in `index.html`) — the pink in the screenshots is not used.

## Scope

- `index.html`: replace the `#rsvp` section's form/JS with the new lookup + per-seat toggle flow.
- New `admin.html`: private page (Firebase Auth-gated) for adding/editing/deleting invitations.
- Firestore: new `guests` collection + rules; old `rsvps` collection and its read/write code path are removed (dead code), not migrated.
- `wedding-site.md`: updated with the new setup/maintenance instructions (Firestore rules, enabling Firebase Auth, creating the admin login, how to add invitations).

## Data model

Collection: `guests`. One document per invitation.

- **Doc ID**: normalized invitee name — `name.trim().toLowerCase().replace(/\s+/g, ' ')`. Lookup is a direct `.doc(id).get()`, never a collection query, so a guest can fetch their own invitation without any query that could enumerate other guests.
- **Fields**:
  - `displayName` (string) — original-cased name, e.g. `"Johnnie Walker"`
  - `seatCount` (number) — total invited seats, e.g. `4`
  - `seatNames` (array<string>, length === `seatCount`) — pre-filled name per seat, entered by the couple via admin.html
  - `responses` (array<{name: string, attending: boolean}>, same length as `seatNames`) — present once the invitation has been responded to; absent/undefined before first submit
  - `respondedAt` (Firestore server timestamp) — present once responded to

Re-submission is allowed: looking up an invitation that already has `responses` pre-fills the form with those saved names/toggle states (instead of `seatNames`/all-accept default), and confirming again overwrites `responses`/`respondedAt`.

## Firestore rules

```
match /guests/{id} {
  allow get: if true;
  allow list: if request.auth != null;
  allow create, delete: if request.auth != null;
  allow update: if request.auth != null
    || (
      request.resource.data.diff(resource.data).affectedKeys()
        .hasOnly(['responses', 'respondedAt'])
      && request.resource.data.responses.size() == resource.data.seatNames.size()
      && request.resource.data.responses.size() < 20
    );
}
```

(Field-shape checks on each `responses[i]` — string `name` under ~80 chars, boolean `attending` — are enforced client-side before write; Firestore rules validate the size/key constraints above, which is the practical limit for array-of-map validation without per-element rules.)

The `rsvps` collection and its rules are deleted — nothing writes to it anymore.

## Guest-facing flow (`#rsvp` in index.html)

Three states inside the existing `#rsvp` section (same eyebrow/title/intro copy as today):

**1. Lookup (default state)**
- Field "GUEST NAME", placeholder "Enter your name as written in the invitation"
- Status line under the field, starts as "Ready — enter your name above"
- "Find My Invitation" button (submit on button click or Enter)
- On submit: normalize input, `db.collection('guests').doc(id).get()`
  - **Not found**: status line switches to an error style: "We couldn't find that name on our guest list. Please check the spelling, or enter it exactly as printed on your invitation."
  - **Found**: transition to state 2, pre-filling from `responses` if present, else from `seatNames` with every toggle defaulted to accept

**2. Confirm attendance**
- Hero: big serif number = `seatCount`, subtitle "guest(s) included in the invitation"
- "CONFIRM ATTENDANCE" / "Toggle each name below to mark who will be attending."
- One row per seat: lower-case roman numeral (i., ii., iii., …), editable text input (pre-filled per above), toggle switch + dynamic label ("JOYFULLY ACCEPTS" when on, "REGRETFULLY DECLINES" when off) — reuses `--gold`/muted-gray for on/off states
- "‹ Search another name" link back to state 1
- "CONFIRM RESPONSE" button
- RSVP deadline banner (existing "January 26, 2027" copy, restyled per mockup as its own block)
- Static notice block: "Only guests with confirmed RSVPs will be accommodated at the celebration... STRICTLY NO PLUS ONE." (marked with an editable-content HTML comment, same convention as other site copy)
- On confirm: build `responses` from current inputs/toggles, `update()` the doc with `responses` + `respondedAt`, then show state 3

**3. Thank you**
- Reuses existing `.form-thanks` treatment ("Thank you!" + confirmation copy)

## Admin page (`admin.html`)

New sibling file, not linked from the site nav (direct-URL only), same Firebase SDK versions/config duplicated inline (consistent with the project's single-file, no-build-step approach).

- **Login**: email/password form → `firebase.auth().signInWithEmailAndPassword()`. Firebase's default session persistence keeps you logged in across visits until you sign out.
- **Invitation list**: table of all `guests` docs (allowed since `list` requires `auth != null`) — displayName, seatCount, responded yes/no, response summary.
- **Add/edit form**: displayName + seatCount (number input) → generates `seatCount` seat-name text inputs → Save writes `guests/{normalize(displayName)}` with `displayName`, `seatCount`, `seatNames`. Editing loads an existing doc's values back into this same form.
- **Delete** button per row.
- Minimal/utilitarian styling only — this page is a private tool, not guest-facing, so it doesn't need the wedding theme.

## Setup steps (documented in wedding-site.md, done once by the user)

1. Firebase console → Authentication → Sign-in method → enable Email/Password.
2. Firebase console → Authentication → Users → Add user → create the one admin login (your own email + a password).
3. Publish the updated Firestore rules (above) replacing the current `messages`/`rsvps` rules block (keep `messages` as-is, replace the `rsvps` block with `guests`).
4. Open `admin.html`, log in, add each invitation (name, seat count, seat names) before sharing invitations with guests.

## Out of scope / explicitly not doing

- No bulk CSV import — invitations are entered one at a time through admin.html.
- No email/SMS notifications on RSVP submit.
- No guest self-service "add a seat" — seat count is fixed by the registry entry, enforcing strictly-no-plus-one.
- No migration of old `rsvps` collection data — it's simply no longer written to.
