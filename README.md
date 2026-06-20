# Chess PGN/FEN Viewer for Obsidian

An [Obsidian](https://obsidian.md/) community plugin that renders interactive chess boards from **PGN** games and **FEN** positions in your notes. It is built on [@mliebelt/pgn-viewer](https://github.com/mliebelt/pgn-viewer) and includes an optional **Stockfish 18** engine panel for position evaluation, depth, eval bar, and best-line suggestions.

**Author:** Sergei Dolganov, e-mail: [mychessworld@yahoo.com](mailto:mychessworld@yahoo.com)

## Features

- **PGN code blocks** — full games with move list, navigation, and board controls
- **FEN code blocks** — single positions with a flip-board button
- **Custom starting positions** — PGN blocks can start from a FEN via tags or a `fen:` header line
- **Stockfish analysis** (optional) — score, depth, eval bar, and SAN variation line under each board
- **Live updates** — click moves in the notation to re-analyze the current position
- **Multi-board pages** — several boards on one note; analysis runs in a shared queue (configurable limit)
- **Adjustable board size** — 200–1000 px in plugin settings

## Installation

### Manual install (from GitHub)

1. Download or clone this repository.
2. Copy the entire plugin folder into your vault’s plugins directory:

   ```
   <Vault>/.obsidian/plugins/obsidian-chess-plugin/
   ```

3. Ensure the folder contains at least these files:

   ```
   obsidian-chess-plugin/
   ├── manifest.json
   ├── main.js
   ├── styles.css
   └── package/
       └── bin/
           ├── stockfish-18-lite-single.js
           └── stockfish-18-lite-single.wasm
   ```

4. Open Obsidian → **Settings** → **Community plugins**.
5. Enable **Chess PGN/FEN Viewer** (or reload plugins if it is already listed).
6. Install [Node.js](https://nodejs.org/) (LTS) if you want Stockfish analysis.
7. Reload Obsidian (**Ctrl+R** / **Cmd+R**) after copying updated files.

### Updating

Replace `main.js`, `styles.css`, `manifest.json`, and the `package/` directory with the new release, then reload Obsidian.

## Usage

### PGN — full game

Create a fenced code block with language `pgn`:

````markdown
```pgn
[Event "Example Game"]
[Site "?"]
[Date "????.??.??"]
[White "Alice"]
[Black "Bob"]
[Result "1-0"]

1. e4 e5 2. Nf3 Nc6 3. Bb5 a6 4. Ba4 Nf6 5. O-O Be7
```
````

Switch to **Reading view** or **Live Preview** to see the board. Use the move list and board controls to step through the game.

### PGN — custom starting position

**Option A — standard PGN tags:**

````markdown
```pgn
[FEN "r1bqkbnr/pppp1ppp/2n5/4p3/4P3/5N2/PPPP1PPP/RNBQKB1R w KQkq - 2 3"]
[SetUp "1"]

3. Bb5 a6 4. Ba4 Nf6
```
````

**Option B — `fen:` header line:**

````markdown
```pgn
fen: rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1

1. d4 d5 2. c4
```
````

### FEN — single position

````markdown
```fen
rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR b KQkq e3 0 1
```
````

Paste a full FEN string (placement, side to move, castling, en passant, halfmove, fullmove). Positions where Black moves first are shown with Black at the bottom.

## Stockfish evaluation panel

When enabled in settings, each board shows a panel below the move list with:

- **Score** — centipawns or mate distance (from White’s perspective)
- **Depth** — search depth reached
- **Eval bar** — visual advantage indicator
- **Line** — best continuation in SAN (length configurable)

Click any move in the notation to analyze that position. On pages with multiple boards, positions are analyzed **one at a time** through a shared queue so the engine stays responsive.

### Settings

Open **Settings → Community plugins → Chess PGN/FEN Viewer**:

| Setting | Default | Description |
|---------|---------|-------------|
| Board Size | 500 px | Width and height of the chess board |
| Stockfish evaluation | On | Toggle the analysis panel |
| Stockfish depth | 12 | Search depth (6–20); higher = stronger but slower |
| Stockfish hash | 16 MB | Engine hash table size (8–128 MB) |
| Variation length | 5 | Full moves shown in the best line (3–10) |
| Max boards per page | 5 | How many boards on one note receive analysis (1–10) |

Boards beyond the limit still render normally; their panel shows a limit message instead of running Stockfish.

## License & third-party components

- **Plugin code** — Sergei Dolganov, e-mail: [mychessworld@yahoo.com](mailto:mychessworld@yahoo.com); see repository license (if applicable).
- **[@mliebelt/pgn-viewer](https://github.com/mliebelt/pgn-viewer)** — chess board and PGN UI (bundled in `main.js`).
- **[Stockfish](https://stockfishchess.org/)** — chess engine (Stockfish 18 lite, WASM build in `package/bin/`). Stockfish is **GPL v3**; see `package/Copying.txt`.

## Contributing

Issues and pull requests are welcome on GitHub. When reporting bugs, include Obsidian version, OS, whether Node.js is installed, and relevant lines from the Obsidian log.
