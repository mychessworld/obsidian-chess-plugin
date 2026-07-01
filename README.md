# Chess PGN/FEN Viewer for Obsidian

[![Obsidian plugin](https://img.shields.io/badge/Obsidian-community%20plugin-7C3AED?style=flat-square&logo=obsidian)](https://obsidian.md/)
[![License](https://img.shields.io/badge/license-GPL--3.0-blue?style=flat-square)](package/Copying.txt)

An [Obsidian](https://obsidian.md/) community plugin that renders interactive chess boards from **PGN** games and **FEN** positions directly in your notes. Built on [@mliebelt/pgn-viewer](https://github.com/mliebelt/pgn-viewer), with optional **Stockfish 18** analysis: evaluation score, search depth, eval bar, and best-line suggestions.

**Author:** Sergei Dolganov — [mychessworld@yahoo.com](mailto:mychessworld@yahoo.com)

## Features

- **PGN code blocks** — full games with move list, navigation, and board controls
- **FEN code blocks** — single positions with a flip-board button
- **Custom starting positions** — start a PGN from any FEN via tags or a `fen:` header line
- **Stockfish analysis** (optional) — centipawn or mate score, depth, eval bar, and SAN variation line
- **Live re-analysis** — click moves in the notation to analyze the current position
- **Multi-board notes** — several boards on one page; analysis runs through a shared queue
- **Configurable board size** — 200–1000 px in plugin settings
- **Toggle analysis panel** — hide Stockfish entirely when you only need the board
- **Desktop and mobile** — boards and Stockfish on Windows, macOS, Linux, iOS, and Android
- **No extra setup** — Stockfish 18 is bundled inside `main.js`; no separate engine files or Node.js

## Requirements

- [Obsidian](https://obsidian.md/) 1.0 or newer
- **Reading view** or **Live Preview** (boards do not render in source mode)

Stockfish runs as **WebAssembly inside Obsidian** on desktop and mobile. No Node.js, no external engine files, and no `package/bin` folder are required.

## Installation

Copy **three files** into your vault:

```
<Vault>/.obsidian/plugins/chess-pgn-fen-viewer/
├── manifest.json
├── main.js
└── styles.css
```

### Steps

1. Download the [latest release](https://github.com/mychessworld/obsidian-chess-plugin/releases) or clone this repository and copy the three files above.
2. Open Obsidian → **Settings** → **Community plugins** → enable **Chess PGN-FEN Viewer**.
3. Reload Obsidian (**Ctrl+R** / **Cmd+R** on desktop; pull to refresh or restart the app on mobile).

`main.js` is large (~11 MB) because Stockfish 18 is embedded inside it. That is expected.

### Updating

Replace `main.js`, `styles.css`, and `manifest.json`, then reload Obsidian.

## Usage

### PGN — full game

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

Use a full FEN string (piece placement, side to move, castling, en passant, halfmove clock, fullmove number). When Black is to move, the board is shown with Black at the bottom.

## Stockfish evaluation panel

When **Stockfish evaluation** is enabled in settings, each board shows a panel below the move list:

| Element | Description |
|---------|-------------|
| **Score** | Centipawns or mate distance (from White’s perspective) |
| **Depth** | Search depth reached |
| **Eval bar** | Visual advantage indicator |
| **Line** | Best continuation in SAN (length configurable) |

Click any move in the notation to analyze that position. On notes with multiple boards, analysis runs **one board at a time** — extra boards may briefly show **Queued...** until their turn.

Turn off **Stockfish evaluation** in settings to hide the panel and stop the engine. Boards continue to work normally.

## Settings

**Settings → Community plugins → Chess PGN-FEN Viewer**

| Setting | Default | Description |
|---------|---------|-------------|
| Board Size | 500 px | Width and height of the chess board |
| Stockfish evaluation | On | Show or hide the analysis panel |
| Stockfish depth | 12 | Search depth (6–20); higher = stronger but slower |
| Stockfish hash | 16 MB | Engine hash table (8–128 MB; capped on mobile) |
| Variation length | 5 | Full moves shown in the best line (3–10) |
| Max boards per page | 5 | How many boards on one note receive analysis (1–10) |

Stockfish-specific settings are hidden when evaluation is turned off. Boards beyond the analysis limit still render; their panel shows a limit message.

## Troubleshooting

**Board does not appear**

- Switch to **Reading view** or **Live Preview**.
- Reload the note or Obsidian after installing the plugin.

**Stockfish stays on “Loading engine…”**

- The first run compiles WebAssembly — this can take **30–90 seconds** on desktop and up to **2 minutes** on mobile.
- Wait until the status changes to **Analyzing...** and then shows a score.

**Stockfish shows “Queued...” for a long time**

- Normal when a note has **several boards** — only one is analyzed at a time.
- Lower **Max boards per page** or **Stockfish depth** if the queue feels too slow.

**Stockfish shows an error**

- Confirm all three plugin files are present and up to date.
- Toggle **Stockfish evaluation** off and on in settings, then reload Obsidian.
- On desktop, open the developer console (**Ctrl+Shift+I** / **Cmd+Option+I**) and look for lines starting with `Stockfish:` (for example `Stockfish: starting browser WASM engine`, `Stockfish: analyzing position`).

## Repository layout

| Path | Purpose |
|------|---------|
| `manifest.json` | Plugin metadata for Obsidian |
| `main.js` | Plugin code, pgn-viewer, and embedded Stockfish 18 |
| `styles.css` | Board and evaluation panel styling |
| `package/Copying.txt` | GPLv3 license text for Stockfish (reference only; not required for install) |

## License & third-party components

- **Plugin code** — Sergei Dolganov ([mychessworld@yahoo.com](mailto:mychessworld@yahoo.com)).
- **[@mliebelt/pgn-viewer](https://github.com/mliebelt/pgn-viewer)** — chess board and PGN UI (bundled in `main.js`).
- **[Stockfish](https://stockfishchess.org/)** — chess engine (Stockfish 18 lite, embedded in `main.js`). Stockfish is **GPL v3**; see [`package/Copying.txt`](package/Copying.txt).

## Contributing

Issues and pull requests are welcome on [GitHub](https://github.com/mychessworld/obsidian-chess-plugin).

When reporting bugs, please include:

- Obsidian version and operating system (desktop or mobile)
- Steps to reproduce (PGN/FEN block content if possible)
- Relevant lines from the developer console or Obsidian log (messages starting with `Stockfish:`)
