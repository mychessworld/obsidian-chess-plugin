# Chess PGN/FEN Viewer for Obsidian (Desktop and Android)

An [Obsidian](https://obsidian.md/) community plugin that renders interactive chess boards from **PGN** games and **FEN** positions in your notes. It is built on [@mliebelt/pgn-viewer](https://github.com/mliebelt/pgn-viewer) and includes an optional **Stockfish 18** engine panel for position evaluation, depth, eval bar, and best-line suggestions.

**Author:** Sergei Dolganov  
**Contact:** [mychessworld@yahoo.com](mailto:mychessworld@yahoo.com)

## Features

- **PGN code blocks** — full games with move list, navigation, and board controls
- **FEN code blocks** — single positions with a flip-board button
- **Custom starting positions** — PGN blocks can start from a FEN via tags or a `fen:` header line
- **Stockfish analysis** (optional) — score, depth, eval bar, and SAN variation line under each board
- **Live updates** — click moves in the notation to re-analyze the current position
- **Multi-board pages** — several boards on one note; boards render in a queue on mobile; analysis runs in a shared queue (configurable limit)
- **Adjustable board size** — 200–1000 px in plugin settings

## Requirements

| Component | Required for | Notes |
|-----------|--------------|-------|
| Obsidian **0.15.0+** | Everything | Desktop and mobile |
| `package/bin/stockfish-18-lite-single.js` + `.wasm` | Stockfish analysis (optional) | Both files must be present in the plugin folder |
| **Node.js** (optional) | Stockfish on **desktop** | Not used on Android/iOS |

**You do not need to install Node.js to use this plugin.**

- **PGN/FEN boards** work on desktop and mobile with Obsidian alone.
- **Stockfish on desktop** — the plugin starts the bundled engine as a child process (Node.js subprocess when available, otherwise via Obsidian/Electron).
- **Stockfish on Android** — the bundled WASM engine runs in the app (main thread with a Worker fallback); no Node.js required.
- Installing [Node.js](https://nodejs.org/) (LTS) on desktop is **recommended** if Stockfish fails to start without it.

Stockfish analysis is slower on mobile devices than on a desktop PC. Lower **Stockfish depth** or **hash** in settings if analysis feels too heavy on a phone.

On **Android**, notes with many boards may load progressively as you scroll. Scroll through the full page and wait a few seconds so all boards can mount before expecting Stockfish analysis on every board.

## Installation

### Manual install (from GitHub)

1. Download or clone this repository.
2. Copy the entire plugin folder into your vault’s plugins directory:

   ```
   <Vault>/.obsidian/plugins/obsidian-chess-plugin/
   ```

3. Ensure the folder contains at least these files:

   obsidian-chess-plugin/
   ├── manifest.json
   ├── main.js
   ├── styles.css
   └── package/
       └── bin/
           ├── stockfish-18-lite-single.js
           └── stockfish-18-lite-single.wasm

4. Open Obsidian → **Settings** → **Community plugins**.
5. Enable **Chess PGN/FEN Viewer** (or reload plugins if it is already listed).
6. Reload Obsidian (**Ctrl+R** / **Cmd+R**) after copying updated files.

   Optional: install [Node.js](https://nodejs.org/) (LTS) if you use Stockfish analysis and the engine does not start without it.

### Updating

Replace `main.js`, `styles.css`, `manifest.json`, and the `package/` directory with the new release, then reload Obsidian.

## Usage

### PGN — full game

Create a fenced code block with language `pgn`:

```pgn
1. e4 e5 2. Nf3 (2. Nc3) Nc6 3. Bb5 a6 (Nf6) 4. Ba4 Nf6 {Good move} 5. O-O Be7
```

Switch to **Reading view** or **Live Preview** to see the board. Use the move list and board controls to step through the game.

### PGN — custom starting position

**Option A — standard PGN tags:**

```pgn
[FEN "r1bqkbnr/pppp1ppp/2n5/4p3/4P3/5N2/PPPP1PPP/RNBQKB1R w KQkq - 2 3"]
[SetUp "1"]

3. Bb5 a6 4. Ba4 Nf6
```

**Option B — `fen:` header line:**

```pgn
fen: rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1

1. d4 d5 2. c4
```

### FEN — single position

```fen
rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR b KQkq e3 0 1
```

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

## Troubleshooting

### Board does not appear

- Use language tags **`pgn`** or **`fen`** on the code block (not `chess` or plain text).
- View the note in **Reading view** or **Live Preview** (not Source-only mode).
- On **Android**, scroll to the code block — boards below the fold may render only after they become visible.

### Stockfish shows an error or never updates

**Desktop**

1. Confirm both Stockfish files exist in `package/bin/`.
2. Reload Obsidian after updating the plugin.
3. If analysis still fails, install [Node.js](https://nodejs.org/) (LTS) and verify `node --version` in a terminal.
4. On Windows, you can point to a custom Node path before starting Obsidian:
   ```powershell
   $env:OBSIDIAN_CHESS_NODE = "C:\Path\To\node.exe"
   ```
5. Check `obsidian.log` (Help → Open log folder) for lines starting with `Stockfish:`.

**Android / mobile**

1. Confirm the full plugin folder (including `package/bin/*.wasm`) is synced to the device.
2. Reload the note or restart Obsidian after installing the plugin.
3. Scroll through the page so all boards load, then wait 15–20 seconds for the first WASM startup.
4. If analysis is very slow, reduce **Stockfish depth** or **hash** in plugin settings.

### Analysis is slow on notes with many boards

This is expected: one engine instance analyzes positions serially. Reduce **Max boards per page**, lower **Stockfish depth**, or split games across multiple notes.

## Development

This repository ships a pre-built `main.js` bundle. The plugin registers two Markdown code block processors:

- `pgn` → interactive game viewer
- `fen` → static position board

After changing source or rebuilding, copy the updated files into `.obsidian/plugins/obsidian-chess-plugin/` and reload Obsidian.

## License & third-party components

- **Plugin code** — Sergei Dolganov ([mychessworld@yahoo.com](mailto:mychessworld@yahoo.com)); see repository license (if applicable).
- **[@mliebelt/pgn-viewer](https://github.com/mliebelt/pgn-viewer)** — chess board and PGN UI (bundled in `main.js`).
- **[Stockfish](https://stockfishchess.org/)** — chess engine (Stockfish 18 lite, WASM build in `package/bin/`). Stockfish is **GPL v3**; see `package/Copying.txt`.

## Contributing

Issues and pull requests are welcome on GitHub. When reporting bugs, include Obsidian version, OS (desktop or Android), whether Node.js is installed (desktop only), and relevant lines from the Obsidian log.

## Author

**Sergei Dolganov**  
Email: [mychessworld@yahoo.com](mailto:mychessworld@yahoo.com)
