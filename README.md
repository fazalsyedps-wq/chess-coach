# Chess Move Coach

Enter your opponent's move and the engine suggests your best reply, with an explanation.

- Written from scratch in JavaScript, with no chess libraries.
- Move generator checked against standard perft counts.
- Engine: alpha-beta minimax, iterative deepening, quiescence search, killer moves.
- Evaluation: material plus piece-square tables.
- Works offline as a Home Screen app (manifest + service worker).

Files: `index.html` (the whole app), `manifest.webmanifest` (app name and icon),
`sw.js` (offline support), `icon-*.png` (icons).
