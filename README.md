# Family Sudoku

A single-file, ad-free classic Sudoku for a shared family iPad and everyone's phones. Open `index.html` in any browser, or host it on GitHub Pages and use *Add to Home Screen* on iPhone or iPad to install it as an app with its own icon.

## What it does

- **Four difficulties** (Easy, Medium, Hard, Master) generated on the device, each with a unique solution, plus a **Daily Family Board** that is the same puzzle for every player on a given day.
- **Pro notation**: pen, corner pencil notes and Snyder-style center notes, with auto-notes and a digit "sniper" highlight.
- **What-If Trail**: bookmark the board, try a hypothesis in teal ink, then rewind or commit.
- **Technique coach**: hints named after the logic step they use (naked single, hidden single, naked pair).
- **Newspaper entry**: type in a puzzle from a paper or book; the app checks it is solvable and then plays it like any other board.
- **Family profiles** with per-player history, streaks and a stats page, saved in the browser. A sync code moves everything between devices.
- **Comfort settings**: large digits, optional auto error check (turn it off for a "Purist" solve) and smart highlighting.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole game: markup, styles and script |
| `manifest.webmanifest`, `icons/` | Home-screen install name and icon |

Styling comes from the Tailwind CSS CDN and the stats chart from Chart.js on jsDelivr, so the page needs a network connection the first time it loads.

## License

MIT, see [LICENSE](LICENSE).
