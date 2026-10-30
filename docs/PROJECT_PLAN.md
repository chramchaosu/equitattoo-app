# equiTattoo project plan

This is the public engineering summary. The full plan, with options and costs, is kept privately by the owner.

## What the app does

One private workspace per tattoo artist, plus one public gallery that never shows personal data.
The app is mobile-first and available in English, Czech and Slovak.

- **Materials:** stock of needle cartridges (by thickness, needle count, type and manufacturer, with optional lot and
  expiry) and pigments, with low-stock levels.
- **What to buy:** items below their low-stock level appear automatically, alongside manual items.
- **Tattoos:** listed by date or client name, with notes, optional age group and gender, five photo groups (reference,
  stencil, in progress, finished, healed), cartridges used and pigments used.
- **Pigments:** every bottle has a batch number, first-use date, expiry date and expiry alerts. Black is added to a
  tattoo automatically from its date. Colours are picked from the bottles in use that day.
- **Gallery:** tattoos published with the client's consent, as separate copies with no client details. Style tags allow
  filtering. Studio mode shows the gallery full screen behind a PIN.
- **Import:** one-way import from the JSON backup of the original equiTattoo app.

## Data model

Firestore can't hide single fields, so public data lives in separate documents.

| Collection                                | Readers         | Writers                  |
| ----------------------------------------- | --------------- | ------------------------ |
| `artists/{id}`                            | That artist     | That artist              |
| `artists/{id}/cartridges/{id}`            | That artist     | That artist              |
| `artists/{id}/inks/{id}`                  | That artist     | That artist              |
| `artists/{id}/inkBottles/{id}`            | That artist     | That artist              |
| `artists/{id}/shoppingItems/{id}`         | That artist     | That artist              |
| `artists/{id}/tattoos/{id}`               | That artist     | That artist              |
| `artists/{id}/tattoos/{id}/photos/{id}`   | That artist     | That artist              |
| `galleryItems/{id}`                       | App server only | That artist, safe fields |

Cloud Storage holds `private/{artistId}/...` (that artist only) and `public/{artistId}/...` (anyone can read).
Everything else is denied by default. Only accounts with an approved custom claim have any access.

Cartridges and pigments on a tattoo are stored as copies, so later stock edits can't change a past record.
Photos are resized on the device, which also removes hidden data such as GPS location.

## Architecture

- Angular 22 with the official Firebase SDK in our own services. AngularFire is not used.
- Hybrid rendering: the public gallery is rendered on the server and loads no Firebase client code. The private app
  renders in the browser. A small server sets the security headers.
- PWA with offline use, built-in Angular translations, and label reading with Tesseract.js on the device.
- Firestore Standard edition, App Hosting and Cloud Storage all in `europe-west4`.

## Environments and deployment

| Branch       | Firebase project     | Purpose                     |
| ------------ | -------------------- | --------------------------- |
| `main`       | `equitattoo-staging` | Test copy of the app        |
| `production` | `equitattoo-prod`    | Live app                    |

Both branches accept changes only through pull requests with passing checks. App Hosting rolls out when a branch
changes. Firestore and Storage rules deploy through GitHub Actions with Workload Identity Federation, and must stay
compatible with both the old and new app version during a rollout.

## Build order

0. Accounts and tools
1. Foundation: app skeleton, CI, translations, security headers, sign-in
2. Materials and What to buy
3. Tattoos, photos, cartridges used, label reading
4. Pigments and expiry alerts
5. Gallery, consent and studio mode
6. Import, backups, PWA and offline use
7. Launch: accessibility, security and performance review
