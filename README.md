# Custom Game Maker

A private, static web app for designing personalized print-and-play card games. No build step, no backend, no accounts — everything runs in the browser and prints on standard letter paper.

## Makers

- **Deal Deck Maker** (`index.html`) — a property-trading card game. Rename all the places, personalize the card back and the money-card wording, then print a complete, balanced 111-card deck (fronts and backs) plus a matching box.
- **Find-It Deck Maker** (`spot-it-deck-maker.html`) — a matching game where every card shares exactly one symbol with every other card, guaranteed by projective-plane math. Build the deck from emojis, your own photos, or a mix.
- **Deck Box Cutout** (`box-cutout.html`) — a fold-and-glue box template sized for the deck, with customizable colors and cover text.

## Features

- Password gate (SHA-256 hash check, remembered per device via `localStorage`)
- Deck edits auto-save to the browser, with a one-click reset to the sample deck
- Print-ready layouts: 3×3 card sheets on letter paper, exact-color printing enabled
- Fully static — a single HTML file per maker, with fonts and icons from CDNs

## Printing

1. Use cardstock and set printer margins to **None**.
2. Enable **Background graphics** in the print dialog.
3. Print the card fronts, flip the stack, then print the card backs.
4. Print the box template on letter paper in **landscape**; cut the solid lines, fold the dashed ones.

## Deployment (Render)

This repo includes `render.yaml` for a Render **Static Site**:

- Build command: `echo "No build required"`
- Publish directory: `.`
- Entry file: `index.html`

Connect the GitHub repo in Render and it will publish from the root and redeploy on every commit. Any other static host (Netlify, Vercel, GitHub Pages) works the same way.

## Changing the password

The gate compares a SHA-256 hash of the entered password against `UNLOCK_HASH` in `index.html`. To set a new password, generate its hash in a browser console:

```js
const b = new TextEncoder().encode('your-new-password');
crypto.subtle.digest('SHA-256', b).then(d =>
  console.log([...new Uint8Array(d)].map(x => x.toString(16).padStart(2, '0')).join('')));
```

Paste the result into `UNLOCK_HASH`. Note this is a light privacy screen for a static site, not real security — anyone with the deployed files can read the client-side code.
