# Family Sudoku

A single-file, ad-free classic Sudoku for a shared family iPad and everyone's phones. Play it at **https://playmaths.github.io/sudoku/**, or open `index.html` in any browser. On a phone or tablet use *Add to Home Screen*: the web manifest and service worker make it an installed app with its own icon that works offline.

## What it does

- **Built for playing**: the board, entry mode and number pad fit on one screen on a phone, an iPad in either orientation and a laptop. Difficulty, the daily board and paper-puzzle entry live behind the level button; candidate highlighting, input style and settings behind the ⋮ button, so nothing distracts from the grid.

- **Five difficulties** generated on the device in a background worker (the next board is pre-built while you play), each with a unique solution and graded by the techniques it actually needs rather than by clue count: Easy and Medium are solvable with singles alone, Hard needs pairs, triples, pointing pairs or box-line reductions, Master needs an X-Wing, Swordfish or Y-Wing, and Grandmaster needs an XY-Chain or Unique Rectangle, all of which the coach can explain. A **Daily Family Board** gives every player the same puzzle on a given day.
- **Pro notation**: pen, corner pencil notes and Snyder-style center notes, with auto-notes and a digit "sniper" highlight.
- **What-If Trail**: bookmark the board, try a hypothesis in teal ink, then rewind or commit.
- **Technique coach in three taps**: Look here (the cells involved light up), Explain (the technique and digits), Apply. Each tap costs a third of a hint. Techniques: naked and hidden singles, pairs and triples, pointing pairs, box-line reduction, X-Wing, Swordfish, Y-Wing, XY-Chain, Unique Rectangle. Placement hints fill one cell, elimination hints remove only the pencil marks the logic rules out, and in the rare position that needs a technique the coach does not know it says so and reveals one cell.
- **Newspaper entry**: type in a puzzle from a paper or book; the app checks it is solvable and plays it like any other board. A grid with several valid answers is accepted too and judged on clashes only.
- **Family profiles** with per-player history and daily streaks, saved in the device's browser straight away. A **family code** (Players → *New family code* on the first device, type it on the others) keeps the same players on every device through a small cloud database; each player's newer copy wins, so two people can play on two devices at once. Settings stay per device. A manual backup code covers devices with no internet.
- **A stats page that explains itself**: plain-sentence insights (getting faster at Hard, fewer clean solves lately, leaning on hints less), final time per game with one line per level or a running 5-game average for one level, recent form against the ten games before, a level-by-level table with best, last-5 average, clean rate and trend, techniques used through hints, the household board and the recent games.
- **Comfort settings**: large digits, optional auto error check (turn it off for a "Purist" solve, where nothing on screen gives a wrong digit away) and smart highlighting.
- **Scoring that bites on the hard levels**: every hint adds time to the recorded result (15 s Easy, 30 s Medium, 60 s Hard, 90 s Master), a wrong digit with red marking on adds 15 s on Medium, 30 s on Hard and 60 s on Master, and a third wrong digit on Hard or Master ends the board, which is recorded as lost and can be retried from the same clues. Turning red marking off is Purist mode: no checks, no penalties, and a clean solve earns the crown.
- **System, light and dark themes**, chosen under the tools button; System follows the device.
- **Replay** any recent game from the stats page: step through the moves with the engine saying whether each digit was right and which technique was available.
- **Challenge a board**: the win screen and the new-board sheet share a link that opens the exact same board for someone else; results are tagged with who sent it.
- **Today's board, This week and Achievements** cards on the stats page; CSV export of every game.
- **Solver's shortcuts**: long-press a pad digit to lock it and place it by tapping squares, long-press a filled square to light that digit everywhere, long-press an empty square to flip between pen and notes. A fourth entry mode paints squares in six colours for tracking chains. Settings for auto-removing candidates, starting boards with all candidates filled, and a stricter Hard band that demands at least four pair, triple or pointing steps.
- **Optional pencil-note slip check** for players who want their bookkeeping policed, haptic taps where the device supports them, keyboard and screen-reader labels on the board.
- **Honest timer**: counts wall-clock time while the board is on screen, pauses when the app is in the background, and survives a reload.

## Kids mode

A player can be marked as a kid when added (or later in the Players tab). Kids get their own ladder: **Little 4×4** (2×2 boxes), **Middle 6×6** with wide boxes, **Middle 6×6 tall** with tall boxes, **Bigger 8×8** (4×2 boxes) and **Big 9×9** with plenty of clues, all solvable with singles. The box shape is part of the board, so shared links and replays keep it. The timer is not shown (it is still recorded for the stats page), there are no penalties and no three-strike rule. A wrong number bounces back with a wobble and the row, column or box it clashes with lit up, so the rule is taught rather than enforced. Help is free and comes in two taps: *Help me* lights the squares to look at and says what to look for, *Show me* places the number and explains why in a child's words. A finished row, column or box glows, every finished board earns a sticker on a ten-sticker sheet, and the Players tab is behind a small multiplication question so a child cannot rename or remove anyone. Kids are ordinary players otherwise: same family code, same stats page (with a sticker-sheet card and a Kids filter), same daily board and shared links.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole game: markup, styles and script |
| `manifest.webmanifest`, `icons/` | Home-screen install name and icon |
| `sw.js` | Service worker that caches the page for offline play |

Tailwind CSS (compiled for this page) and Chart.js are inlined in `index.html`, so nothing is fetched from a CDN. The Inter font loads from Google Fonts when online and falls back to the system font otherwise.

## Architecture

[docs/architecture.html](docs/architecture.html) is a one-page diagram of how the pieces fit, drawn in the [diagram-design](https://github.com/cathrynlavery/diagram-design) format: the screens, the game state, the engine on the page and in its worker, the device store, the family document and the hosting. Open it at https://playmaths.github.io/sudoku/docs/architecture.html.

## Sync

Local first: every change is written to the browser at once and the game works offline. With a family code, the players are also kept in Firestore through its plain REST API, in the same `playmaths` project and `families/{code}` collection as Number Garden, as one document with two fields: `data` (JSON with `kind: "sudoku"`, `savedAt` and the players) and `savedAt`. A push happens 1.5 seconds after any change; a pull on open, when the app returns to the foreground and when the device comes back online. Each player carries `updatedAt` and the newer copy of each player wins, so different people on different devices never overwrite each other. Joining a code drops empty local players whose name matches a family player, so nobody appears twice. A code that holds a Number Garden document is refused. The web API key is a public identifier; access is governed by the Firestore rules, which allow one document per nine-character code with only those two fields.

## License

MIT, see [LICENSE](LICENSE).
