# Family Sudoku

A single-file, ad-free classic Sudoku for a shared family iPad and everyone's phones. Play it at **https://playmaths.github.io/sudoku/**, or open `index.html` in any browser. On a phone or tablet use *Add to Home Screen*: the web manifest and service worker make it an installed app with its own icon that works offline.

## What it does

- **Four difficulties** (Easy, Medium, Hard, Master) generated on the device, each with a unique solution, plus a **Daily Family Board** that is the same puzzle for every player on a given day.
- **Pro notation**: pen, corner pencil notes and Snyder-style center notes, with auto-notes and a digit "sniper" highlight.
- **What-If Trail**: bookmark the board, try a hypothesis in teal ink, then rewind or commit.
- **Technique coach**: hints named after the logic step they use (naked single, hidden single, naked pair).
- **Newspaper entry**: type in a puzzle from a paper or book; the app checks it is solvable and then plays it like any other board.
- **Family profiles** with per-player history, daily streaks and a stats page, saved in the device's browser. A sync code copies everything to another device.
- **Comfort settings**: large digits, optional auto error check (turn it off for a "Purist" solve) and smart highlighting.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole game: markup, styles and script |
| `manifest.webmanifest`, `icons/` | Home-screen install name and icon |
| `sw.js` | Service worker that caches the page for offline play |

Tailwind CSS (compiled for this page) and Chart.js are inlined in `index.html`, so nothing is fetched from a CDN. The Inter font loads from Google Fonts when online and falls back to the system font otherwise.

## License

MIT, see [LICENSE](LICENSE).
