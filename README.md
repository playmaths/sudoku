# Family Sudoku

A single-file, ad-free classic Sudoku for a shared family iPad and everyone's phones. Play it at **https://playmaths.github.io/sudoku/**, or open `index.html` in any browser. On a phone or tablet use *Add to Home Screen*: the web manifest and service worker make it an installed app with its own icon that works offline.

## What it does

- **Four difficulties** generated on the device, each with a unique solution and graded by the techniques it actually needs rather than by clue count: Easy and Medium are solvable with singles alone, Hard needs pairs, triples, pointing pairs or box-line reductions, and Master needs an X-Wing, Swordfish or Y-Wing but nothing the coach cannot explain. A **Daily Family Board** gives every player the same puzzle on a given day.
- **Pro notation**: pen, corner pencil notes and Snyder-style center notes, with auto-notes and a digit "sniper" highlight.
- **What-If Trail**: bookmark the board, try a hypothesis in teal ink, then rewind or commit.
- **Technique coach**: each hint is the next real logical step on the board, named for its technique (naked and hidden singles, naked and hidden pairs and triples, pointing pairs, box-line reduction, X-Wing, Swordfish, Y-Wing). Placement hints fill one cell, elimination hints remove only the pencil marks the logic rules out, and in the rare position that needs a technique the coach does not know it says so and reveals one cell.
- **Newspaper entry**: type in a puzzle from a paper or book; the app checks it is solvable and plays it like any other board. A grid with several valid answers is accepted too and judged on clashes only.
- **Family profiles** with per-player history, daily streaks and a stats page, saved in the device's browser. A sync code copies everything to another device.
- **Comfort settings**: large digits, optional auto error check (turn it off for a "Purist" solve, where nothing on screen gives a wrong digit away) and smart highlighting.
- **Honest timer**: counts wall-clock time while the board is on screen, pauses when the app is in the background, and survives a reload.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole game: markup, styles and script |
| `manifest.webmanifest`, `icons/` | Home-screen install name and icon |
| `sw.js` | Service worker that caches the page for offline play |

Tailwind CSS (compiled for this page) and Chart.js are inlined in `index.html`, so nothing is fetched from a CDN. The Inter font loads from Google Fonts when online and falls back to the system font otherwise.

## License

MIT, see [LICENSE](LICENSE).
